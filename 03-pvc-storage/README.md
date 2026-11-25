# Kubernetes Persistent Storage & PVC Triage

## Project Overview

I created this lab to understand and troubleshoot common Kubernetes storage issues using a local Kind cluster.

The focus was on investigating why PersistentVolumeClaims (PVCs) remain in `Pending`, understanding the `ReadWriteOnce` (RWO) access mode, and verifying that application data remains available after a pod is deleted and recreated.

## What I Worked On

- Inspected the cluster's StorageClasses to understand dynamic provisioning and volume binding behaviour.
- Reproduced a PVC provisioning failure caused by referencing a non-existent StorageClass (`fast-ssd`).
- Investigated an expected `WaitForFirstConsumer` state where a PVC remained pending until a pod was scheduled.
- Troubleshot a scheduling issue involving the `kube-scheduler` after the local cluster experienced a sleep and resume.
- Fixed the invalid StorageClass reference by recreating the PVC with the correct configuration.
- Verified that the `orders-db` pod could successfully mount its persistent volume.
- Tested data persistence by writing a record to the mounted storage, deleting the pod, recreating it, and confirming that the record was still available.

## Issues Encountered and Resolved

| Issue | Root Cause | Resolution |
| :--- | :--- | :--- |
| `analytics-data-pvc` stuck in `Pending` | The PVC referenced a non-existent `fast-ssd` StorageClass. | Recreated the PVC using the valid `standard` StorageClass. |
| `orders-db-pvc` remained `Pending` | The StorageClass used `WaitForFirstConsumer`, which delays binding until a consumer pod is scheduled. | Created and scheduled the `orders-db` pod to trigger volume provisioning and binding. |
| `orders-db` pod was not scheduled | The local Kubernetes scheduler was recovering after the cluster experienced a sleep and resume. | Checked the control-plane components and waited for the scheduler to recover. |
| RWO storage and multiple pod replicas | RWO volumes cannot be mounted for read-write access by multiple nodes simultaneously. | Reviewed when to use StatefulSets with separate PVCs or RWX storage for shared access. |

## Verification Results

| Check | Result |
| :--- | :--- |
| PVC configuration after fixing the StorageClass | Verified |
| `orders-db-pvc` successfully bound | Bound |
| Persistent volume mounted by `orders-db` | Successful |
| Data retained after pod deletion and recreation | Verified |

Test record:

`order_id=1001 status=PAID`

The record remained available after deleting and recreating the `orders-db` pod, confirming that the data was stored on the persistent volume rather than in the pod's temporary filesystem.

## What I Learned

- How to identify the difference between a genuine PVC provisioning failure and an expected `WaitForFirstConsumer` state.
- Why an existing PVC's `storageClassName` cannot be changed directly.
- How persistent volumes retain application data independently of a pod's lifecycle.
- How StorageClass configuration affects volume provisioning and pod scheduling.
- When to use StatefulSets with RWO volumes and when shared RWX storage may be more appropriate.

## Key Takeaway

This lab helped me build a better understanding of Kubernetes persistent storage and how to troubleshoot PVC provisioning, volume binding, and data persistence issues in a cluster.
