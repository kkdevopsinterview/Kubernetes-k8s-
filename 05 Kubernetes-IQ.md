## 48. How do you expose HTTP/HTTPS applications using Ingress?
In Kubernetes, **Ingress** is used to expose HTTP and HTTPS applications to the outside world in a controlled and centralized way.

Instead of exposing every application using a separate Service of type LoadBalancer, we define **Ingress rules** that describe:
- Which URL should go to which Service
- Which domain should route to which backend

Ingress works at **Layer 7 (HTTP/HTTPS)**, meaning it understands paths, hostnames, and TLS.

Ingress itself does not handle traffic. It requires an **Ingress Controller** (like NGINX Ingress) to process requests.

### Real-World Example
Users access:
```
https://myapp.com
```
Ingress receives the request and routes it to the backend Service inside the cluster.

### Interview Punch Line
“Ingress is the entry point for HTTP/HTTPS traffic into a Kubernetes cluster.”

---

## 49. Why is Ingress preferred over multiple LoadBalancers?

Using a LoadBalancer Service for every application is:
- Expensive (cloud cost)
- Hard to manage
- Not scalable

Ingress allows **multiple applications to share a single LoadBalancer**.

Ingress routes traffic based on:
- URL paths
- Hostnames

This makes traffic management centralized, cost-effective, and clean.

### Real-World Example
Instead of 10 AWS LoadBalancers for 10 services:
- 1 LoadBalancer
- 1 Ingress
- Multiple routing rules

### Interview Line
“Ingress reduces cost and simplifies traffic management.”

---

## 50. How does Ingress differ from a Service?

| Service | Ingress |
|--------|---------|
| Works at Layer 4 | Works at Layer 7 |
| Handles TCP/UDP | Handles HTTP/HTTPS |
| Simple load balancing | Advanced routing |
| Exposes one service | Routes to many services |

A Service exposes one application, while Ingress can expose many applications using routing rules.

### Real-World Example
Service exposes backend internally, Ingress exposes it externally with domain and TLS.

### Interview Punch Line
“Service is basic networking, Ingress is smart HTTP routing.”

---

## 51. What role does an Ingress Controller play?

Ingress is only a **set of rules**.
An **Ingress Controller** is the component that actually enforces those rules.

The controller:
- Watches Ingress resources
- Configures itself (NGINX, HAProxy, Traefik)
- Routes external traffic to Services

Without an Ingress Controller, Ingress rules do nothing.

### Real-World Example
NGINX Ingress Controller runs as Pods in the cluster and acts as a reverse proxy.

### Interview Gold Line
“No Ingress Controller means no Ingress traffic.”

---

## 52. How does NGINX Ingress route traffic internally?

NGINX Ingress works as a **reverse proxy**.

### Traffic flow:
1. External request hits LoadBalancer
2. LoadBalancer forwards to NGINX Ingress Pod
3. NGINX reads Ingress rules
4. Routes traffic to correct Service
5. Service forwards traffic to Pods

NGINX automatically updates its configuration when Ingress rules change.

### Real-World Example
- `/api` → backend service
- `/ui` → frontend service

### Interview Line
“NGINX Ingress dynamically routes traffic based on rules.”

---

## 53. How can multiple services share a single LoadBalancer?

Multiple services share one LoadBalancer by placing **Ingress in front of them**.

The LoadBalancer exposes **one external IP**.
Ingress routes traffic internally using:
- URL paths
- Hostnames

This avoids creating multiple LoadBalancers.

### Real-World Example
```
app.example.com → frontend
api.example.com → backend
```
Both use the same LoadBalancer IP.

### Interview Punch Line
“Ingress enables shared LoadBalancer architecture.”

---

## 54. How is HTTPS configured using Ingress?

HTTPS in Ingress is configured using **TLS settings**.

### Steps:
1. Create TLS certificate (manual or cert-manager)
2. Store certificate as a Secret
3. Reference the Secret in Ingress YAML

The Ingress Controller handles:
- TLS termination
- HTTPS → HTTP forwarding to Services

### Real-World Example
Using **Let’s Encrypt with cert-manager** to auto-generate certificates.

### Interview Line
“Ingress handles TLS termination using Secrets.”

---

## 55. How does path-based routing work?

Path-based routing sends traffic to different services based on URL path.

### Example rules:
- `/api` → backend service
- `/admin` → admin service
- `/` → frontend service

Ingress Controller matches the path and routes accordingly.

### Real-World Example
Single domain hosting multiple microservices.

### Interview Punch Line
“Path-based routing enables microservices under one domain.”

---

## 56. How does host-based routing work?

Host-based routing routes traffic based on **domain name**.

Ingress checks the **Host header** and forwards traffic accordingly.

### Real-World Example
- `app.example.com` → frontend
- `api.example.com` → backend

### Interview Line
“Host-based routing separates applications by domain.”
