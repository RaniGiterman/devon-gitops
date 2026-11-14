# Runbook: container OOMKilled

**Symptom:** pod restarts with `lastState.terminated.reason: OOMKilled`, often CrashLoopBackOff.

1. Compare the memory limit with real usage: `container_memory_working_set_bytes` peak over the last day.
2. Check git history of the deployment for a recent change to `resources.limits.memory`.
3. Fix: set the limit to at least peak usage + 30% headroom. Never set it below the request.
4. If usage grows without bound (leak), escalate to the owning team instead of raising the limit.
