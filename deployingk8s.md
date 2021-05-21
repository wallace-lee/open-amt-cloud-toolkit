Prerequisites

- Docker Desktop w/ kubectl
- Helm CLI     

# Local k8s Deployment
1) Create secrets 

If you are using a private docker registry, you'll need to provide your credentials to K8S. 
```
kubectl create secret docker-registry registrycredentials --docker-server=<your-registry-server> 
--docker-username=<your-username> 
--docker-password=<your-password>
```

MPS/KONG JWT Token

```
kubectl create secret generic open-amt-admin-jwt --from-literal=kongCredType=jwt --from-literal=key="admin-issuer" --from-literal=algorithm=HS256 --from-literal=secret="<your-secret>"
```
KONG ACL for JWT Token
```
kubectl create secret generic open-amt-admin-acl --from-literal=kongCredType=acl --from-literal=group=open-amt-admin
```

MPS Web Username and Password
```
kubectl create secret generic mpsweb --from-literal=user=admin --from-literal=password=G@ppm0ym
```

MPS Username and Password
```
kubectl create secret generic mps --from-literal=user=admin --from-literal=password=G@ppm0ym
```


2) Deploy Open AMT Cloud Toolkit using Helm

Deploying using Helm
```
helm install openamtstack ./charts
```

3) Initialize and Unseal Vault

Use the following instructions to initialize unseal and vault:  [https://learn.hashicorp.com/tutorials/vault/kubernetes-azure-aks?in=vault/kubernetes#initialize-and-unseal-vault](https://learn.hashicorp.com/tutorials/vault/kubernetes-azure-aks?in=vault/kubernetes#initialize-and-unseal-vault)

4) Once you have your vault token, we'll add the root token as a secret to the k8s cluster so the services can access it.

Vault Token:
```
kubectl create secret generic vault --from-literal=vaultKey=<your-root-token>
```

5) Continue from getting started steps [here](https://open-amt-cloud-toolkit.github.io/docs/1.3/General/loginToRPS/)

