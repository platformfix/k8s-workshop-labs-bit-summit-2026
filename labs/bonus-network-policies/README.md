# Bonus Lab: Network Policies

Manifests for the [Network Policies bonus lab](https://workshop.platformfix.com/k8s/labs/bonus-network-policies/). Self-paced, take-home. Run it on your own cluster.

## What the lab applies

Every manifest in this folder is used. There is nothing extra here.

| File | What it is |
|------|------------|
| `backend-deployment.yaml` | The workload you are protecting. |
| `client-deployment.yaml` | The client that is allowed to reach it. |
| `other-deployment.yaml` | A second client that is not, so you can see a policy bite. |
| `default-deny-ingress.yaml` | Deny all incoming traffic, then open only what you need. |
| `allow-client-to-backend.yaml` | The one rule that lets the client through. |
| `allow-from-default-namespace.yaml` | Allowing traffic by namespace rather than by Pod label. |
| `restrict-client-egress.yaml` | Controlling what a Pod is allowed to call out to. |

## Your cluster needs a CNI that enforces policies

The defaults on kind, k3s, and Docker Desktop accept a NetworkPolicy and then ignore it. Nothing errors. The deny rules simply never bite, so the lab appears to fail when it is your cluster that is not enforcing.

The lab page says which CNI to install.
