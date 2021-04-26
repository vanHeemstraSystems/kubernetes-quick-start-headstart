# 200 - Conditions
The ```conditions``` field describes the status of all ```Running``` nodes. Examples of conditions include:

| Node Condition  | Description |
| ------------- | ------------- |
| Ready  | ```True``` if the node is healthy and ready to accept pods, ```False``` if the node is not healthy and is not accepting pods, and Unknown if the node controller has not heard from the node in the last ```node-monitor-grace-period``` (default is 40 seconds)  |
| DiskPressure  | ```True``` if pressure exists on the disk size--that is, if the disk capacity is low; otherwise ```False```  |
| MemoryPressure  | ```True``` if pressure exists on the node memory--that is, if the node memory is low; otherwise ```False```  |
| PIDPressure  | ```True``` if pressure exists on the processes—that is, if there are too many processes on the node; otherwise ```False```  |
| NetworkUnavailable  | ```True``` if the network for the node is not correctly configured, otherwise ```False```  |

***Note***: If you use command-line tools to print details of a cordoned Node, the Condition includes SchedulingDisabled. SchedulingDisabled is not a Condition in the Kubernetes API; instead, cordoned nodes are marked Unschedulable in their spec.

The node condition is represented as a JSON object. For example, the following structure describes a healthy node:

```
"conditions": [
  {
    "type": "Ready",
    "status": "True",
    "reason": "KubeletReady",
    "message": "kubelet is posting ready status",
    "lastHeartbeatTime": "2019-06-05T18:38:35Z",
    "lastTransitionTime": "2019-06-05T11:41:27Z"
  }
]
```

If the Status of the Ready condition remains ```Unknown``` or ```False``` for longer than the ```pod-eviction-timeout``` (an argument passed to the kube-controller-manager), then all the Pods on the node are scheduled for deletion by the node controller. The default eviction timeout duration is ***five minutes***. In some cases when the node is unreachable, the API server is unable to communicate with the kubelet on the node. The decision to delete the pods cannot be communicated to the kubelet until communication with the API server is re-established. In the meantime, the pods that are scheduled for deletion may continue to run on the partitioned node.

The node controller does not force delete pods until it is confirmed that they have stopped running in the cluster. You can see the pods that might be running on an unreachable node as being in the ```Terminating``` or ```Unknown``` state. In cases where Kubernetes cannot deduce from the underlying infrastructure if a node has permanently left a cluster, the cluster administrator may need to delete the node object by hand. Deleting the node object from Kubernetes causes all the Pod objects running on the node to be deleted from the API server and frees up their names.

The node lifecycle controller automatically creates taints that represent conditions. The scheduler takes the Node's taints into consideration when assigning a Pod to a Node. Pods can also have tolerations which let them tolerate a Node's taints.

See Taint Nodes by Condition for more details.
