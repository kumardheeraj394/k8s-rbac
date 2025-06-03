# Kubernetes RBAC (Role-Based Access Control)

Kubernetes uses RBAC to define **who can do what** within the cluster. There are four main components:

---

## 🔐 1. Role
- **Scope:** Namespaced  
- **Purpose:** Grants permissions **within a specific namespace**.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
---

## 🔐 2. ClusterRole
Scope: Cluster-wide

Purpose:

Grants permissions across all namespaces

Required for non-namespaced resources like nodes, persistentvolumes, etc.

yaml
Copy
Edit
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin-reader
rules:
- apiGroups: [""]
  resources: ["nodes", "pods"]
  verbs: ["get", "list"]
🔗 3. RoleBinding
Scope: Namespaced

Purpose: Assigns a Role or ClusterRole to a user, group, or service account within a namespace.

yaml
Copy
Edit
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-binding
  namespace: dev
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
🔗🌐 4. ClusterRoleBinding
Scope: Cluster-wide

Purpose: Assigns a ClusterRole to users/groups/service accounts at the cluster level.

yaml
Copy
Edit
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-nodes-binding
subjects:
- kind: User
  name: infra-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin-reader
  apiGroup: rbac.authorization.k8s.io
🔄 Summary Table
Component	Scope	Binds To	Applies To
Role	Namespace	—	Namespaced resources
ClusterRole	Cluster	—	Cluster-wide + global objects
RoleBinding	Namespace	User/Group/SA	Role or ClusterRole in a NS
ClusterRoleBinding	Cluster	User/Group/SA	ClusterRole (all namespaces)

Notes
apiGroups: [""] refers to the core API group, e.g., pods, services.

apiGroups: ["apps"] is used for resources like deployments, statefulsets.

yaml
Copy
Edit

---
