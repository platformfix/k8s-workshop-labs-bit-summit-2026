# Lab 6: Health Checks

Manifest for the health-checks section of the slides
([bit-summit-2026.kubernetes-training.co.uk](https://bit-summit-2026.kubernetes-training.co.uk)).

## What the lab applies

| File | What it is |
|------|------------|
| `rng-liveness-probe.yaml` | Adds a `livenessProbe` to the `rng` Deployment the slides deploy earlier, imperatively (`kubectl create deployment rng --image=ghcr.io/platformfix/k8coins-rng:v0.5.0`, exposed, then scaled to 3 replicas). Applied directly against that already-running Deployment - there's nothing to redeploy from scratch. |

There is only one manifest here on purpose. Health checks is one slide
topic with one hands-on step: adding a probe to something already
deployed and running.

## Applying it

```bash
kubectl apply -f rng-liveness-probe.yaml
kubectl rollout status deployment rng
```

`kubectl apply` merges this in as a patch against whatever's already
running - it won't reset `replicas` or anything else the earlier slides
already set up.
