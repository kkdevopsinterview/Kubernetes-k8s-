## 56. How do you externalize application configuration in Kubernetes?

In Kubernetes, application configuration is externalized so that configuration is separated from application code.
This means we do not hard-code values like database URLs, environment variables, or feature flags inside container images.

Kubernetes provides **ConfigMaps** and **Secrets** to manage configuration externally. This allows us to:
- Change configuration without rebuilding images
- Reuse the same image across environments
- Deploy faster and safer

Applications read configuration at runtime from ConfigMaps or Secrets.

### Real-World Example
The same Docker image is used in dev, staging, and prod, but each environment uses a different ConfigMap.

### Interview Punch Line
“Externalizing configuration makes applications flexible and environment-agnostic.”

---

## 58. Why are ConfigMaps useful in deployments?

ConfigMaps are used to store **non-sensitive configuration data** such as:
- Application properties
- URLs
- Feature flags
- Environment variables

They allow us to change application behavior without rebuilding container images.

ConfigMaps also support:
- Better version control
- Easier rollbacks
- Cleaner deployments

### Real-World Example
Changing a log level from INFO to DEBUG by updating a ConfigMap instead of rebuilding the image.

### Interview Line
“ConfigMaps decouple configuration from application code.”

---

## 59. What are the ways Pods can consume ConfigMaps?

Pods can consume ConfigMaps in three main ways:
- As environment variables
- As command-line arguments
- Mounted as files inside volumes

Each method is used depending on how the application expects configuration.

### Real-World Example
A Spring Boot application reads configuration files mounted from a ConfigMap inside `/config`.

### Interview Punch Line
“ConfigMaps can be injected into Pods in multiple flexible ways.”

---

## 60. How does Kubernetes handle sensitive information?

Kubernetes handles sensitive data using **Secrets**.

Secrets store:
- Passwords
- Tokens
- API keys
- Certificates

Secrets are:
- Base64-encoded
- Stored in etcd
- Accessed securely by Pods

They are treated differently from ConfigMaps to avoid accidental exposure.

### Real-World Example
A database password is stored in a Secret and injected into a Pod as an environment variable.

### Interview Line
“Sensitive data should always be stored in Secrets, not ConfigMaps.”

---

## 61. When would you use a Secret instead of a ConfigMap?

Secrets are used when data is:
- Confidential
- Security-sensitive
- Requires restricted access

Examples include:
- Database credentials
- TLS certificates
- OAuth tokens

ConfigMaps are strictly for non-sensitive data.

### Real-World Example
Database URL in a ConfigMap, database password in a Secret.

### Interview Punch Line
“ConfigMap for config, Secret for credentials.”

---

## 62. Where are Secrets stored internally?

Secrets are stored in **etcd**, the Kubernetes key-value store.

By default:
- Secrets are base64-encoded, not encrypted
- Anyone with etcd access can read them

In production:
- Encryption at rest should be enabled
- Access to etcd must be restricted

### Interview Line
“Secrets live in etcd, so etcd security is critical.”

---

## 63. What steps do you take to secure Secrets?

To secure Secrets properly, multiple layers of security are applied:
- Enable encryption at rest for etcd
- Use RBAC to limit access
- Avoid storing Secrets in Git
- Rotate Secrets regularly
- Use external secret managers

Secrets should follow the **principle of least privilege**.

### Real-World Example
Only backend Pods can access database Secrets, frontend Pods cannot.

### Interview Punch Line
“Secrets must be protected at rest, in transit, and in access.”

---

## 64. How do you manage different configurations per environment?

Different environments like dev, stage, and prod require different configuration values.

Common approaches:
- Separate ConfigMaps per environment
- Separate namespaces per environment
- Environment-specific values files

This ensures isolation and prevents accidental misconfiguration.

### Real-World Example
`app-config-dev`, `app-config-stage`, `app-config-prod` ConfigMaps.

### Interview Line
“Environment isolation prevents configuration mistakes.”

---

## 65. What strategy do you use for dev, stage, and prod configs?

A clean and common strategy:
- One or multiple Kubernetes clusters
- Separate namespaces per environment
- Separate ConfigMaps and Secrets per namespace
- Same application image everywhere

This provides consistency and reduces deployment errors.

### Real-World Example
CI/CD deploys the same image but applies different configs per environment.

### Interview Punch Line
“Same image, different configuration.”

---

## 66. How do teams manage secrets using external tools?

In production, many teams use **external secret management tools** instead of native Kubernetes Secrets.

Common tools include:
- HashiCorp Vault
- AWS Secrets Manager
- Azure Key Vault

These tools:
- Provide stronger encryption
- Support secret rotation
- Improve auditability

Kubernetes integrates with them using operators, CSI drivers, or sidecars.

### Real-World Example
Vault injects secrets into Pods at runtime using a sidecar or CSI driver.

### Interview Line
“External secret managers improve security and compliance.”
