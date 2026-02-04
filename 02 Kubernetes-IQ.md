
## 16. Explain Kubernetes Architecture

Kubernetes follows a **master–worker (control plane–node)** architecture.

The **control plane** is responsible for managing the cluster. It makes decisions like:
- What should run  
- Where it should run  
- How many replicas should exist  

The **worker nodes** are responsible for actually running the application workloads in the form of Pods.

All communication in Kubernetes happens through the **API server**. Users, CI/CD tools, and internal components talk only to the API server, not directly to each other.

This architecture helps Kubernetes remain **scalable, reliable, and loosely coupled**.

### Real-World Example
In AWS EKS:
- AWS manages the control plane  
- You manage worker nodes (EC2)  
- Applications run only on worker nodes  

### Interview Punch Line
“Kubernetes separates cluster management from workload execution.”

---

## 17. What is the Kubernetes Control Plane?

The Kubernetes control plane is the **brain of the cluster**.  
It is responsible for maintaining the **desired state** of the cluster.

### Main responsibilities:
- Scheduling Pods  
- Monitoring cluster health  
- Scaling applications  
- Self-healing failed components  
- Storing cluster state  

If the control plane is down, existing Pods may continue running, but:
- You cannot deploy new apps  
- You cannot scale or update workloads  

### Real-World Example
If the API server is unavailable, `kubectl get pods` will fail even though applications are still running.

---

## 18. What is kube-apiserver?

kube-apiserver is the **entry point** of the Kubernetes cluster.

Every operation goes through it:
- kubectl commands  
- CI/CD pipelines  
- Controllers  
- Scheduler  

It performs:
- Request validation  
- Authentication  
- Authorization (RBAC)  
- Updates cluster state in etcd  

### Real-World Example
When you run:
```bash
kubectl apply -f deployment.yaml
```
The YAML is first sent to the kube-apiserver.

### Interview Gold Line
“If kube-apiserver is down, the cluster becomes read-only or unusable.”

---

## 19. What is etcd and why is it important?

etcd is a **distributed key-value store** used by Kubernetes to store the **entire cluster state**.

It stores:
- Pods  
- Deployments  
- Services  
- ConfigMaps  
- Secrets  
- Node information  

etcd is the **single source of truth** for Kubernetes.

If etcd data is lost, Kubernetes loses the cluster configuration.

### Real-World Example
In production, etcd is always:
- Backed up regularly  
- Run with high availability  

### Interview Punch Line
“No etcd means no Kubernetes cluster.”

---

## 20. What is kube-scheduler?

kube-scheduler decides **which worker node a Pod should run on**.

It does **not create Pods**.  
It only selects the best node based on:
- CPU and memory availability  
- Resource requests  
- Taints and tolerations  
- Affinity and anti-affinity rules  

Once a node is selected, **kubelet** on that node creates the Pod.

### Real-World Example
If one node is full and another has free resources, the scheduler picks the free node.

---

## 21. What is kube-controller-manager?

kube-controller-manager runs multiple controllers that continuously monitor the cluster.

Controllers ensure:
- Desired state = Actual state  

### Common controllers:
- ReplicaSet controller  
- Node controller  
- Job controller  

### Real-World Example
If a Pod dies, the ReplicaSet controller automatically creates a new Pod.

### Interview Line
“Controllers are the self-healing mechanism of Kubernetes.”

---

## 22. What is kubelet?

kubelet is an agent that runs on **every worker node**.

### Responsibilities:
- Talks to kube-apiserver  
- Pulls container images  
- Starts and stops containers  
- Reports node and Pod status  

kubelet ensures containers defined in Pod specifications are running.

### Real-World Example
If a container crashes, kubelet restarts it based on Pod configuration.

---

## 23. What is kube-proxy?

kube-proxy is responsible for **networking at the Service level**.

It:
- Maintains network rules  
- Forwards traffic from Services to Pods  
- Supports load balancing  

kube-proxy works using **iptables or IPVS**.

### Real-World Example
When traffic hits a Service IP, kube-proxy routes it to one of the backend Pods.

---

## 24. How does kubectl communicate with the cluster?

kubectl communicates with Kubernetes using **REST API calls**.

It uses a **kubeconfig** file that contains:
- Cluster address  
- Authentication details  
- Context information  

kubectl sends requests **only to the kube-apiserver**.

### Real-World Example
Different kubeconfig contexts allow switching between dev and prod clusters.

---

## 25. What happens when you run kubectl apply?

When you run:
```bash
kubectl apply -f app.yaml
```

### Steps:
1. YAML is sent to kube-apiserver  
2. API server validates the request  
3. Desired state is stored in etcd  
4. Controllers detect changes  
5. Scheduler assigns a node  
6. kubelet creates containers  
7. Kubernetes continuously monitors and maintains this state  

### Interview Punch Line
“Kubernetes works on a declarative model, not an imperative one.”
