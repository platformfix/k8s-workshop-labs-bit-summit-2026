# Lab 3: Scaling and Storage

Manifests for [Lab 3](https://workshop.platformfix.com/k8s/labs/3-scaling-storage/).

## What the lab applies

| File | What it is |
|------|------------|
| `busyhttp-deployment.yaml` | A CPU-intensive web server (`jpetazzo/busyhttp`) and its ClusterIP Service. You autoscale this one. It ships with no CPU request - that gap is the point of the lab's scaling half. |
| `data-pvc.yaml` | A claim for 1Gi of storage. Names no storage class, so it binds to your cluster's default. |
| `no-volume-pod.yaml` | A Pod with no volume. Write a file to it, delete it, bring it back - the file is gone. |
| `with-volume-pod.yaml` | The same Pod, but mounting the claim from `data-pvc.yaml`. Same delete-and-recreate - this time the file survives. |

The lab creates the Horizontal Pod Autoscaler itself with `kubectl autoscale deployment busyhttp --max=10`, then adds the missing CPU request with `kubectl edit deployment busyhttp` once the autoscaler sits at `<unknown>/80%` and does nothing - both deliberately interactive steps, not something a file can pre-empt without spoiling the "aha."

## Extra, not used in the lab

Left in on purpose. Nothing in the lab applies these. Try them on your own cluster.

| File | What it demonstrates |
|------|---------------------|
| `statefulset.yaml` | A StatefulSet: stable Pod names, and a disk per Pod from `volumeClaimTemplates` rather than one shared claim. |
| `nginx-headless-service.yaml` | The headless Service that StatefulSet needs to give each Pod its own DNS name. |
| `retain-storageclass.yaml` | A `Retain` storage class you create yourself, because most clouds only hand you a `Delete` one. |
| `retain-pvc.yaml` | A claim on that `Retain` class. Delete the claim and the disk underneath survives. |
| `retain-pod.yaml` | A Pod that mounts that retain claim. |
| `xfs-pvc.yaml` | A claim asking for an XFS-backed class, rather than taking the default. Needs a cluster that can do this; see below. |
| `xfs-demo-pod.yaml` | A Pod that mounts it, so you can check the filesystem type from inside. |
| `mysql-data-pvc.yaml` | Storage for a database. |
| `mysql-deployment.yaml` | MySQL on that claim, using the `Recreate` strategy so two Pods never hold the same disk. |
| `mysql-service.yaml` | A headless Service for that database. |

Two of these need something first.

`statefulset.yaml` needs `nginx-headless-service.yaml` applied before it. `retain-pvc.yaml` needs `retain-storageclass.yaml`. `mysql-deployment.yaml` needs a Secret named `mysql-pass` with a `password` key:

```bash
kubectl create secret generic mysql-pass --from-literal=password='change-me'
```

## A note on storage classes

`statefulset.yaml` and `mysql-data-pvc.yaml` name `civo-volume`, the class on the cluster you used during the workshop. Neither of them cares which class it gets; they just need a disk.

On any other cluster that class does not exist. The claim sits in `Pending`, and `kubectl get pvc` tells you nothing about why. Ask the claim directly:

```bash
kubectl describe pvc <name>
```

The event reads `storageclass.storage.k8s.io "..." not found`.

See what you actually have, then either name one of those classes or delete the `storageClassName` line so the claim takes your default:

```bash
kubectl get storageclass
```

`retain-pvc.yaml` is the exception: for that one the class *is* the lesson, so read the next section before you change it.

### Why this lab creates its own Retain class

A storage class carries a reclaim policy, and that policy decides what happens to the disk when you delete the claim. `Delete` throws it away. `Retain` keeps it.

Most clouds only give you a `Delete` class. Civo ships exactly one class, `civo-volume`, and it is `Delete`. So pointing `retain-pvc.yaml` at the default would let it bind and then destroy the disk, which is the opposite of what this exercise is for.

Instead you create the class yourself, in `retain-storageclass.yaml`. `reclaimPolicy` is a field on the StorageClass, not something the driver decides, so you can set it on top of whatever storage your cloud provides:

```bash
kubectl apply -f retain-storageclass.yaml
kubectl apply -f retain-pvc.yaml
kubectl apply -f retain-pod.yaml
```

Change `provisioner` in that file to your own cluster's CSI driver if you are not on Civo.

Delete the Pod and then the claim, and `kubectl get pv` still lists the volume, now `Released` rather than gone. Nothing else reclaims it, so delete it yourself when you are finished, because a retained disk keeps costing money after the cluster it belonged to is gone.

### The XFS exercise needs a different cloud

`xfs-pvc.yaml` asks for an XFS filesystem rather than the default ext4. You cannot run it on Civo, and you cannot fix it by creating a class the way you just did for `Retain`.

The reason is in the driver rather than the class. `csi.civo.com` formats every volume `ext4` and mounts it `ext4`, ignoring any filesystem a StorageClass asks for. So a class carrying `csi.storage.k8s.io/fstype: xfs` would be accepted, bind, and hand you an ext4 disk: working, silently, on the wrong filesystem.

Run this one on a cloud whose CSI driver exposes the choice. DigitalOcean does, which is where `do-block-storage-xfs` comes from. On a driver that honours it, the general form is a class with:

```yaml
parameters:
  csi.storage.k8s.io/fstype: xfs
```

Check what your own driver does with that before trusting it. `xfs-demo-pod.yaml` mounts the claim so you can run `df -T /data` and see the filesystem you actually got, which is the check worth doing on any cluster.

### Claims stay Pending until a Pod turns up

On the workshop cluster, and on most clouds, the storage class binds `WaitForFirstConsumer`: the volume is not created until a Pod that mounts the claim is scheduled, so the scheduler can place the disk in the right zone.

So `data-pvc.yaml` reading `Pending` with no Pod yet is working correctly, not broken. Apply `with-volume-pod.yaml` and watch it bind - `no-volume-pod.yaml` never references the claim at all, it exists only to prove nothing survives without one.
