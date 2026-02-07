## 115. What does it indicate when a node becomes NotReady?

When a Kubernetes node shows NotReady, it means the control plane cannot
communicate properly with that node or the node cannot report its health
status.

A node may become NotReady due to: - kubelet stopped running - network
connectivity issues - resource exhaustion (disk, memory) - node crash or
reboot - cloud infrastructure problems

When this happens, Kubernetes avoids scheduling new Pods on that node
and may reschedule existing Pods elsewhere.

### Real-World Example

A worker node loses network connectivity. The control plane marks it
NotReady and workloads move to healthy nodes.

### Interview Punch Line

**"NotReady means the node cannot safely run workloads."**

------------------------------------------------------------------------

## 116. How would you troubleshoot a NotReady node?

Troubleshooting steps:

#### Check node status

``` bash
kubectl describe node <node-name>
```

Check node conditions and events.

#### Verify kubelet service on the node

``` bash
systemctl status kubelet
```

#### Check resources

-   disk space
-   memory
-   CPU

Also inspect networking and container runtime.

### Real-World Example

Node disk filled up; clearing space restored Ready state.

### Interview Line

**"Start with node description, then inspect kubelet and resources."**

------------------------------------------------------------------------

## 117. What node conditions does Kubernetes monitor?


Kubernetes tracks several node health conditions: - Ready -
MemoryPressure - DiskPressure - PIDPressure - NetworkUnavailable

These conditions help Kubernetes decide scheduling and eviction
behavior.

### Real-World Example

If DiskPressure is detected, new Pods may not be scheduled on that node.

### Interview Punch Line

**"Node conditions guide scheduling decisions."**

------------------------------------------------------------------------

## 118. Why do we cordon and drain nodes?

Cordon and drain are used for safe node maintenance.

**Cordon**\
Marks node unschedulable --- no new Pods placed

**Drain**\
Safely evicts existing Pods and moves them elsewhere

This ensures zero disruption during upgrades or maintenance.

### Real-World Example

Upgrading OS patches on a worker node.

### Interview Line

**"Cordon prevents scheduling, drain migrates workloads."**

------------------------------------------------------------------------

## 119. In what scenarios would you drain a node?

Node draining is done during: - node upgrades - hardware maintenance -
cluster scaling down - troubleshooting

It ensures applications remain available while node work happens.

### Real-World Example

Before terminating a cloud instance, draining prevents sudden workload
loss.

### Interview Punch Line

**"Drain protects application availability during node changes."**

------------------------------------------------------------------------

## 120. What happens to Pods if a node fails?

If a node fails completely: - Control plane detects heartbeat loss -
Pods marked lost - Controllers create replacements on healthy nodes

This is Kubernetes self-healing capability.

### Real-World Example

Cloud VM crashes; Kubernetes recreates Pods elsewhere automatically.

### Interview Line

**"Controllers restore desired state after node failure."**

------------------------------------------------------------------------

## 121. How does Kubernetes recover workloads after node failure?

Recovery process: 1. Node marked NotReady\
2. Pods marked unavailable\
3. ReplicaSet or Deployment detects mismatch\
4. New Pods scheduled elsewhere

Storage-backed workloads reconnect using persistent volumes.

### Real-World Example

Web app continues running on remaining nodes without manual
intervention.

### Interview Punch Line

**"Kubernetes restores workloads through reconciliation.Reconciliation is the continuous control-loop process where Kubernetes detects differences between desired and actual state and automatically takes corrective action to restore the declared state."**
