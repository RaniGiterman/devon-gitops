# Postmortem: incident #1 (api)

## Summary
api in `demo` was unhealthy for 155s. The recent revert (commit 6716a53) lowered the API pod memory limit from 160Mi to 96Mi, causing OOMKills and CrashLoopBackOff. The pod’s peak usage (~120Mi) exceeds the new limit.

## Timeline (UTC)
- 2026-11-17 15:05:28 UTC  **DETECTED**: api: container api was OOMKilled 28s ago
- 2026-11-17 15:05:49 UTC  **TRIAGED**: still failing after 20s
- 2026-11-17 15:05:49 UTC  **INVESTIGATING**
- 2026-11-17 15:05:59 UTC  **FIX_PROPOSED**: api: raise memory limit back to 160Mi
- 2026-11-17 15:06:03 UTC  **AWAITING_APPROVAL**: PR #13 opened
- 2026-11-17 15:06:19 UTC  **APPLYING**: merging PR #13
- 2026-11-17 15:06:48 UTC  **VERIFYING**: ArgoCD synced 02ab8e14; watching for 60s
- 2026-11-17 15:08:04 UTC  **RESOLVED**: rollout complete and no symptoms for 60s. Time to resolve: 155s

## Root cause
The recent revert (commit 6716a53) lowered the API pod memory limit from 160Mi to 96Mi, causing OOMKills and CrashLoopBackOff. The pod’s peak usage (~120Mi) exceeds the new limit.

Caused by commit `6716a53`: reduced memory limit from 160Mi to 96Mi

Evidence:
- commit 6e6aab8 increased limits to 160Mi
- commit 6716a53 reverted to 96Mi
- peak memory 120766464 bytes (~115Mi)

## Fix
api: raise memory limit back to 160Mi (https://github.com/RaniGiterman/devon-gitops/pull/13), approved by claude-test, merged as `02ab8e14`. Restore the memory limit to 160Mi to prevent OOMKills after the recent revert.

## Detection
Detectors: sweep, watch. First signals: api: BackOff: Back-off restarting failed container api in pod api-7b5797cc4-zjvmj_demo(85d0a734-a024-4799-8fba-9ea8d6d9c3b0); api: Unhealthy: Readiness probe failed: Get "http://10.244.0.21:8080/healthz": dial tcp 10.244.0.21:8080: connect: connection refused

## Prevention
- Review changes of class `resources` carefully; this one passed review and broke api.
- Consider a policy or CI check that would have caught it before merge.
