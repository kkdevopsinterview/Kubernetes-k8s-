
## 26. What is a Deployment in Kubernetes?

A Deployment is a Kubernetes workload resource used to manage **stateless applications**.

It defines:
- How many replicas of an application should run
- Which container image to use
- How updates should be rolled out

A Deployment does not directly manage Pods.
Instead:
- Deployment → manages ReplicaSets
- ReplicaSets → manage Pods

Deployments provide:
- Rolling updates
- Rollbacks
- Scaling
- Self-healing

In real production, we never create Pods directly. We always use Deployments for stateless applications.

### Real-World Example
A frontend or backend API service deployed using a Deployment with 3 replicas for high availability.

### Interview Punch Line
“Deployment is the most commonly used workload for stateless applications.”

---

## 27. Why do we use Deployments?

We use Deployments because they provide production-ready features that Pods alone do not.

### Key benefits:
- Automatic Pod recreation if one fails
- Zero-downtime rolling updates
- Easy rollback to previous versions
- Simple horizontal scaling

Deployments ensure applications stay available and stable during updates.

### Real-World Example
When deploying a new API version, the Deployment updates Pods one by one so users don’t experience downtime.

### Interview Line
“Deployments give us reliability, scalability, and controlled updates.”

---

## 28. What is a ReplicaSet?

A ReplicaSet ensures that a specified number of Pod replicas are always running.

If a Pod:
- Crashes
- Is deleted
- Is evicted

The ReplicaSet immediately creates a new Pod.

ReplicaSets focus only on:
- Maintaining Pod count

They do not handle updates or rollbacks directly.

### Real-World Example
If a ReplicaSet is configured for 3 replicas and one Pod crashes, a new Pod is created automatically.

### Interview Punch Line
“ReplicaSet maintains the desired number of Pods.”

---

## 29. Difference between ReplicaSet and Deployment?


| Deployment | ReplicaSet |
|-----------|-----------|
| Manages application lifecycle | Maintains Pod count |
| Supports rolling updates | No update strategy |
| Supports rollback | No rollback |
| Used directly by users | Managed by Deployment |

### In Practice
- Users create Deployments
- Deployments create ReplicaSets

### Real-World Example
When you update a Deployment image version, Kubernetes creates a new ReplicaSet and gradually shifts traffic.

### Interview Line
“We don’t manage ReplicaSets directly; Deployments manage them for us.”

---

## 30. What is a StatefulSet?

A StatefulSet is a workload used to manage **stateful applications**.

Unlike Deployments, StatefulSets provide:
- Stable Pod names
- Stable network identity
- Stable persistent storage

Each Pod:
- Has a unique name (pod-0, pod-1…)
- Gets its own Persistent Volume
- Is created and deleted in order

### Real-World Example
Databases like MySQL, PostgreSQL, and MongoDB use StatefulSets.

### Interview Punch Line
“StatefulSet is used when application identity and data persistence matter.”

---

## 31. When would you use a StatefulSet?

We use StatefulSets when:
- Applications store data
- Pods must not be interchangeable
- Stable network identity is required

### Ideal for:
- Databases
- Message brokers
- Distributed systems

### Real-World Example
A MySQL primary-replica setup where each Pod has its own disk and identity.

---

## 32. Difference between Deployment and StatefulSet?


| Deployment | StatefulSet |
|-----------|------------|
| Stateless apps | Stateful apps |
| Pods interchangeable | Pods have identity |
| Random Pod names | Ordered Pod names |
| Shared or no storage | Dedicated storage per Pod |

### Interview Line
“Use Deployment for stateless apps, StatefulSet for stateful apps.”

---

## 33. What is a DaemonSet?

A DaemonSet ensures that **one Pod runs on every node** (or selected nodes).

When:
- A new node is added
- A node is removed

DaemonSet automatically:
- Adds Pods
- Removes Pods

DaemonSets are mainly used for infrastructure-level services.

### Real-World Example
Log collectors, monitoring agents, security agents.

### Interview Punch Line
“DaemonSet runs one Pod per node.”

---

## 34. When would you use a DaemonSet?

We use DaemonSets when we need:
- Node-level monitoring
- Node-level logging
- System-wide agents

### Real-World Example
Running Fluentd on every node to collect logs.

---

## 35. What is a Job?

A Job is used to run a task **once** and then stop.

A Job:
- Creates one or more Pods
- Retries on failure
- Completes successfully once the task finishes

Jobs are ideal for batch processing.

### Real-World Example
Database migration or data cleanup task.

### Interview Line
“Jobs are for one-time execution.”

---

## 36. What is a CronJob?

A CronJob runs Jobs on a **scheduled basis**, similar to Linux cron.

It allows us to define:
- When a Job should run
- How often it should repeat

### Real-World Example
Daily database backup at 2 AM.

### Interview Punch Line
“CronJob is a scheduled Job.”

---

## 37. Difference between Job and CronJob?


| Job | CronJob |
|-----|---------|
| Runs once | Runs on schedule |
| Manual trigger | Time-based |
| One-time tasks | Repeating tasks |

### Real-World Example
- Job → Database migration
- CronJob → Daily backup
