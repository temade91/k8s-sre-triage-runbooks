# Kubernetes SRE Triage Runbooks

A collection of hands-on Kubernetes troubleshooting labs where I reproduce common production-style failures, investigate their root causes, and document the steps used to resolve them.

Built and tested using a local Kind cluster.

## Repository Structure

| Runbook | Focus Area | Key Issues |
| :--- | :--- | :--- |
| [01-probes-crashloop](./01-probes-crashloop/) | Container Lifecycle & Health Checks | `CrashLoopBackOff`, probe misconfigurations, port mismatches |
| [02-network-isolation](./02-network-isolation/) | Network Security & Ingress | NetworkPolicy restrictions, cross-namespace traffic, DNS connectivity |
| [03-pvc-storage](./03-pvc-storage/) | Persistent Storage & PVC Triage | PVC provisioning failures, `WaitForFirstConsumer`, RWO persistence |
| [04-oom-quotas](./04-oom-quotas/) | Resource Management & OOMKilled Triage | `OOMKilled`, `ResourceQuota` failures, `LimitRange` defaults |

## What This Repository Demonstrates

- Kubernetes troubleshooting and root cause analysis
- Container resource requests and limits
- Namespace resource management
- ResourceQuota and LimitRange configuration
- Persistent storage and PVC management
- NetworkPolicy and application health check troubleshooting

Each runbook documents the failure scenario, investigation, root cause, resolution, and verification.
