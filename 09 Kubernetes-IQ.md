## 87. How does Kubernetes scale Pods automatically?

Kubernetes scales Pods automatically using the **Horizontal Pod Autoscaler (HPA)**.

HPA continuously monitors application metrics such as CPU or memory usage. When usage goes above a defined threshold, Kubernetes increases the number of Pod replicas. When usage goes down, it reduces the number of replicas.

This scaling happens automatically without manual intervention. Autoscaling helps applications handle traffic spikes while keeping infrastructure costs under control.

### Interview Punch Line
“Kubernetes automatically scales Pods based on demand.”

---

## 88. How does HPA decide when to scale?

HPA decides when to scale by comparing **current metrics** with **target metrics** defined by the user.

If current usage is higher than the target value, HPA increases the number of Pods. If usage falls below the target, it scales down.

HPA checks metrics at regular intervals and scales gradually to avoid sudden spikes.

### Interview Line
“HPA scales Pods by comparing actual usage with desired usage.”

---

## 89. What metrics are commonly used for autoscaling?

Common autoscaling metrics include:
- CPU utilization (most common)
- Memory usage
- Custom application metrics (requests per second, queue length)
- External metrics (cloud load, message queue depth)

These metrics are collected using:
- Metrics Server
- Prometheus
- Custom metrics adapters

### Interview Punch Line
“Autoscaling is driven by resource or application metrics.”

---

## 90. When would you use Vertical Pod Autoscaler (VPA)?

Vertical Pod Autoscaler (VPA) is used when an application needs **more or less resources**, not more replicas.

VPA:
- Automatically adjusts CPU and memory requests
- Helps right-size Pods
- Is useful for workloads with unpredictable resource usage

VPA may restart Pods to apply updated resource values.

### Interview Line
“VPA scales Pod size, not Pod count.”

---

## 91. How is VPA different from HPA?

- **HPA** scales the number of Pods  
- **VPA** scales the resources of a Pod  

HPA is best for stateless applications handling variable traffic.
VPA is best for optimizing resource usage of workloads.

### Interview Punch Line
“HPA scales horizontally, VPA scales vertically.”

---

## 92. How does Kubernetes scale the cluster itself?

Kubernetes scales cluster nodes using the **Cluster Autoscaler**.

When Pods cannot be scheduled due to insufficient resources, Cluster Autoscaler:
- Adds new nodes
- Schedules Pods on them

When nodes are underutilized, it:
- Safely removes nodes
- Reduces infrastructure cost

### Interview Line
“Cluster Autoscaler scales infrastructure, not Pods.”

---

## 93. When does Cluster Autoscaler add or remove nodes?

Cluster Autoscaler **adds nodes** when:
- Pods are pending
- Resource requests cannot be satisfied

It **removes nodes** when:
- Nodes are underutilized
- Pods can be safely moved elsewhere

Application availability is always preserved.

### Interview Punch Line
“Cluster Autoscaler reacts to scheduling pressure.”

---

## 94. Can HPA and VPA work together?

Yes, HPA and VPA can work together, but with care.

Best practice:
- Use HPA for scaling replicas
- Use VPA in recommendation-only mode

Using both aggressively can cause conflicts.

### Interview Line
“HPA and VPA can coexist if configured carefully.”

---

## 95. What are common reasons HPA does not scale Pods?

Common reasons HPA fails to scale include:
- Metrics Server not installed or unhealthy
- Resource requests not defined
- Metrics not crossing target thresholds
- Incorrect minReplicas or maxReplicas
- Load not sustained long enough

HPA is reactive and requires consistent metric pressure.

### Interview Line
“HPA issues are usually metric or configuration problems, not Kubernetes bugs.”

---

## 96. What commands do you use to debug HPA issues?

To debug HPA issues, I:
- Check HPA status and metrics
- Verify Pod resource usage
- Ensure metrics are available in the cluster
- Inspect events for errors

I also confirm that the application is actually under load.

### Interview Punch Line
“HPA troubleshooting starts with metrics visibility and request validation.”
