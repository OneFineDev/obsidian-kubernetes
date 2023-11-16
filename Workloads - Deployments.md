**Deployment Primitive**
**ReplicaSet Primitive**
**Rollout/Rollback**

## Commands
`kubectl set image deployment app-cache memcached=memcached:1.6.10 --record`
`kubectl edit deployment`
`kubectl rollout status`
`kubectl rollout history`

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

## Rollout, Rollback

`kubectl rollout undo deployment app-cache --to-revision=1`

## Strategy
**RollingUpdate**
- Slowly swaps out pods
- Max surge: proportion of pods that can exist above the replica number during an update (i.e. 1/4 above)
- Max unavailable, same

**Recreate**
- fully removes all pods, then adds the new versions

**Blue/Green**
- Not built in, but can be manually achieved using a separate deployment for Blue and Green, and a LoadBalancer service with the appropriate labels to switch between.
- To rollback, recreate LB pointing at previous service.
