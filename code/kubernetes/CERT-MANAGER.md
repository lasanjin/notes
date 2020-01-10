[⌫ back](../KUBERNETES.md)

## Cert Manager
Cert-manager is a native Kubernetes certificate management controller. It can help with issuing certificates from Let’s Encrypt.

 - [Documentation](https://cert-manager.io/docs/)
   - [Rate limits](https://letsencrypt.org/docs/rate-limits/)
 - Output
   - `acme-issuer`
   - `acme-certificate`
 - This guide assumes that a cluster is hosted on GCP and that it already has a domain set up with CloudDNS

</br>

### Concepts
   - An [Issuer](https://cert-manager.io/docs/concepts/issuer/) is created in order to configure cert-manager to begin issuing [certificates](https://cert-manager.io/docs/concepts/certificate/)
   - Cert-manager supports requesting certificates from [ACME](https://cert-manager.io/docs/configuration/acme/) servers ([Let’s Encrypt](https://letsencrypt.org/)) with use of the ACME Issuer
     - ACME Issuer represents a single account registered with the Automated Certificate Management Environment (ACME) Certificate Authority server
         - When a ACME Issuer is created, cert-manager generates a private key which is used for identification with the ACME server
     - To successfully request a certificate, cert-manager must solve ACME Challenges which are completed in order to prove that the client owns the DNS addresses that are being requested. 
       - This ensures clients are unable to request certificates for domains they do not own and as a result, fraudulently impersonate another’s site
- Cert-manager offers two challenge validations: [HTTP01](https://cert-manager.io/docs/configuration/acme/http01/) and [DNS01](https://cert-manager.io/docs/configuration/acme/dns01/) challenges.
  - With a DNS01 challenge, ownership of a domain is proven by proving control of its DNS records
    - This is done by creating a TXT record with specific content that proves you control of the domains DNS records

</br>

### 1. [Install Cert Manager on Kubernetes](https://docs.cert-manager.io/en/latest/getting-started/install/kubernetes.html)
1. Create a namespace to run cert-manager in
```
$ kubectl create namespace cert-manager
```

2. Install the CustomResourceDefinitions, cert-manager and the webhook component (included in a single YAML manifest file)
```
$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.12.0/cert-manager.yaml
```

- May encounter a ‘permission denied’ error on GKE 
  - Run command as ‘cluster-admin’
```
$ kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=$(gcloud config get-value core/account)
```

3. Verify installation
```
$ kubectl get pods --namespace cert-manager
```

</br>

### 2. [Issuing an ACME certificate using DNS validation](https://docs.cert-manager.io/en/latest/tutorials/acme/dns-validation.html)


1. Set up a service account
   - Create a GCP service account with `dns.admin` role in order to enable cert-manager to add records to CloudDNS for solving the DNS01 challenge
      - [gcloud](https://cert-manager.io/docs/configuration/acme/dns01/google/)
      - [console](https://console.cloud.google.com/iam-admin/)
```
$ gcloud iam service-accounts create dns01-solver --display-name "dns01-solver"
```
```
$ gcloud projects add-iam-policy-binding <PROJECT ID> \
--member serviceAccount:dns01-solver@<PROJECT ID>.iam.gserviceaccount.com \
--role roles/dns.admin
```

2. Create a key for the service account and download it as a JSON file
3. Create a service account [secret](https://kubernetes.io/docs/concepts/configuration/secret/) named `acme-issuer-secret`
   - Cert-manager uses a key stored in a Kubernetes secret to access this service account
   - NOTE
     - When a secret being already consumed in a volume is updated, projected keys are eventually updated as well
     - A container using a Secret as a subPath volume mount will not receive secret updates
```
$ gcloud iam service-accounts keys create clouddns-key.json \
--iam-account dns01-solver@<PROJECT ID>.iam.gserviceaccount.com
```

```
$ kubectl create secret generic clouddns-dns01-solver-svc-acct \
--from-file=clouddns-key.json
```

</br>

### 4. Create a file named `acme-certificate.yaml`
```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: acme-certificate
  namespace: default
spec:
  #If successful, the resulting key and certificate will be 
  #stored in a secret with keys of tls.key and tls.crt respectively
  secretName: acme-certificate-secret
  #Automatic renewal every 90 days (2160h)
  renewBefore: 1920h #Renew every 10th day
  dnsNames:
  - example.com
  - www.example.com
  issuerRef:
    name: acme-issuer
    kind: Issuer
  acme:
    config:
    - dns01:
        provider: clouddns
      domains:
      - example.com
      - www.example.com
```

</br>

### 5. Create a file named `acme-issuer.yaml`
```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Issuer
metadata:
  name: acme-issuer
  namespace: default
spec:
  acme:
    #For testing: https://acme-staging-v02.api.letsencrypt.org/directory
    server: https://acme-v02.api.letsencrypt.org/directory
    email: <EMAIL>
    #Name of a secret used to store the ACME account private key
    #Not the same as certificate secret used to save ssl keys
    privateKeySecretRef:
      name: acme-issuer-secret
    #ACME DNS-01 provider configurations
    dns01:
      #List of DNS-01 providers that can solve DNS challenges
      providers:
        - name: clouddns
          clouddns:
            #Secret used to access the service account
            serviceAccountSecretRef:
              name: clouddns-secret
              key: clouddns-key.json
            #Project ID in which to update the DNS zone
            project: <PROJECT ID>
```

 - Cloudflare configurations (nice solution outside GCP)
```yaml
dns01:
  providers:
    - name: cf-dns
      cloudflare:
        email: <EMAIL>
        #Secret for a cloudflare api key
        apiKeySecretRef:
          name: cloudflare-api-key
          key: api-key.txt
```
