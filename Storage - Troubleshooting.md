**PVC Stuck in Terminating Status**

`kubectl patch pvc {PVC_NAME} -p '{"metadata":{"finalizers":null}}'`
