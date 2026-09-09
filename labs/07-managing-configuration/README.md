# Lab 7: Managing Configuration

The slides' own configuration examples (blue/green load balancing
through HAProxy, a Docker registry reading its port from a ConfigMap)
generate manifests too large to type live - they live here instead.

## What the lab applies

| File | What it is |
|------|------------|
| `blue-green.yaml` | Two Namespaces (`blue`, `green`), each running `platformfix/colour` behind its own Service - the setup for the blue/green load-balancing demo. |
| `haproxy-pod.yaml` | The HAProxy Pod that balances across `color.blue.svc` and `color.green.svc`. Apply the `haproxy` ConfigMap first (the slides create it with `kubectl create configmap haproxy --from-file=haproxy.cfg` - it's small enough to type live). |
| `registry-pod.yaml` | A registry Pod reading its listen address from a ConfigMap via `configMapKeyRef`. Apply the `registry` ConfigMap first (also created live: `kubectl create configmap registry --from-literal=http.addr=0.0.0.0:80`). |
| `configmap.yaml` | Not part of the slides above - a real k8coins example instead of a generic one. One real setting, `DEBUG`, the same key k8coins' own real chart reads. |
| `rng-with-config.yaml` | Pairs with `configmap.yaml`: `rng`, updated to read it via `envFrom`. Carries the `livenessProbe` from Lab 6 forward too - `kubectl apply -f` only keeps fields it's told about, so leaving the probe out here would silently remove it. |

## Applying the slides' own examples

```bash
kubectl apply -f blue-green.yaml
# ... create the haproxy ConfigMap live, per the slides ...
kubectl apply -f haproxy-pod.yaml
# ... create the registry ConfigMap live, per the slides ...
kubectl apply -f registry-pod.yaml
```

## Applying the k8coins example

```bash
kubectl apply -f configmap.yaml
kubectl apply -f rng-with-config.yaml
kubectl rollout status deployment rng
```

Run Lab 6 first. If you haven't, `rng` still gets `DEBUG` wired in fine -
you'll just be adding the liveness probe for the first time here instead
of carrying it forward.
