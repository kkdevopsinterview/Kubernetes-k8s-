## 132. How do Network Policies control Pod communication?

Network Policies in Kubernetes are used to control how Pods communicate
with each other and external endpoints. By default, Pods can communicate
freely inside the cluster. Network Policies allow us to define rules
that permit or deny traffic based on:

-   Pod labels
-   Namespace labels
-   Ports and protocols

Once a Network Policy is applied, only the explicitly allowed traffic
flows --- everything else is blocked.

They work like a firewall for Pods.

### Real-World Example

Allowing only frontend Pods to communicate with backend Pods while
blocking all other access.

### Interview Punch Line

"Network Policies enforce Pod-level traffic security."

------------------------------------------------------------------------

## 133. Why are Network Policies important for security?

Without Network Policies, any compromised Pod could communicate with
sensitive services. Network Policies reduce the attack surface by
enforcing segmentation.

They protect:

-   databases
-   internal APIs
-   sensitive workloads

They are essential for zero-trust networking inside clusters.

### Real-World Example

Blocking unauthorized Pods from accessing a payment database.

### Interview Line

"Network segmentation strengthens cluster security."

------------------------------------------------------------------------

## 134. What is the default network behavior without policies?

By default, Kubernetes networking is fully open. All Pods can
communicate with each other across namespaces.

No restrictions exist unless policies are defined.

This makes initial deployment easy but unsafe for production.

### Real-World Example

A debugging Pod could reach production database unless restricted.

### Interview Punch Line

"Default Kubernetes networking is allow-all."

------------------------------------------------------------------------

## 135. How does Kubernetes enforce Network Policies?
## KK FUNDA 

Kubernetes itself defines policies, but enforcement is handled by the
CNI plugin.

CNI tools like:

-   Calico
-   Cilium
-   Weave

Translate policy rules into actual network filtering rules.

If the cluster uses a CNI without policy support, policies won't work.

### Real-World Example

Calico converting policy YAML into iptables rules.

### Interview Line

"CNI plugins implement Network Policy enforcement."

------------------------------------------------------------------------

## 136. How do you allow traffic from selected Pods only?

This is done using label selectors in Network Policies.

You define:

-   which Pods receive traffic
-   which Pods are allowed to send traffic

Traffic is matched based on labels.

### Real-World Example

Allow traffic only from Pods labeled app=frontend to backend Pods.

### Interview Punch Line

"Selectors define allowed communication paths."

------------------------------------------------------------------------

## 137. How do you protect a database using Network Policies?

Database protection strategy:

-   Deny all traffic by default
-   Allow only backend Pods
-   Restrict specific ports

This ensures only authorized application layers access the database.

### Real-World Example

Blocking internal testing Pods from accessing production DB.

### Interview Line

"Least-access networking protects critical services."

------------------------------------------------------------------------

## 138. How do you allow secure cross-namespace traffic?

Cross-namespace traffic is controlled using namespace selectors.

Policies specify:

-   allowed namespaces
-   allowed Pods inside them

This allows fine-grained multi-team communication.

### Real-World Example

Dev namespace allowed to call shared logging namespace.

### Interview Punch Line

"Namespace selectors enable controlled cross-environment communication."

------------------------------------------------------------------------

## 139. What role does CNI play in Kubernetes networking?

CNI (Container Network Interface) provides the network connectivity for
Pods.

It handles:

-   IP allocation
-   routing
-   network isolation
-   policy enforcement

Without CNI, Pods cannot communicate.

### Real-World Example

Calico assigns IP addresses and manages routing.

### Interview Line

"CNI is the networking backbone of Kubernetes."

------------------------------------------------------------------------

## 140. Which CNI plugins are commonly used?

Popular CNI plugins include:

-   Calico --- networking + security
-   Flannel --- simple networking
-   Cilium --- advanced eBPF networking
-   Weave Net --- easy setup

Choice depends on scalability and security needs.

### Real-World Example

Enterprises often use Calico for policy support.

### Interview Punch Line

"CNI choice impacts networking performance and security."
