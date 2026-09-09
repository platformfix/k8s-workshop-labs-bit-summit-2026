# Lab 8: Managing Secrets

No manifest here, and none is coming. k8coins has no credential, API
key, or password anywhere in it - a genuine gap, not one glossed over
for the workshop (Lab 4's own README says the same thing).

## What the slides actually demo

The slides' Secret walkthrough is a private-registry pull secret
(`imagePullSecrets`) against a generic private image
(`docker-registry.enix.io/jpetazzo/private`) - not k8coins. k8coins'
own images live on `ghcr.io/platformfix`, which is public, so there's
no private-registry story to retell using k8coins' real images either.

Run the slides' own demo as written. The mechanics (`kubectl create
secret docker-registry`, attaching it via `imagePullSecrets` on a
Deployment or a ServiceAccount, the base64 encoding, `stringData`)
apply to any private image - there's nothing k8coins-specific to add.
