# Lab 5: Rolling Updates

Most of this lab is `kubectl` commands against a Deployment that
already exists (`worker`, deployed in Lab 4) - nothing to `apply -f`
for those. One manifest here, used at the very end.

## What the lab applies

| File | What it is |
|------|------------|
| `worker-tuned-rollout.yaml` | Updates `worker`'s image and tunes its rollout strategy (`maxUnavailable`, `maxSurge`, `minReadySeconds`) in one change - the slides' own "combine an update with a strategy change" example. |

## Applying it

```bash
kubectl apply -f worker-tuned-rollout.yaml
kubectl rollout status deployment worker
kubectl get deploy -o json worker | jq "{name:.metadata.name} + .spec.strategy.rollingUpdate"
```

Everything before this - updating the image directly, breaking a
rollout on purpose, undoing it, reading revision history - is plain
`kubectl` against the real `worker` Deployment:

```bash
# Update worker to a real, different k8coins tag
kubectl set image deployment worker worker=ghcr.io/platformfix/k8coins-worker:v0.4.1
kubectl rollout status deployment worker

# Point it at a tag that doesn't exist - a real ImagePullBackOff, not a
# staged one
kubectl set image deployment worker worker=ghcr.io/platformfix/k8coins-worker:v0.6.0
kubectl rollout status deployment worker
# Ctrl-C once it's clearly stuck, then:
kubectl get pods -l app=worker

# Undo it - lands back on v0.4.1
kubectl rollout undo deployment worker

# Roll back to the very first revision (v0.5.0, whatever this course
# deployed `worker` as originally)
kubectl rollout undo deployment worker --to-revision=1
```
