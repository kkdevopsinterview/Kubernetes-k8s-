
## 67. How does Kubernetes provide storage to Pods?

By default, containers inside a Pod have **temporary storage**. When a Pod is deleted or restarted, that data is lost.

To solve this, Kubernetes provides **Volumes**, which allow data to live outside the container lifecycle.

A Volume is attached to a Pod and mounted inside containers.  
As long as the Pod exists, the data inside the Volume remains available.

Kubernetes supports multiple volume types such as:
- emptyDir  
- hostPath  
- Persistent Volumes (PV)  

### Real-World Example
A container writes log files to a volume so logs are not lost when the container restarts.

### Interview Punch Line
“Kubernetes separates application lifecycle from data lifecycle.”

---

## 68. What problem does a Persistent Volume (PV) solve?

A Persistent Volume (PV) solves the problem of **data persistence beyond Pod lifetime**.

A PV represents actual storage in the infrastructure:
- Cloud disks (EBS, Azure Disk, GCE PD)  
- NFS  
- SAN storage  

A PV exists independently of Pods.  
Even if a Pod is deleted, the data in the PV remains.

### Real-World Example
A database Pod crashes, but the data stored in the PV is preserved and reused by the new Pod.

### Interview Line
“Persistent Volumes make storage independent of Pods.”

---

## 69. How does a Persistent Volume Claim (PVC) work?

A Persistent Volume Claim (PVC) is a **request for storage** by a user or application.

A PVC specifies:
- Required storage size  
- Access mode  
- StorageClass  

Kubernetes automatically matches the PVC with a suitable PV.
Once bound, the Pod uses the PVC, not the PV directly.

### Real-World Example
An application requests 10Gi storage using a PVC, and Kubernetes assigns a matching PV automatically.

### Interview Punch Line
“PVC is how applications request storage.”

---

## 70. How do PV and PVC differ in responsibility?

- **PV** → Provided by admin or dynamically by cloud  
- **PVC** → Requested by developer or application  

PV represents supply, PVC represents demand.

Developers do not need to know storage implementation details.

### Real-World Example
Admin creates a StorageClass, developer creates a PVC.

### Interview Line
“PV is infrastructure, PVC is application request.”

---

## 71. Why do we need StorageClasses?

StorageClasses define **types of storage** available in the cluster.

They allow:
- Dynamic provisioning  
- Multiple performance tiers  
- Abstraction from cloud provider details  

Without StorageClasses, PVs must be created manually.

### Real-World Example
- Fast SSD storage for databases  
- Standard storage for logs  

### Interview Punch Line
“StorageClasses automate storage provisioning.”

---

## 72. What is dynamic volume provisioning?

Dynamic provisioning means storage is created **automatically when a PVC is created**.

Kubernetes uses the StorageClass to:
1. Create the disk  
2. Attach it to the node  
3. Bind it to the PVC  

No manual PV creation is required.

### Real-World Example
Creating a PVC in AWS automatically provisions an EBS volume.

### Interview Line
“Dynamic provisioning removes manual storage management.”

---

## 73. What access modes can a volume have?

Kubernetes supports three access modes:
- **ReadWriteOnce (RWO)** – mounted by one node  
- **ReadOnlyMany (ROX)** – read-only by many nodes  
- **ReadWriteMany (RWX)** – read-write by many nodes  

Support depends on the storage backend.

### Real-World Example
- NFS supports RWX  
- AWS EBS supports RWO  

### Interview Punch Line
“Access modes depend on storage type.”

---

## 74. What happens to data when a Pod using PVC is deleted?

When a Pod is deleted:
- PVC remains  
- PV remains  
- Data is preserved  

Data lifecycle is independent of Pod lifecycle.

Deletion behavior depends on reclaim policy:
- Retain  
- Delete  

### Real-World Example
A database Pod restarts and reuses the same data.

### Interview Line
“Deleting a Pod does not delete persistent data.”

---

## 75. How does StatefulSet manage persistent storage?

A StatefulSet automatically creates:
- One PVC per Pod  
- Stable storage mapping  

Each Pod always gets the same PVC, even after restart.

Pods are created in order and storage identity is preserved.

### Real-World Example
`db-0` always uses `db-0-pvc`, even after restart.

### Interview Punch Line
“StatefulSet guarantees stable storage per Pod.”

---

## 76. Why are StatefulSets preferred for databases?

Databases require:
- Persistent storage  
- Stable network identity  
- Ordered startup and shutdown  

StatefulSets provide all these guarantees.
Deployments cannot ensure storage and identity stability.

### Real-World Example
MySQL, PostgreSQL, and MongoDB clusters use StatefulSets.

### Interview Line
“Stateful applications need StatefulSets.”
