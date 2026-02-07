
## 122. How does Kubernetes handle authorization using RBAC?

Kubernetes uses RBAC (Role-Based Access Control) to decide who can do
what in the cluster.

First, a user or Pod is authenticated (identity is verified). Then
Kubernetes checks RBAC rules to see if the action is allowed.

RBAC uses four main objects:

-   Role / ClusterRole → define permissions
-   RoleBinding / ClusterRoleBinding → assign permissions

This allows very detailed control, like:

-   read-only access
-   access limited to one namespace
-   full admin rights

### Real-World Example

Developers can view Pods but cannot delete them. Admins can do
everything.

### Interview Line

"RBAC controls access by defining permissions and assigning them using
bindings."

------------------------------------------------------------------------

## 123. What is the purpose of a Role?

A Role defines permissions inside one namespace only.

It tells Kubernetes:

-   What actions are allowed (get, list, create, delete)
-   On which resources (Pods, Services, Deployments)

This keeps access limited and secure.

### Real-World Example

A developer allowed to manage Deployments only in the dev namespace.

### Interview Line

"A Role gives namespace-level permissions."

------------------------------------------------------------------------

## 124. When do you use a ClusterRole?

A ClusterRole gives permissions across the entire cluster, not just one
namespace.

Used when access is needed to:

-   all namespaces
-   cluster-level resources (nodes, storage, metrics)

### Real-World Example

Monitoring tools that need to see all nodes.

### Interview Line

"ClusterRoles are used for cluster-wide access."

------------------------------------------------------------------------

## 125. How do Role and ClusterRole differ?
### KK FUNDA

**Role** - Works inside one namespace - Limited scope

**ClusterRole** - Works across the whole cluster - Bigger scope

ClusterRoles can also be applied to one namespace using bindings.

### Real-World Example

Role for developers\
ClusterRole for admins

### Interview Line

"The main difference is scope --- namespace vs cluster."

------------------------------------------------------------------------

## 126. What does a RoleBinding do?

A RoleBinding connects:

-   a Role
-   to a user or ServiceAccount

It activates the permissions.

Without binding, the Role does nothing.

### Real-World Example

Binding a developer user to a namespace Role.

### Interview Line

"RoleBinding assigns permissions to identities."

------------------------------------------------------------------------

## 127. Why is ClusterRoleBinding powerful?

ClusterRoleBinding gives cluster-wide permissions.

Because it affects the whole cluster:

-   It must be used carefully
-   Too much access can create security risks

Always follow least-privilege.

### Real-World Example

Giving admin access to cluster operators.

### Interview Line

"ClusterRoleBinding grants global access, so it must be controlled."

------------------------------------------------------------------------

## 128. Why are ServiceAccounts used by Pods?

In Kubernetes, Pods need a way to identify themselves when talking to
the Kubernetes API server. ServiceAccounts provide this identity.

When a Pod starts, Kubernetes automatically assigns it a ServiceAccount
and gives it a token. This token is used to:

-   Authenticate the Pod
-   Check permissions using RBAC
-   Allow controlled access to cluster resources

This means Pods don't act anonymously --- they follow security rules.

### Real-World Example

A backend Pod reading ConfigMaps or listing Pods through the API using
its ServiceAccount token.

### Interview Line

"ServiceAccounts give Pods a secure identity to interact with
Kubernetes."

------------------------------------------------------------------------

## 129. When would you bind a ServiceAccount to a ClusterRole?

Sometimes a Pod needs access beyond one namespace --- maybe across the
entire cluster.

In this case:

-   The Pod uses a ServiceAccount
-   That ServiceAccount is bound to a ClusterRole
-   This gives the Pod cluster-level permissions

This is common for infrastructure tools that need visibility everywhere.

But permissions should always be limited to only what is necessary.

### Real-World Example

A monitoring agent collecting metrics from all nodes and namespaces.

### Interview Line

"Binding a ServiceAccount to a ClusterRole gives Pods controlled
cluster-wide access."

------------------------------------------------------------------------

## 130. How do Pods authenticate with the API server?

Pods authenticate using tokens provided by their ServiceAccount.

Here's what happens:

1.  Kubernetes creates a token
2.  Token is automatically mounted inside the Pod
3.  Pod uses the token when calling the API server
4.  API server verifies identity before allowing actions

This ensures secure communication between Pods and Kubernetes.

### Real-World Example

A controller Pod listing Deployments using its token.

### Interview Line

"Pods authenticate using ServiceAccount tokens mounted inside
containers."

------------------------------------------------------------------------

## 131. How do you restrict user or Pod permissions?

Restricting permissions is very important for cluster security.

This is done by:

-   Creating Roles with minimal permissions
-   Binding them only where needed
-   Using namespace separation
-   Regularly auditing access

This follows the least privilege principle, meaning nobody gets extra
permissions.

### Real-World Example

A CI/CD pipeline allowed only to update deployments, not delete
resources.

### Interview Line

"Permissions are restricted by giving only the minimum access required."
