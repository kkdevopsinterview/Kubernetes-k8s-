## 106. What usually causes CrashLoopBackOff?

CrashLoopBackOff occurs when a container starts, crashes, and Kubernetes
repeatedly attempts to restart it but continues to fail.\
Kubernetes applies an exponential backoff delay between restart attempts
to prevent wasting cluster resources.

Typical causes include: - Application bugs or runtime exceptions -
Missing configuration (ConfigMaps, Secrets) - Incorrect environment
variables - Database or external service connection failures - Wrong
container command or entrypoint - Resource limits causing OOMKilled

It is not the root problem itself --- it's a symptom indicating repeated
container failure.

### Real-World Example

A microservice crashes because incorrect database credentials were
configured in a Secret, causing authentication failure during startup.

### Interview Punch Line

**"CrashLoopBackOff indicates repeated container startup failure due to
an underlying application or configuration issue."**

------------------------------------------------------------------------

## 107. How do you investigate CrashLoopBackOff issues?

The investigation should follow a structured approach:

#### Check Pod details

``` bash
kubectl describe pod <pod-name>
```

This shows events, restart counts, and container state.

#### Check logs

``` bash
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
```

Previous logs help if container exits quickly.

#### Verify configuration

-   Environment variables
-   ConfigMaps / Secrets
-   Resource limits
-   Startup command

This systematic process usually reveals the root cause quickly.

### Real-World Example

Logs revealed a missing configuration file mounted incorrectly, causing
application startup failure.

### Interview Punch Line

**"Describe and logs commands are the first steps to isolate
CrashLoopBackOff root causes."**

------------------------------------------------------------------------

## 108. What does it mean when a Pod is Pending?

### KK Funda

A Pod in Pending state means Kubernetes has accepted the Pod but cannot
yet schedule it onto a node.

Common reasons include: - Insufficient CPU or memory -
PersistentVolumeClaim not bound - Node selector / affinity mismatch -
Taints without tolerations - Image pull delays

The scheduler is still attempting placement.

### Real-World Example

A Pod requesting high memory cannot be scheduled because no node has
enough available capacity.

### Interview Punch Line

**"Pending indicates scheduling has not completed due to resource or
constraint limitations."**

------------------------------------------------------------------------

## 109. How do you debug scheduling issues for Pending Pods?

Start by inspecting Pod events:

``` bash
kubectl describe pod <pod-name>
```

Look at the Events section for messages like: - Insufficient resources -
Node mismatch - Storage issues

Check cluster capacity:

``` bash
kubectl get nodes
kubectl top nodes
```

Also verify: - Node labels/taints - PVC status - Resource requests

These steps reveal scheduling blockers.

### Real-World Example

Events showed requested CPU exceeded available capacity across nodes.

### Interview Punch Line

**"Pod description events clearly expose scheduling constraints."**

------------------------------------------------------------------------

## 110. Why does ImagePullBackOff occur?

ImagePullBackOff happens when Kubernetes fails to pull the container
image from a registry and backs off retry attempts.

Common causes: - Incorrect image name or tag - Image doesn't exist -
Registry authentication failure - Network connectivity issues - Private
registry access not configured

It reflects an image retrieval failure, not runtime failure.

### Real-World Example

Deployment referencing a private image failed because imagePullSecret
was missing.

### Interview Punch Line

**"ImagePullBackOff indicates Kubernetes cannot retrieve the container
image."**

------------------------------------------------------------------------

## 111. How do you fix image pull failures?

Troubleshooting involves verifying: - Image name and tag correctness -
Registry accessibility - imagePullSecrets configuration - Network
connectivity

Useful commands:

``` bash
kubectl describe pod
kubectl get secrets
```

Fixing credentials or correcting image reference usually resolves the
issue.

### Real-World Example

Adding correct registry credentials restored successful image pulling.

### Interview Punch Line

**"Correct image references and authentication resolve most pull
failures."**

------------------------------------------------------------------------

## 112. How do you debug failing liveness probes?

Liveness probe failures trigger container restarts.\
Debugging involves: - Checking probe configuration - Validating endpoint
responses - Testing manually inside container

Commands:

``` bash
kubectl describe pod
kubectl exec -it <pod> -- curl localhost:<port>
```

Common issues: - Incorrect path or port - Timeout too short - App
startup slower than probe delay

### Real-World Example

Probe failed because health endpoint responded slowly and timeout was
too aggressive.

### Interview Punch Line

**"Incorrect probe timing or endpoint configuration often causes
unnecessary restarts."**

------------------------------------------------------------------------

## 113. How do liveness and readiness probes differ?

  -----------------------------------------------------------------------
  Probe         Purpose              Failure Result
  ------------- -------------------- ------------------------------------
  Liveness      Checks if container  Container restarted
                is alive             

  Readiness     Checks if ready for  Removed from service endpoints
                traffic              
  -----------------------------------------------------------------------

Liveness ensures recovery from deadlocks.\
Readiness protects user traffic routing.

They solve different operational concerns.

### Real-World Example

Application starting up is alive but not ready until DB connection
established.

### Interview Punch Line

**"Liveness handles recovery; readiness controls traffic flow."**

------------------------------------------------------------------------

## 114. Why is readiness probe critical for traffic?

Readiness probes ensure only healthy Pods receive traffic.\
Without them: - Requests hit initializing Pods - Users see failures -
Deployments cause downtime

They protect availability during: - Startup - Rolling updates -
Temporary degradation

### Real-World Example

New Pods join load balancing only after successful database connection
validation.

### Interview Punch Line

**"Readiness probes protect user experience by gating traffic."**
