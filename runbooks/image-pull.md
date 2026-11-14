# Runbook: ImagePullBackOff

**Symptom:** pod stuck in `ErrImagePull` / `ImagePullBackOff`.

1. Check the event message: "not found" means a bad tag; "unauthorized" means registry credentials.
2. Find the commit that changed the image tag.
3. Fix: revert to the last tag that ran successfully. Typos in tags are the most common cause.
