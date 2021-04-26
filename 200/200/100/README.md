# 100 - Nodes
Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods

Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have only one node.

The components on a node include the kubelet, a container runtime, and the kube-proxy.

## 100 - Management
See [README.md](./100/README.md)

## 200 - Node Status
See [README.md](./200/README.md)

## 300 - Node Topology
See [README.md](./300/README.md)

## 400 - Gracefull Node Shutdown
See [README.md](./400/README.md)
