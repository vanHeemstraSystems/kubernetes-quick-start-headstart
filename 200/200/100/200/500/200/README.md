# 200 - Reliability
In most cases, the node controller limits the eviction rate to ```--node-eviction-rate``` (default 0.1) per second, meaning it won't evict pods from more than 1 node per 10 seconds.

The node eviction behavior changes when a node in a given availability zone becomes unhealthy. The node controller checks what percentage of nodes in the zone are unhealthy (NodeReady condition is ConditionUnknown or ConditionFalse) at the same time:

- If the fraction of unhealthy nodes is at least ```--unhealthy-zone-threshold``` (default 0.55), then the eviction rate is reduced.
- If the cluster is small (i.e. has less than or equal to ```--large-cluster-size-threshold``` nodes - default 50), then evictions are stopped.
- Otherwise, the eviction rate is reduced to ```--secondary-node-eviction-rate``` (default 0.01) per second.

The reason these policies are implemented per availability zone is because one availability zone might become partitioned from the master while the others remain connected. If your cluster does not span multiple cloud provider availability zones, then there is only one availability zone (i.e. the whole cluster).

A key reason for spreading your nodes across availability zones is so that the workload can be shifted to healthy zones when one entire zone goes down. Therefore, if all nodes in a zone are unhealthy, then the node controller evicts at the normal rate of ```--node-eviction-rate```. The corner case is when all zones are completely unhealthy (i.e. there are no healthy nodes in the cluster). In such a case, the node controller assumes that there is some problem with master connectivity and stops all evictions until some connectivity is restored.

The node controller is also responsible for evicting pods running on nodes with ```NoExecute``` taints, unless those pods tolerate that taint. The node controller also adds taints corresponding to node problems like node unreachable or not ready. This means that the scheduler won't place Pods onto unhealthy nodes.
