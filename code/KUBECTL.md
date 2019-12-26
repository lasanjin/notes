[⌫ back](KUBERNETES.md)

## kubectl

### 2.1 [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
1. Download the latest release
```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```

2. Make the kubectl binary executable
```
$ chmod +x ./kubectl
```

1. Move the binary in to PATH
```
$ sudo mv ./kubectl /usr/local/bin/kubectl
```

### 2.2 Connect kubectl to you cluster 
```
$ gcloud container clusters get-credentials <cluster-name> --zone <zone-name> --project <project-id>
```

### Commands
- [Cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete)
- [Managing Resources](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/)

`kubectl get $1 --all-namespaces -o wide`\
`$1:`
 - deployments
 - pods
 - services
 - secrets
 - ...

`kubectl logs <pod-name>`\
`kubectl describe certificate`\
`kubectl get secret <secret-name> -o yaml`\
`sudo kubectl exec <pod name> -- ls -la`