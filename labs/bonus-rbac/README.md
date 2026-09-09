# Bonus Lab: RBAC

Manifests for the [RBAC bonus lab](https://workshop.platformfix.com/k8s/labs/bonus-rbac/). Self-paced, take-home. Run it on your own cluster.

## What the lab applies

| File | What it is |
|------|------------|
| `pod-and-service-reader-role.yaml` | A Role listing what is allowed, scoped to one namespace. |
| `read-pods-and-services-rolebinding.yaml` | The RoleBinding tying that Role to the `app-sa` ServiceAccount. |
| `rbac-test-pod.yaml` | A Pod running `kubectl` under `app-sa`, so you can test permissions from inside the cluster. |
| `app-sa-edit-rolebinding.yaml` | The deliberate over-grant, binding `app-sa` to the built-in `edit` ClusterRole. The app can then read every Secret in the namespace. |
| `pod-and-service-reader-role-v2.yaml` | The same Role, widened, so you can watch a permission appear. |
| `node-reader-clusterrole.yaml` | A ClusterRole, for resources that are not namespaced. |
| `read-nodes-clusterrolebinding.yaml` | The ClusterRoleBinding that grants it. |
| `app-sa-view-rolebinding.yaml` | Binding a built-in cluster role instead of writing your own. Optional, in the "Finished early?" box. |

The lab creates the `app-sa` ServiceAccount by hand with `kubectl create serviceaccount`.

## Extra, not used in the lab

| File | What it demonstrates |
|------|---------------------|
| `rbac-test-app-deployment.yaml` | A normal Deployment running under `app-sa`, rather than the single test Pod the lab uses. This is how a real workload picks up an identity: you name the ServiceAccount in the Pod spec and every replica inherits it. |
| `troubleshoot-pod.yaml` | A second Pod running `kubectl` under `troubleshoot-sa`, so you can hold two identities side by side and compare what each one is allowed to do. |
| `pod-reader-rbac.yaml` | A Role and its RoleBinding in a single file, separated by `---`, granting `troubleshoot-sa` read access to Pods. The lab splits the two across separate files; this is the same grant written the other way. |

`rbac-test-app-deployment.yaml` needs `app-sa` to exist first, and the other two need `troubleshoot-sa`. A Pod naming a ServiceAccount that does not exist is refused outright, and a Deployment reports `FailedCreate` rather than starting. The lab creates only `app-sa`, so create the second one yourself before applying these:

```bash
kubectl create serviceaccount troubleshoot-sa
```
