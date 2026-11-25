# Kubernetes SRE Triage Runbooks

A collection of hands-on Kubernetes troubleshooting labs where I reproduce common production-style failures, investigate their root causes, and document the steps used to resolve them.

Built and tested using a local Kind cluster.

## Repository Structure

| Runbook | Focus Area | Key Issues |
| :--- | :--- | :--- |
| [01-probes-crashloop](./01-probes-crashloop/) | Container Lifecycle & Health Checks | `CrashLoopBackOff`, probe misconfigurations, port mismatches |
| [02-network-isolation](./02-network-isolation/) | Network Security & Ingress | NetworkPolicy restrictions, cross-namespace traffic, DNS connectivity |
| [03-pvc-storage](./03-pvc-storage/) | Persistent Storage & PVC Triage | PVC provisioning failures, `WaitForFirstConsumer`, RWO persistence |

## What This Repository Demonstrates

- Kubernetes troubleshooting and root cause analysis
- Pod health checks and lifecycle management
- NetworkPolicy and ingress troubleshooting
- PersistentVolumeClaim and StorageClass management
- Data persistence and storage access modes

Each runbook includes the failure scenario, investigation steps, root cause, resolution, and verification.
