# 200 - Self-registration of Nodes
When the kubelet flag ```--register-node``` is true (the default), the kubelet will attempt to register itself with the API server. This is the preferred pattern, used by most distros.

For self-registration, the kubelet is started with the following options:

- ```--kubeconfig``` - Path to credentials to authenticate itself to the API server.

- ```--cloud-provider``` - How to talk to a cloud provider to read metadata about itself.

- ```--register-node``` - Automatically register with the API server.

- ```--register-with-taints``` - Register the node with the given list of taints (comma separated ```<key>=<value>:<effect>```).

No-op if ```register-node``` is false.

- ```--node-ip``` - IP address of the node.

- ```--node-labels``` - Labels to add when registering the node in the cluster (see label restrictions enforced by the NodeRestriction admission plugin).

- ```--node-status-update-frequency``` - Specifies how often kubelet posts node status to master.

When the Node authorization mode and NodeRestriction admission plugin are enabled, kubelets are only authorized to create/modify their own Node resource.
