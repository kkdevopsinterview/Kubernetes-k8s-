## 77. How does Kubernetes decide which node runs a Pod?

Kubernetes decides where a Pod should run using the **kube-scheduler**.

When a Pod is created, it does not immediately run on any node. The scheduler looks at all available worker nodes and determines which node is best suited to run the Pod.

The scheduler works in two phases:
- **Filtering**: Removes nodes that cannot run the Pod (for example, insufficient CPU or memory).
- **Scoring**: Scores the remaining nodes and selects the best one.

Once the node is selected, the scheduler informs the **kubelet** on that node, and the Pod is started.

### Interview Punch Line
“Kubernetes schedules Pods based on resource availability and constraints.”

---

## 78. What factors influence scheduling decisions?

The Kubernetes scheduler considers multiple factors before placing a Pod:
- CPU and memory availability  
- Resource requests defined in the Pod  
- Taints and tolerations  
- Node selectors and node affinity  
- Pod affinity and anti-affinity  
- Node conditions (disk pressure, memory pressure)  

Scheduling is rule-based and optimized, not random.

### Interview Line
“Scheduler evaluates resources, rules, and constraints before placing Pods.”

---

## 79. Why do we define resource requests?

Resource requests define the **minimum CPU and memory** a Pod needs to run.

The scheduler uses requests to:
- Decide which node can run the Pod  
- Pack Pods efficiently across nodes  

Without requests, Kubernetes cannot make intelligent scheduling decisions.

### Interview Punch Line
“Requests help Kubernetes schedule Pods correctly.”

---

## 80. What is the purpose of resource limits?

Resource limits define the **maximum CPU and memory** a container is allowed to use.

Limits protect the cluster by:
- Preventing one Pod from consuming all node resources  
- Keeping other applications stable  
- Ensuring fair resource usage  

Limits are enforced **at runtime**, not during scheduling.

### Interview Line
“Resource limits protect cluster stability by preventing resource abuse.”

---

## 81. How do requests and limits differ?

Requests and limits serve different purposes:
- **Requests** are used for scheduling  
- **Limits** are used for enforcement  

Requests affect **where a Pod runs**.  
Limits affect **how much a container can use at runtime**.

### Interview Punch Line
“Requests decide where a Pod runs, limits decide how much it can use.”

---

## 82. What happens if a container exceeds CPU limits?

CPU is a **compressible resource**.

If a container exceeds its CPU limit:
- Kubernetes does **not kill** the container  
- The container is **throttled**  
- The application continues running but slower  

### Interview Line
“Exceeding CPU limits causes throttling, not termination.”

---

## 83. What happens if memory limits are exceeded?

Memory is a **non-compressible resource**.

If a container exceeds its memory limit:
- Kubernetes kills the container immediately  
- The container enters the **OOMKilled** state  
- The Pod may restart depending on restart policy  

### Interview Punch Line
“Memory limit violations lead to container termination.”

---

## 84. How are resources handled in multi-container Pods?

In a multi-container Pod:
- All containers run on the same node  
- Each container defines its own requests and limits  

During scheduling:
- Kubernetes adds up all container requests  
- The Pod is placed on a node that can satisfy the total  

At runtime:
- Limits are enforced **per container**, not per Pod  

### Interview Line
“Scheduling is Pod-level, but limits are enforced at the container level.”

---

## 85. What is Quality of Service (QoS)?

Quality of Service (QoS) defines how Kubernetes **prioritizes Pods during resource pressure**.

Based on requests and limits, Kubernetes assigns each Pod a QoS class.
QoS helps decide:
- Which Pods are protected  
- Which Pods are evicted first  

### Interview Punch Line
“QoS controls Pod priority under resource pressure.”

---

## 86. How do Guaranteed, Burstable, and BestEffort Pods differ?


### Guaranteed Pods
- CPU requests = CPU limits  
- Memory requests = Memory limits  
- Highest priority, least likely to be evicted  

### Burstable Pods
- Requests defined  
- Limits higher than requests  
- Medium eviction priority  

### BestEffort Pods
- No requests or limits  
- No guarantees  
- First to be evicted  

### Interview Punch Line
“QoS classes define resource guarantees and eviction priority in Kubernetes.”
