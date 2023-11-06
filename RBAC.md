# Overview
3 key building blocks
1. Subject - User or process requiring access
2. Resource - K8s API resource type
3. Verb - Operation that can be executed

# Users and Groups
No 'user' API object

"Generate" a user:
``` bash
openssl genrsa -out johndoe.key 2048

# generate csr
openssl req -new -key johndoe.key -out johndoe.csr -subj "/CN=johndoe/O=cka-study-guide"

# sign csr with cluster CA
sudo openssl x509 -req -in johndoe.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out johndoe.crt -days 364

#Creat the user in kubernetes by setting entry in kubeconfig for user and pint to CRT and key file
kubectl config set-credentials johndoe \
--client-certificate=johndoe.crt --client-key=johndoe.key

# Set the context
kubectl config set-context johndoe-context --cluster=kubernetes-admin@kubernetes --user=johndoe
```
``

# Service Accounts
- Any pod that doesn't explicitly assign a service account uses the default service account.
- Since 1.24, secrets are not automatically created with a service account. When a SA is assigned to a Pod, a temporary secret is auto mounted into the pod.
```bash
kubectl create token <SA>
```

- To assign SA to a pod:
``` bash
kubectl run build-observer --image=alpine --restart=Never \
--serviceaccount=build-bot
```

```yaml
apiVersion: v1
kind: Pod
metadata:
name: build-observer
spec:
serviceAccountName: build-bot
```

- To create a secret explicitly for SA:
```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: "build-bot"
```
# RBAC API Primitives
**Role**
The API resource and the Operations allowed on them
**RoleBinding**
Links the Roel to a Subject




# RBAC Scopes - NS and Cluster


# RBAC Aggregation