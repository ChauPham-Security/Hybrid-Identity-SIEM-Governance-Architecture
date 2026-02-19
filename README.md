# Hybrid Identity & SIEM Governance Architecture  
### Azure-Based Identity-First Security Design

---

## Overview

This project demonstrates the design and implementation of a hybrid identity-first security architecture using Microsoft Azure.

The objective was to simulate how an enterprise can:

- Centralize identity control  
- Enforce least-privilege access  
- Reduce credential abuse risk  
- Aggregate hybrid identity and infrastructure telemetry  
- Detect identity-driven threats  
- Provide governance-level visibility  

The architecture follows a layered security model:

**Identity Control → Access Governance → Hybrid Workload → SIEM Monitoring → Governance Reporting**

---

## The Problem

Hybrid environments frequently struggle with:

- Over-privileged administrative access  
- Standing admin permissions  
- Limited visibility into identity-related risk  
- Disconnected identity and infrastructure logs  

This project was designed to address those challenges using an identity-first Zero Trust approach.

---

## Architecture Flow

Users  
→ Microsoft Entra ID (Identity Control Layer)  
→ Azure RBAC & Conditional Access (Governance Layer)  
→ Azure Virtual Machine (Hybrid Workload Simulation)  
→ Log Aggregation (Identity + Infrastructure)  
→ Microsoft Sentinel (SIEM)  
→ Governance Dashboard & Detection Rules  


---

## Step 1 — Centralized Identity Control

Microsoft Entra ID was configured as the authoritative identity provider.

### Controls Implemented

- Security groups created for role separation  
- Role-Based Access Control (RBAC) assigned to groups (not individual users)  
- Multi-Factor Authentication (MFA) enforced for all users  
- Break-glass emergency account excluded from Conditional Access policies  

### Outcome

- Reduced privilege sprawl  
- Standardized access control model  
- Improved identity governance scalability  

---

## Step 2 — Access Governance & Zero Trust Enforcement

Zero Trust principles were applied to administrative and user access.

### Governance Controls

- Conditional Access policies enforcing MFA  
- Legacy authentication blocked  
- Privileged Identity Management (PIM) for Just-In-Time (JIT) admin access  
- Time-bound role activation with approval and justification  

### Security Impact

- Eliminated standing administrative privileges  
- Reduced risk of credential abuse  
- Enforced verification before privilege elevation  

---

## Step 3 — Hybrid Workload Simulation

An Azure Virtual Machine was deployed to simulate hybrid infrastructure.

The VM was:

- Integrated with Entra ID identity controls  
- Onboarded to Microsoft Defender for Endpoint  
- Configured to send security logs to Azure Monitor  

This simulated an enterprise workload operating under centralized identity governance.

---

## Step 4 — SIEM Integration (Microsoft Sentinel)

Microsoft Sentinel was configured to ingest:

- Entra ID Sign-in Logs  
- Entra ID Audit Logs  
- Azure Activity Logs  
- VM Security Events  
- Microsoft Defender alerts  

This created a unified monitoring layer combining identity and infrastructure telemetry.

---

## Step 5 — Detection Engineering (KQL)

Custom KQL-based detections were developed to monitor identity risk.

### Privilege Escalation Monitoring

```kql
AuditLogs
| where OperationName contains "Add member to role"
