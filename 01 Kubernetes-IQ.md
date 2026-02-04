# Kubernetes Interview 

## 1. What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and maintain containerized applications automatically.

In real projects, applications are not just one container. We have many microservices, multiple versions, high traffic, and failures. Managing containers manually using Docker alone becomes very hard. Kubernetes acts like a central control system that manages containers across many servers.

Kubernetes follows a declarative approach. We tell Kubernetes what we want (for example, 3 replicas of an app), and Kubernetes continuously works to maintain that state.

### Real-World Example
In an e-commerce application, during a sale, traffic increases suddenly. Kubernetes automatically creates more Pods to handle traffic and scales them down when traffic reduces.

### Interview Punch Lines
- “Kubernetes manages containers at scale.”
- “It provides self-healing, auto-scaling, and high availability.”

---

## 2. Why do we need Kubernetes?

We need Kubernetes because manual container management does not scale in production environments.

Without Kubernetes:
- Containers must be restarted manually
- Scaling needs human intervention
- Load balancing is complex
- Downtime risk is high

Kubernetes automates these tasks and reduces human error.

### Real-World Example
If a container crashes at 3 AM, without Kubernetes someone must log in and restart it. With Kubernetes, the Pod restarts automatically within seconds.

### Interview Punch Line
“Kubernetes reduces operational overhead and improves reliability.”

---

## 3. What problems does Kubernetes solve?

Kubernetes solves several real production problems:
- Application downtime
- Manual scaling
- Poor resource utilization
- Deployment inconsistency
- Server failures

It ensures applications stay running even when containers or nodes fail.

### Real-World Example
If one server crashes, Kubernetes automatically reschedules Pods on healthy servers without downtime.

---

## 4. What is container orchestration?

Container orchestration means automatically managing the lifecycle of containers. This includes:
- Scheduling containers on nodes
- Scaling containers up or down
- Restarting failed containers
- Managing networking and storage

Kubernetes is a container orchestrator.

### Real-World Example
Managing 200 containers across 20 servers manually is impossible. Kubernetes handles it efficiently.

---

## 5. What is a Kubernetes cluster?

A Kubernetes cluster is a group of machines that work together to run containerized applications.

A cluster consists of:
- Control Plane – manages the cluster
- Worker Nodes – run application Pods

### Real-World Example
In AWS EKS:
- AWS manages the control plane
- EC2 instances act as worker nodes

---

## 6. What are the main components of Kubernetes?

**Control Plane components:**
- kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager

**Worker Node components:**
- kubelet
- kube-proxy
- container runtime (Docker/containerd)

### Simple Understanding
Control plane decides what should run, worker nodes execute it.

---

## 7. What is a Node in Kubernetes?

A Node is a machine (VM or physical) where application Pods run.

Each node runs:
- kubelet (manages Pods)
- kube-proxy (networking)
- container runtime

### Real-World Example
Each EC2 instance in a Kubernetes worker node group is a node.

---

## 8. Difference between master and worker nodes?

**Master / Control Plane:**
- Manages cluster state
- Schedules Pods
- Handles scaling and healing

**Worker Nodes:**
- Run application Pods
- Execute containers

### Interview Punch Line
“Control plane manages, worker nodes execute.”

---

## 9. What is a Pod?

A Pod is the smallest building block in Kubernetes.
It represents a running process in the cluster.

Pods:
- Always run on worker nodes
- Contain one or more containers
- Have a unique IP address
- Are created inside a namespace
- Are ephemeral (can be recreated anytime)

### Real-World Example
A backend app container and a log collector running together inside one Pod.

---

## 10. Why are Pods needed instead of containers?

Pods allow tightly coupled containers to:
- Share the same IP
- Communicate using localhost
- Share storage
- Scale together

Containers alone cannot provide this level of coordination.

### Real-World Example
App container + metrics exporter in the same Pod.

---

## 11. Can a Pod have multiple containers?

Yes. A Pod can have multiple containers when they need to work closely together.

### Real-World Example
- Main application container
- Sidecar container for logging or monitoring

---

## 12. What is a Namespace?

A Namespace is a logical isolation mechanism inside a Kubernetes cluster.

It helps divide cluster resources between teams, environments, or projects.

### Real-World Example
Separate namespaces for dev, staging, and production in the same cluster.

---

## 13. Why do we use Namespaces?

Namespaces are used to:
- Isolate environments
- Apply RBAC permissions
- Set resource limits
- Avoid naming conflicts

### Real-World Example
Different teams using the same cluster safely.

---

## 14. What are default namespaces in Kubernetes?

Default namespaces include:
- default – user workloads
- kube-system – system components
- kube-public – public data
- kube-node-lease – node heartbeats

---

## 15. How do you list all resources in a namespace?

```bash
kubectl get all -n <namespace>
```

This command lists Pods, Services, Deployments, ReplicaSets, etc.
