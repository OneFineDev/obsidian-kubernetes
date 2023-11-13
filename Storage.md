- **Persistent Volumes**
- **Static and Dynamic Provisioning**
- **Storage Classes**
- **Configuration of Persistent Volumes**
- **Persistent Volume Claims**
- **Mounting Persistent Volumes in a Pod**

----

## Persistent Volumes
**Types:**
| Type              | Description                    | Lifecycle |
| ----------------- | ------------------------------ | --------- |
| emptyDir          | Empty directory in pod         | Pod       |
| hostPath          | File or Dir from the host      | Host      |
| configMap, secret | Config data mounted into a pod | Object    |
| nfs               |                                |           |
