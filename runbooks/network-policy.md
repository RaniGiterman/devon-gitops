# Runbook: traffic blocked between services

**Symptom:** pods are Running and Ready, but calls between services time out. No restarts, no Kubernetes warnings.

1. List NetworkPolicies in the namespace and check git for a recently added policy.
2. A policy selecting the destination pods with no matching ingress rule denies all ingress to them.
3. Fix: add an ingress rule allowing the caller's pod labels on the needed port, or remove the policy if it was added by mistake.
