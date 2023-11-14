**Deployment Primitive**
**ReplicaSet Primitive**
**Rollout/Rollback**

----

*Deployments*: 
- Abstract away ReplicaSet functionality and can track application versions
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-cache
  namespace: default
  labels:
    app: app-cache 
spec:
  selector:
    matchLabels:
      app: app-cache # Matchy… Matchy
  replicas: 4
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: app-cache # Matchy
    spec:
      containers:
        - name: memcached
          image: memcached:latest

```