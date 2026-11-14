# Runbook: CrashLoopBackOff after a config change

**Symptom:** container exits right after start; logs mention a missing env var or setting.

1. Read the last log lines of the previous container (`logs --previous`).
2. Diff the ConfigMap / env in git against the last healthy commit.
3. Fix: restore the renamed or removed key. Do not hard-code secrets into the deployment.
