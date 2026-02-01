
# Azure Sentinel Windows Honeypot Lab

## Overview
This project deploys a publicly exposed Windows workstation honeypot in Azure and forwards Windows Security telemetry to a Log Analytics Workspace (LAW) connected to Microsoft Sentinel for detection and investigation.

**Goal:** generate real-world authentication noise, validate telemetry ingestion, and build detections + triage workflows using KQL.

## Architecture
- Resource Group: [MyLab]
- VNET: [CRP-VNET]
- VM: [WKSTN-1079] (Windows 10 Enterprise)
- Log Analytics Workspace: [LAW-1]
- Microsoft Sentinel enabled on LAW-1
- Data connector: Windows Security Events → `SecurityEvent`

> Diagram: `diagrams/architecture.png`

## Telemetry Collected
- Windows Security Events (e.g., 4625/4624/4672/4799, etc.) via Sentinel connector

## Detections (KQL)
- RDP brute force (4625) — `detections/rdp_bruteforce_4625.kql`
- First-seen attacking IP — `detections/first_seen_ip_rdp_4625.kql`
- Successful external logon (4624) — `detections/successful_external_logon_4624.kql`

## Triage / Investigation
- RDP brute force triage playbook — `investigations/triage_rdp_bruteforce.md`

## Repro Steps (High Level)
1. Deploy Windows VM in Azure with public IP and NSG allowing inbound (honeypot intentionally exposed).
2. Enable Windows Security Event collection into LAW-1 and connect Sentinel.
3. Validate ingestion with test logons (e.g., generate 4625 and confirm in `SecurityEvent`).
4. Run detections and tune thresholds based on observed baseline.

## Notes / Lessons Learned
See `docs/project-writeup.md`.
