# 300 - Manual Node administration
You can create and modify Node objects using ***kubectl***.

When you want to create Node objects manually, set the kubelet flag ```--register-node=false```.

You can modify Node objects regardless of the setting of ```--register-node```. For example, you can set labels on an existing Node or mark it unschedulable.

You can use labels on Nodes in conjunction with node selectors on Pods to control scheduling. For example, you can constrain a Pod to only be eligible to run on a subset of the available nodes.

Marking a node as unschedulable prevents the scheduler from placing new pods onto that Node but does not affect existing Pods on the Node. This is useful as a preparatory step before a node reboot or other maintenance.

To mark a Node unschedulable, run:

```kubectl cordon $NODENAME```

***Note***: Pods that are part of a DaemonSet tolerate being run on an unschedulable Node. DaemonSets typically provide node-local services that should run on the Node even if it is being drained of workload applications.
