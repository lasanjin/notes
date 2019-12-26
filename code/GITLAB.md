[⌫ back](KUBERNETES.md)

## Automate Spring Boot application build, test and deployment on Kubernetes cluster
 - [Source](https://about.gitlab.com/blog/2016/12/14/continuous-delivery-of-a-spring-boot-application-with-gitlab-ci-and-kubernetes/)
 - Output
   - `Dockerfile`
   - `demo-deployment.yaml`
   - `demo-service.yaml`
   - Gitlab CI/CD environment variables
   - `.gitlab-ci.yml.yml`

</br>

### 1. Create a GitLab project and a Spring Boot application
 - [Gitlab](https://gitlab.com/projects/new)
 - [Spring Boot](https://start.spring.io/)
 - [Add existing Kubernetes cluster to Gitlab project](https://gitlab.com/help/user/project/clusters/index.md#adding-an-existing-kubernetes-cluster)

</br>

### 2. Package a Spring Boot application as a Docker container
 -  Create a file named `Dockerfile` in the root directory of the project
```docker
FROM maven:3-jdk-8-alpine
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN mvn package
ENV PORT 9000
EXPOSE $PORT
CMD [ "sh", "-c", "mvn -Dserver.port=${PORT} spring-boot:run" ]
```

</br>

### 3. Define the Kubernetes deployment
 - Create a file named `demo-deployment.yml` in the root directory of the project

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  namespace: default
  name: demo
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: demo
        tier: backend
    spec:
      containers:
      - name: demo
        image: registry.gitlab.com/<PATH> #Docker image
        imagePullPolicy: Always
        ports:
        - containerPort: 80
      imagePullSecrets:
          #Used to authenticate to the GitLab container registry
        - name: registry.gitlab.com 
```

</br>

### 4. Define a service for the Kubernetes deployment
 - Create a file named `demo-service.yml` in the root directory of the project
   - Provides an internally accessible IP for the Spring Boot service

```yaml
apiVersion: v1
kind: Service
metadata:
    name: demo-service
spec:
  selector:
    app: demo
    tier: backend
  ports:
  - protocol: TCP
    port: 9000 #Port to Spring Boot app runing on pod 
    targetPort: 9000
```

</br>

### 5. Setup Gitlab CI/CD environment variables
 - Turn on Auto-Devops to run our pipeline on each checkin to the repository
   - `Settings -> CI/CD -> Auto DevOps`
 - Create variables
   - `Gitlab -> Settings -> CI/CD -> Environment Variables `
     - `GOOGLE_KEY`
       1. Go to cloud console for the project and select `IAM - Admin - Service Account` 
       2. Create service account and give this account permissions
            - `Compute Engine admin` 
              - In order to update the cluster as part of the pipeline
            - `Kubernetes Engine API`
            - `Cloud Resource Manager API`
              - Crucial for continuous delivery with GitLab CI
            - `Identity and Access Management (IAM) API`	
       1. Create key, select JSON and save the file locally
       2. Paste the entire contents of the JSON file into the box next to the variable and save
          - Enable the pipeline in Gitlab to execute commands against the Kubernetes cluster. It represents a service account in Google Cloud Platform which is authorized to do specific actions relative to the Kubernetes cluster
     - `$REGISTRY_USER`
        - Username to the Gitlab repository where the Docker containers are stored
     - `REGISTRY_PASSWD`
        - Password to the Gitlab repository
     - `REGISTRY_EMAIL`
        - Email to the Gitlab account

</br>

### 6. Create the GitLab CI pipeline
 - Create and add a file named `.gitlab-ci.yml` to the root directory of the repository
   - [Documentation](https://docs.gitlab.com/ee/ci/yaml/)
   - This file is used by [GitLab Runners](https://docs.gitlab.com/ee/ci/runners/README.html) to manage the project's builds and deployments


```yaml
image: docker:latest
services:
  - docker:dind

variables:
  DOCKER_DRIVER: overlay
  SPRING_PROFILES_ACTIVE: gitlab-ci

stages:
  - build
  - package
  - deploy

maven-build:
  image: maven:3-jdk-8
  stage: build
  script: "mvn package -B"
  artifacts:
    paths:
      - target/*.jar

docker-build:
  stage: package
  script:
  - docker build -t registry.gitlab.com/<PATH> .
  - docker login -u gitlab-ci-token -p $CI_BUILD_TOKEN registry.gitlab.com
  - docker push registry.gitlab.com/<PATH>

k8s-deploy:
  image: google/cloud-sdk
  stage: deploy
  script:
  - echo "$GOOGLE_KEY" > key.json
  - gcloud auth activate-service-account --key-file key.json
  - gcloud config set compute/zone <ZONE>
  - gcloud config set project <PROJECT ID>
  - gcloud config set container/use_client_certificate False 
  - gcloud container clusters get-credentials <CLUSTER NAME>
  - kubectl delete secret registry.gitlab.com --ignore-not-found=true 
  - kubectl create secret docker-registry registry.gitlab.com --docker-server=https://registry.gitlab.com --docker-username=$REGISTRY_USER --docker-password=$REGISTRY_PASSWD --docker-email=$REGISTRY_EMAIL
  - kubectl apply -f demo-deployment.yml --namespace=<NAMESPACE NAME>
```

- **`stages`**
  - Defines the lifecycle of the build
  - Each job us associated with one [stage](https://docs.gitlab.com/ee/ci/yaml/#stages)
    - All jobs within a stage are run in parallel and stages are triggered sequentially in the order they are defined
      - The next stage is initiated only when the previous one is complete
- **`variables`**
  - `DOCKER_DRIVER` signals the Docker Engine which storage driver to use
    - `overlay` is used for [performance reasons](https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#using-the-overlayfs-driver)
  - `SPRING_PROFILES_ACTIVE` activates Spring profiles which provides a way to segregate parts of the application configuration and make it available only in certain environments
- **`maven-build`**
  - A job definition to perform a Maven build
  - Specifies `build` as the stage of this job
    - Jobs associated with the same stage run concurrently
      - Useful if there is a need to cross-compile the application
  - [script](https://docs.gitlab.com/ee/ci/yaml/#script) is executed by the GitLab Runner 
    - `mvn package -B` triggers a non-interactive Maven build up to the package phase, specific to the Maven build lifecycle
      - It includes the validate, compile and test phases
        - Tests are to be included in the src/test/java folder
  - [artifacts](https://docs.gitlab.com/ee/ci/yaml/#artifacts) is specified to persist the executable JAR and share it across jobs  
    - These are files or directories that are attached to the build after success and made downloadable from the UI in the Pipelines screen
- **`docker-build`**
  - Packages the application into a Docker container
  - Defined as the `package` stage since the maven-build job needs to produce the executable JAR beforehand
  - Images are pushed to the GitLab Container Registry
  - `$CI_BUILD_TOKEN` is a pre-defined variable which is injected by GitLab CI into the build environment automatically 
    - Used to log in to the GitLab Container Registry
- **`k8-deploy`**
  - Responsible for deploying the application to the Google Kubernetes Engine 
  - Use the `google/cloud-sdk` image since it comes preloaded with gcloud and all components and dependencies of the Google Cloud SDK
  - Chose `deploy` as the stage since the application needs to be packaged beforehand and its container pushed to the GitLab Container Registry
  - `echo "$GOOGLE_KEY" > key.json` 
    - Injects the Google Cloud service account key in the container 
  - `gcloud auth activate-service-account --key-file key.json`
    - Performs the non-interactive authentication process
  - `The gcloud config set ...`
    - Selects the target project, zone and cluster
    - `... container/use_client_certificate False`
      - Configures kubectl to use the service account's credentials
      - `True` 
        - Makes kubectl use your cluster's client cert or basic-auth
      - `False` 
        - Configures kubectl to use an OAuth2 access token from whatever Google identity is currently configured (either a logged in user from gcloud auth login or a service account)
  - `kubectl delete secret ...`
    - Deletes secret before it creates one and ignores error if there is none to delete
  - `kubectl create secret ...`
    - Creates the imagePullSecret defined in the `demo-deployment.yml`
      - There is also the risk of interference between pipelines because of the kubectl delete command
      - Secrets should be rotated periodically for security reasons and updated in each GitLab project using thee
      - It is recommended to handle Kubernetes secrets that can affect multiple pipelines in a separate pipeline or through configuration management tools

### [GitLab Environments](https://docs.gitlab.com/ee/ci/environments.html)
- Enables track environments and deployments
  - By committing on master and production we trigger a pipeline per environment 
```yaml
k8s-deploy-staging:
  image: google/cloud-sdk
  stage: deploy
  script:
  - ...
  environment:
    name: staging
    url: https://example.staging.com
  only:
  - master

k8s-deploy-production:
  image: google/cloud-sdk
  stage: deploy
  script:
  - ...
  environment:
    name: production
    url: https://example.production.com
  when: manual
  only:
  - production
```

 - **`environment`** 
   - Associates the job with a specific environment 
 - **`url`** 
   - Used to generate a hyperlink to our application on the GitLab Environments page
 - **`when`**
   - Used to turn the job execution to `manual` 
     - `automatic` implies [Continuous Deployment](https://about.gitlab.com/blog/2016/08/05/continuous-integration-delivery-and-deployment-with-gitlab/#continuous-deployment) rather than [Continuous Delivery](https://about.gitlab.com/blog/2016/08/05/continuous-integration-delivery-and-deployment-with-gitlab/#continuous-delivery)
     - From a Kubernetes perspective, it is making use of namespaces to segregate the different environments
