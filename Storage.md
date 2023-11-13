- **Persistent Volumes**
- **Static and Dynamic Provisioning**
- **Storage Classes**
- **Configuration of Persistent Volumes**
- **Persistent Volume Claims**
- **Mounting Persistent Volumes in a Pod**

----

## Volumes
**Types:**

| Type              | Description                    | Lifecycle  |
| ----------------- | ------------------------------ | ---------- |
| emptyDir          | Empty directory in pod         | Pod        |
| hostPath          | File or Dir from the host      | Host       |
| configMap, secret | Config data mounted into a pod | Object     |
| nfs               | Network file share             | persistent |
| PVC               | Claim on a persistent volume   | Volume     |

```yaml
apiVersion: v1
kind: Pod
metadata:
name: business-app
spec:
volumes:
- name: logs-volume # Volume stated in spec
emptyDir: {}
containers:
- image: nginx
name: nginx
volumeMounts:
- mountPath: /var/logs  # Mountpath provided
name: logs-volume
```

## Persistent Volumes

POD  ---> PVC  ---> PV

**Static:** Manually provisioned volume
**Dynamic**: PVC creates volume via Storage Class

*Statically provisioned volume on Node*
``` yaml
apiVersion: v1
kind: PersistentVolume
metadata:
	name: db-pv
spec:
	capacity:
		storage: 1Gi
	accessModes:
		- ReadWriteOnce
	hostPath:
		path: /data/db
```

**Configuration Options:**
- *Volume Mode*: Handles the type of device (filesystem or a block device)

| Type       | Description                                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------- |
| Filesystem | Mounts volume in a dir of the consuming pod. Creates a filesystem first if backed by block and the device is empty. |
| Block      | Used for a volume as a raw block dev with no fs.                                                                    |

- *Access mode*: How many pods can read/write

| Type             | Shorthand | Description                      |
| ---------------- | --------- | -------------------------------- |
| ReadWriteOnce    | RWO       | R/W by single node               |
| ReadOnlyMany     | ROX       | R-Only, many nodes               |
| ReadWriteMany    | RWX       | R/W many nodes                   |
| ReadWriteOncePod | RWOP      | R/W access mounted by single pod | 

- Reclaim Policy: What should happen to Volume once claim has gone

| Type    | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| Retain  | Default. When claim is deleted, PV is released and can be reclaimed |
| Delete  | PV gets nuked                                                       |
| Recycle | DEPRECATED                                                          | 

## Persistent Volume Claim

``` yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
	name: db-pvc
spec:
	accessModes:
		- ReadWriteOnce  # Previous PV satisfies this
	storageClassName: ""
	resources:
		requests:
			storage: 256Mi # Is available in the pv
```

## Mounting the PVC
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-consuming-pvc
spec:
  volumes:
    - name: app-storage # give the PVC a name in the pod volume 
      persistentVolumeClaim:
        claimName: db-pvc #Specify the PVC to mount it
  containers:
    - image: alpine
      name: app
      command: ["/bin/sh"]
      args: ["-c", "while true; do sleep 60; done;"]
      volumeMounts:
        - mountPath: "/mnt/data" # location to mount the PV assigned through the PVC
          name: app-storage 
```

