# tbs-pipelines
Examples of using Tanzu Build Service in an application pipeline

# Create Service Account 

```
kubectl -n kube-system create serviceaccount azure-pipelines
```
```
kubectl create clusterrolebinding azure-pipelines-cb --clusterrole=cluster-admin --serviceaccount=kube-system:azure-pipelines
```


```
TOKENNAME=`kubectl -n kube-system get serviceaccount/azure-pipelines -o jsonpath='{.secrets[0].name}'`
```

```
TOKEN=`kubectl -n kube-system get secret $TOKENNAME -o jsonpath='{.data.token}'| base64 --decode`
```

```
CACRT=$(kubectl -n kube-system get secret $TOKENNAME -o jsonpath='{.data.ca\.crt}' | base64 --decode)
```


Create the kubeconfig file
```
kubectl config set-cluster demo --server=https://34.136.206.76
```

```
kubectl config set-credentials azure-pipelines --token=$TOKEN
```

```
kubectl config set-context demo --user=azure-pipelines --cluster=demo
```

```
kubectl config set-cluster demo --embed-certs --certificate-authority <(echo $CACRT)
```

```
kubectl config use-context demo
```