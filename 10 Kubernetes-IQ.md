## 97. How are rolling updates performed in Kubernetes?

Rolling updates in Kubernetes are performed using a Deployment with a
`rollingUpdate` strategy.

When we update the application image or configuration, Kubernetes does
not stop all Pods at once. Instead, it: - creates new Pods with the new
version - gradually removes old Pods - ensures some Pods are always
running

This allows the application to stay available during updates.

Rolling updates are controlled by Deployment settings like: -
`maxSurge` - `maxUnavailable`

### Real-World Example

Suppose we update an API image from v1 to v2 with 4 replicas:

Kubernetes might start 1 new v2 Pod (maxSurge=1)

Once Ready → terminate 1 v1 Pod

Continue until all Pods run v2

Users never lose access because traffic is always served by healthy Pods.

### Interview Punch Line

> Rolling updates replace Pods gradually to avoid downtime. In Kubernetes, this is achieved by creating a new ReplicaSet and scaling it up while scaling down the old one, using maxSurge and maxUnavailable to maintain service availability.

------------------------------------------------------------------------

## 98. How does Kubernetes avoid downtime during deployments?

Kubernetes avoids downtime by ensuring new Pods become ready before old
Pods are removed.

Key mechanisms: - rolling updates - readiness probes - Service routing

Traffic is sent only to ready Pods. If a new Pod is not ready,
Kubernetes keeps the old Pod running.

### Real-World Example

During deployment, users continue using the application while new Pods
are warming up.

### Interview Line

> Readiness probes + rolling updates ensure zero downtime.

------------------------------------------------------------------------

## 99. What controls update behavior in rolling deployments?

Update behavior is controlled by the `rollingUpdate` strategy in the
Deployment.

Two key parameters: - `maxSurge` -- how many extra Pods can be created -
`maxUnavailable` -- how many Pods can be unavailable

These values control speed vs safety of deployments.

### Real-World Example

Setting `maxUnavailable: 0` ensures no downtime but slower rollout.

### Interview Punch Line

> rollingUpdate parameters balance speed and availability.

------------------------------------------------------------------------

## 100. How do maxSurge and maxUnavailable affect deployments?

**maxSurge** - Allows creating extra Pods temporarily during updates

**maxUnavailable** - Limits how many Pods can be down at a time

Together, they control how aggressively Kubernetes updates the
application.

### Real-World Example

With **4 replicas**: - `maxSurge: 1` - `maxUnavailable: 1`

Kubernetes updates safely with minimal risk.

### Interview Line

> maxSurge adds capacity, maxUnavailable limits risk.

------------------------------------------------------------------------

## 101. When would Blue-Green deployment be useful?

Blue-Green deployment maintains two identical environments: - Blue →
current version - Green → new version

Traffic is switched instantly from Blue to Green after validation.

This approach provides: - instant rollback - zero downtime

### Real-World Example

Critical production systems where even small downtime is unacceptable.

### Interview Punch Line

> Blue-Green gives fast rollback with full environment separation.

------------------------------------------------------------------------

## 102. How does Canary deployment reduce risk?

Canary deployment releases a new version to a small percentage of users
first.

If everything works: - traffic is gradually increased

If issues are detected: - rollout is stopped - impact is limited

This reduces risk compared to full rollout.

### Real-World Example

New API version is tested with **5% traffic** before full release.

### Interview Line

> Canary deployments reduce blast radius.

------------------------------------------------------------------------

## 103. How do you rollback a failed Deployment?

### KK Funda

Kubernetes supports automatic and manual rollbacks for Deployments.

Rollback can be done using:

    kubectl rollout undo deployment <deployment-name>

Kubernetes switches back to the previous ReplicaSet.

### Real-World Example

A buggy release causes errors; rollback restores stable version within
seconds.

### Interview Punch Line

> Deployments make rollback fast and safe.

------------------------------------------------------------------------

## 104. How do you track deployment rollout status?

Kubernetes provides rollout status commands to monitor deployments.

Common commands:

    kubectl rollout status deployment <name>
    kubectl get pods
    kubectl describe deployment <name>

These show: - progress - failures - readiness

### Real-World Example

DevOps engineer watches rollout status during production release.

### Interview Line

> Rollout status helps monitor deployment health.

------------------------------------------------------------------------

## 105. What happens if a rollout does not succeed?

If rollout fails: - new Pods may crash - readiness probes may fail -
deployment pauses automatically

Kubernetes does not delete old Pods unless new ones are healthy.

We can: - fix the issue - rollback - resume rollout

### Real-World Example

New image has config error; Kubernetes stops rollout, preventing
downtime.

### Interview Punch Line

> Kubernetes fails safely during bad deployments.
