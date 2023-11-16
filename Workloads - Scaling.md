- **Manual Scaling**
- **Horizontal Pod Auto Scaler**

## Commands

`kubectl get hpa`
`kubectl describe hpa`

## Manual Scaling
**Deployments**
- Can either edit the live object:
	`edit deployment`
- Or use the imperative command:
	`kubectl scale deployment app-cache --replicas=6`

**StatefulSet**
- use the imperative command:
	`kubectl scale statefulset app-cache --replicas=6`

##  AutoScaling

**Horizontal Pod Autoscalers - V1** 
Autoscaling based on CPU resource only
`kubectl autoscale deployment app-cache --cpu-percent=80 --min=3 --max=5`

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: app-cache
spec:
  maxReplicas: 5
  minReplicas: 3
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app-cache
  targetCPUUtilizationPercentage: 80
```

For HPA to work, the Pods must define resource requirements:
``` yaml
# …
spec:
# …
	template:
# …
		spec:
		containers:
			- name: memcached
# …
			  resources:
				requests:
				  cpu: 250m
				limits:
				  cpu: 500m
```

**Horizontal Pod Autoscalers - V2**
Autoscaling based on CPU or memory
``` yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-cache
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app-cache
  minReplicas: 3
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
  - type: Resource
    resource:
      name: memory
      target:
        type: AverageValue
        averageValue: 500Mi
```