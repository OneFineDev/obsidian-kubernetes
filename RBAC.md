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
```
``


# Service Accounts



# RBAC API Primitives


# Roles and RoleBindings


# RBAC Scopes - NS and Cluster


# RBAC Aggregation