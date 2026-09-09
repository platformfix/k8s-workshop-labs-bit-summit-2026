# Bonus Lab: DaemonSets

Manifests for the [DaemonSets bonus lab](https://workshop.platformfix.com/k8s/labs/bonus-daemonsets/). Self-paced, take-home. Run it on your own cluster.

## What the lab applies

Every manifest in this folder is used. There is nothing extra here.

| File | What it is |
|------|------------|
| `node-monitor-ds.yaml` | A DaemonSet putting one monitoring Pod on every node. |
| `comparison-deployment.yaml` | A plain Deployment, for contrast. Replicas land where the scheduler chooses, not one per node. |
| `log-collector-ds.yaml` | A DaemonSet reading log paths from the host, the shape most log agents take. |
