<div align="center">

# Introduction to Kubernetes

**From "What's a Pod?" to Production Confidence**

[![Workshop](https://img.shields.io/badge/Workshop-Bit%20Summit%202026-D4A843?style=for-the-badge&labelColor=0A0F1C)](https://workshop.platformfix.com/k8s/)
[![Instructor](https://img.shields.io/badge/Instructor-Steve%20Wade-D4A843?style=for-the-badge&labelColor=0A0F1C)](https://www.linkedin.com/in/stevendavidwade/)

</div>

---

This is your workshop repository. Fork it, clone your fork to your environment, and run the labs from there. Every manifest you apply today is in here, and the fork is yours to keep.

Your cluster is built and waiting for you. By the end of the day you'll have deployed, scaled, broken, and recovered a real application on it, with every manifest in your own fork to run again on Monday.

## About this repository

This repo is generated from a private source and republished wholesale, so pull requests opened against it can't be accepted. If it's updated before your cohort starts, re-fork rather than pull, because a later publish rewrites history here. Found a broken manifest? Open an issue on this repo and it'll get fixed at the source.

## What You'll Build

| Lab | What You'll Do |
|-----|----------------|
| **1. Your First Cluster** | Deploy an app, break it on purpose, read the failure, roll back. |
| **2. Networking** | Make services find each other by name. No hardcoded IPs. |
| **3. Scaling and Storage** | Autoscale under load. Storage that survives restarts. |
| **4. The Complete Application** | A real three-tier app, every pattern from the day together. |
| **5. Rolling Updates** | Update a running Deployment, break one on purpose, roll it back. |
| **6. Health Checks** | Add a liveness probe to something already running. |
| **7. Managing Configuration** | Wire a ConfigMap into that same Deployment. |
| **8. Managing Secrets** | Where k8coins doesn't need one, and where a real app would. |
| **Kubernetes + AI** | Watch an AI operate the cluster you spent the day on, in plain English. |

### Bonus Labs (Self-Paced, Take-Home)

| Lab | What You'll Do |
|-----|----------------|
| **DaemonSets** | One Pod per node, for agents and log collectors. |
| **Network Policies** | Control which services are allowed to talk to which. |
| **RBAC** | Lock down who can do what. |

Run the bonus labs on your own cluster (kind, k3s, or Docker Desktop) when you're ready.

## How to Use This Repo

1. **Fork** this repository to your own GitHub account (the Fork button, top right).
2. **Clone your fork** to your workshop environment:
   ```bash
   git clone https://github.com/<your-username>/k8s-workshop-labs-bit-summit-2026.git
   cd k8s-workshop-labs-bit-summit-2026
   ```
3. Follow along with the slides at **[bit-summit-2026.kubernetes-training.co.uk](https://bit-summit-2026.kubernetes-training.co.uk)**.

Every lab folder has a `README.md`. It says which manifests that lab applies, and what the extra ones are for.

> [!NOTE]
> Re-running these on your own cluster takes a little setup for Labs 3 and 4. The [What You Built](https://workshop.platformfix.com/k8s/labs/summary/) page says what to install and why.

## Repository Structure

```
.
├── labs/
│   ├── 01-first-cluster/         # Lab 1 manifests
│   ├── 02-networking/            # Lab 2 manifests
│   ├── 03-scaling-storage/       # Lab 3 manifests
│   ├── 04-complete-application/  # Lab 4 manifests
│   ├── 05-rolling-updates/       # Lab 5 - no manifests, README only
│   ├── 06-health-checks/         # Lab 6 manifest
│   ├── 07-managing-configuration/ # Lab 7 manifests
│   ├── 08-managing-secrets/      # Lab 8 - no manifests, README only
│   ├── bonus-daemonsets/         # Take-home
│   ├── bonus-network-policies/   # Take-home
│   └── bonus-rbac/               # Take-home
└── notes.md                      # Your scratchpad. Take notes. Plan your next step.
```

> [!IMPORTANT]
> The manifests are demo workloads with placeholder values. Never commit a real secret to a fork. Forks of public repos are public.

## Quick Links

| Resource | Link |
|----------|------|
| Slides | [bit-summit-2026.kubernetes-training.co.uk](https://bit-summit-2026.kubernetes-training.co.uk) |
| Workshop Labs | [workshop.platformfix.com/k8s](https://workshop.platformfix.com/k8s/) |
| Kubernetes Docs | [kubernetes.io/docs](https://kubernetes.io/docs/home/) |
| kubectl Cheat Sheet | [kubernetes.io/docs/reference/kubectl/cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) |

## After the Workshop

| Resource | Description |
|----------|-------------|
| [The 10 Kubernetes Landmines](https://guides.platformfix.com/kubernetes-10-landmines) | Free guide with the 25-question readiness audit. Share it with your team. |
| [Book a Platform Review](https://calendly.com/platformfixer/discovery) | 30 minutes. You describe your stack. I tell you what I'd delete first. |
| [The Deletion Digest](https://newsletter.platformfix.com) | Weekly newsletter. One idea, no fluff. |

---

<div align="center">

**Platform Fix** - Delete before you add.

[LinkedIn](https://www.linkedin.com/in/stevendavidwade/) · [Newsletter](https://newsletter.platformfix.com)

</div>
