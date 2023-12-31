
**Node Affinity**: a property of [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) that _attracts_ them to a set of [nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) (either as a preference or a hard requirement).
**Taints**: the opposite -- they allow a node to repel a set of pods.
**Tolerations**: applied to pods. Tolerations allow the scheduler to schedule pods with matching taints. Tolerations allow scheduling but don't guarantee scheduling: the scheduler also [evaluates other parameters](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) as part of its function.

