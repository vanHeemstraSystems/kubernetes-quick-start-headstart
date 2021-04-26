# 100 - Heartbeats
Heartbeats, sent by Kubernetes nodes, help determine the availability of a node.

There are two forms of heartbeats: updates of ```NodeStatus``` and the Lease object. Each Node has an associated Lease object in the ```kube-node-lease``` namespace. Lease is a lightweight resource, which improves the performance of the node heartbeats as the cluster scales.

The kubelet is responsible for creating and updating the ```NodeStatus``` and a Lease object.

- The kubelet updates the ```NodeStatus``` either when there is change in status or if there has been no update for a configured interval. The default interval for ```NodeStatus``` updates is 5 minutes, which is much longer than the 40 second default timeout for unreachable nodes.
- The kubelet creates and then updates its Lease object every 10 seconds (the default update interval). Lease updates occur independently from the ```NodeStatus``` updates. If the Lease update fails, the kubelet retries with exponential backoff starting at 200 milliseconds and capped at 7 seconds.
