- **Manual Scaling**
- **Horizontal Pod Auto Scaler**

## Commands



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

**Horizontal Pod Autoscalers**
`kubectl autoscale deployment app-cache --cpu-percent=80 --min=3 --max=5`