# 400 - Gracefull Node Shutdown
***FEATURE STATE***: Kubernetes v1.21 [beta]

The kubelet attempts to detect node system shutdown and terminates pods running on the node.

Kubelet ensures that pods follow the normal pod termination process during the node shutdown.

The Graceful node shutdown feature depends on systemd since it takes advantage of systemd inhibitor locks to delay the node shutdown with a given duration.

Graceful node shutdown is controlled with the ```GracefulNodeShutdown``` feature gate which is enabled by default in 1.21.

Note that by default, both configuration options described below, ```ShutdownGracePeriod``` and ```ShutdownGracePeriodCriticalPods``` are set to zero, thus not activating Graceful node shutdown functionality. To activate the feature, the two kubelet config settings should be configured appropriately and set to non-zero values.

During a graceful shutdown, kubelet terminates pods in two phases:

1. Terminate regular pods running on the node.
2. Terminate critical pods running on the node.

Graceful node shutdown feature is configured with two KubeletConfiguration options:

- ```ShutdownGracePeriod```:
- -- Specifies the total duration that the node should delay the shutdown by. This is the total grace period for pod termination for both regular and critical pods.

- ```ShutdownGracePeriodCriticalPods```:
- -- Specifies the duration used to terminate critical pods during a node shutdown. This value should be less than ShutdownGracePeriod.

For example, if ```ShutdownGracePeriod=30s```, and ```ShutdownGracePeriodCriticalPods=10s```, kubelet will delay the node shutdown by 30 seconds. During the shutdown, the first 20 (30-10) seconds would be reserved for gracefully terminating normal pods, and the last 10 seconds would be reserved for terminating critical pods.
