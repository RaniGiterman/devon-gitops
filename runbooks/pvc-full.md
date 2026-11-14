# Runbook: PersistentVolume filling up

**Symptom:** `KubePersistentVolumeFillingUp` alert; later, write errors in the app.

1. Check growth rate (`kubelet_volume_stats_used_bytes`) and what is writing.
2. If growth is expected, expand the PVC (StorageClass must allow expansion), at most 2x at a time.
3. If something is writing without bound (debug logging, missing compaction), escalate; expanding only delays the outage.
