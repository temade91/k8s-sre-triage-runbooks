# Kubernetes Resource Management & OOMKilled Triage

## Project Overview

I built this lab to understand how Kubernetes handles container memory limits and namespace resource management.

I reproduced two common issues on a local Kind cluster: containers being terminated with `OOMKilled` and pods failing to start because a namespace had reached its `ResourceQuota` limit.

The goal was to investigate the root causes, apply the appropriate fixes, and verify that the workload could run successfully within the available resources.

## What I Worked On

- Created a `prod-inventory` namespace with a strict `256Mi` memory ResourceQuota.
- Deployed a 3-replica `inventory-cache` workload with containers attempting to allocate more memory than their configured limits.
- Investigated why containers were repeatedly terminated with `OOMKilled` and Exit Code `137`.
- Identified why the third replica could not be created after the namespace reached its memory quota.
- Updated the ResourceQuota to provide sufficient memory capacity for the workload.
- Increased container memory limits to accommodate the application's memory usage.
- Added a LimitRange to provide default memory requests and limits for containers without explicit resource specifications.
- Verified that all three replicas were running successfully after the changes.

## Issues Encountered and Resolved

| Issue | Root Cause | Resolution |
| :--- | :--- | :--- |
| Containers terminated with `OOMKilled` | The workload attempted to allocate approximately `150Mi` of memory while the container limit was `128Mi`. | Increased the memory limit to `256Mi` and the request to `192Mi`. |
| Third replica failed to create | The namespace ResourceQuota was exhausted by the first two replicas. | Increased the namespace memory quota to `1Gi`. |
| Pods without resource specifications rejected | The namespace enforced memory requests and limits through ResourceQuota. | Added a LimitRange to apply default memory requests and limits automatically. |

## Verification Results

| Check | Result |
| :--- | :--- |
| Deployment rollout | Successful |
| Running replicas | `3/3` |
| Container restarts | `0` |
| Memory limits usage | `768Mi / 1Gi` |
| Memory requests usage | `576Mi / 1Gi` |
| LimitRange | Active |

The workload was successfully deployed with all three replicas running and no container restarts after the fixes.

## What I Learned

- How exceeding a container's memory limit can result in `OOMKilled` and Exit Code `137`.
- The difference between memory limits and CPU limits, and how Kubernetes handles each.
- How ResourceQuota controls resource consumption at the namespace level.
- Why a ReplicaSet can fail to create additional pods when a namespace quota is exhausted.
- How LimitRange provides default resource requests and limits for containers.
- How ResourceQuota and LimitRange work together to improve resource management and prevent uncontrolled resource consumption.

## Key Takeaway

This lab helped me understand how to troubleshoot memory-related failures and manage resource consumption in Kubernetes namespaces.

It also gave me practical experience investigating pod creation failures, adjusting resource configurations, and verifying workload stability after remediation.
