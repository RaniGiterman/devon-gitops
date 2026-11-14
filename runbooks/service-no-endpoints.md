# Runbook: Service has no ready endpoints

**Symptom:** callers get 502/503 or connection refused; EndpointSlice for the Service is empty.

1. Compare the Service `spec.selector` with the pod template labels.
2. If pods exist but none are Ready, follow the readiness probe instead.
3. Fix: make the selector match the pod labels (change the Service, not the pods, unless the label change was intended).
