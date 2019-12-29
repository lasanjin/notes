[⌫ back](../KUBERNETES.md)

## Google Cloud Platform

### 1.1 [Install Google Cloud SDK](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)
1. Add the Cloud SDK distribution URI as a package source
```
$ echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```

2. Import the Google Cloud Platform public key
```
$ curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
```

3. Update the package list and install the Cloud SDK
```
$ sudo apt-get update && sudo apt-get install google-cloud-sdk
```

### 1.2 Initialize the SDK
```
$ sudo gcloud init
```
- Manage OAuth2 ([gcloud auth](https://cloud.google.com/sdk/gcloud/reference/auth/)) credentials for the Google Cloud SDK

### 1.3 Create a cluster
 - [gcloud](https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster?refresh=1)
 - [console](https://console.cloud.google.com/kubernetes/)

### 1.4 Enable GCP APIs for GKE
 - Kubernetes Engine API
 - Cloud Resource Manager API
   - Crucial for continuous delivery with GitLab CI
 - Identity and Access Management (IAM) API	

### Cross Namespace communication 
 - Services in Kubernetes expose their endpoint using a common DNS pattern
```
<service name>.<namespace name>.svc.cluster.local:PORT
```
- [Best practices](https://cloud.google.com/blog/products/gcp/kubernetes-best-practices-organizing-with-namespaces)