# Lab 4: The Complete Application

Manifests for [Lab 4](https://workshop.platformfix.com/k8s/labs/4-complete-application/).

## What the lab applies

Every manifest in this folder is used. There is nothing extra here.

| File | What it is |
|------|------------|
| `configmap.yaml` | The one real setting this app reads from its environment: `DEBUG`. Handed to all four application services uniformly - only `rng` and `worker` ever look at it, `hasher` and `webui` don't, and that's the real chart's own behaviour, not an oversight. |
| `redis-data-pvc.yaml` | The claim `redis` keeps its data on. Names no storage class, so it binds to your cluster default. |
| `redis-deployment.yaml` | `redis`, on the image the real chart uses, plus the Service that gives it a name. This is the one deliberate deviation from k8coins' own real chart: its own redis manifest has no volume at all. It's added here so the force-kill demo later in the lab has something to actually demonstrate. |
| `rng-deployment.yaml` | `rng`, plus its Service. Entirely internal, never touches `redis`. |
| `hasher-deployment.yaml` | `hasher`, plus its Service. Also never touches `redis`. |
| `worker-deployment.yaml` | `worker`, plus its Service. The only service that calls more than one neighbour - `rng` and `hasher` both. |
| `webui-deployment.yaml` | `webui`, plus its Service. Has the real dashboard; reads the result back out of `redis`. |

All five files that carry a Deployment also carry the Service that fronts it, separated by `---`. One `kubectl apply -f` creates both.

There is no Secret here. k8coins has no credential, API key, or password anywhere in it - a genuine gap, not one glossed over for the workshop.

## Reaching webui

`webui`'s Service is a ClusterIP, so `port-forward` works the same on every cluster:

```bash
kubectl port-forward svc/webui 8080:80
```

Then open `http://localhost:8080` - k8coins mining, live.

You can change the Service `type` to `LoadBalancer` and use the external address instead, but read this first if your cluster is a cloud one.

On kind or Docker Desktop that switch is free: nothing can provision a load balancer, so the Service sits in `Pending` and you have lost nothing. On a cloud it is not free. The cluster used during the workshop runs on Civo, which has no ServiceLB. A `LoadBalancer` Service there is handled by `civo-ccm`, which creates a real Civo load balancer and charges for it. Nobody has measured what one costs, so treat the cost as real rather than assuming it is small.

`kubectl delete svc webui` and reapply `webui-deployment.yaml` removes it again, and it is worth checking your provider's console afterwards rather than assuming the bill stopped.

Port-forward needs none of that, which is why the lab uses it.

## Force-killing tiers

The lab kills `hasher` (stateless - `worker` errors out, then recovers on its own) and then `redis` (stateful - the `PersistentVolumeClaim` reattaches to the replacement Pod and the data comes back with it), both with:

```bash
kubectl delete pod -l app=<name> --grace-period=0 --force
```

Nothing here is cleaned up afterwards on purpose - the stack has to survive into the following AI demo. `kubectl get pv` is worth running by hand once you do tear it down: deleting a Deployment doesn't touch its `PersistentVolumeClaim`, and anything still listed as `Released` is a disk nobody is using and somebody is still paying for.
