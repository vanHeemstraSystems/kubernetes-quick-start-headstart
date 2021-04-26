# 500 - Node Controller
The node controller is a Kubernetes control plane component that manages various aspects of nodes.

The node controller has multiple roles in a node's life. The first is assigning a CIDR block to the node when it is registered (if CIDR assignment is turned on).

The second is keeping the node controller's internal list of nodes up to date with the cloud provider's list of available machines. When running in a cloud environment and whenever a node is unhealthy, the node controller asks the cloud provider if the VM for that node is still available. If not, the node controller deletes the node from its list of nodes.

The third is monitoring the nodes' health. The node controller is responsible for:

- Updating the NodeReady condition of NodeStatus to ConditionUnknown when a node becomes unreachable, as the node controller stops receiving heartbeats for some reason such as the node being down.
- Evicting all the pods from the node using graceful termination if the node continues to be unreachable. The default timeouts are 40s to start reporting ConditionUnknown and 5m after that to start evicting pods.

The node controller checks the state of each node every ```--node-monitor-period``` seconds.

## 100 - Heartbeats
See [README.md](./100/README.md)

## 200 - Reliability
See [README.md](./200/README.md)
