# Control Mapping Model (NIST SP 800-53)

## Overview

This document describes the OpenGRC Framework approach for mapping security and privacy controls to implementation artifacts, evidence, and operational telemetry.

The objective is to enable a vendor-neutral, automation-ready method for tracking **NIST SP 800-53** control implementation and verification across the RMF lifecycle. This model is designed to support **Release 5.2.0 or subsequent revisions** of the control catalog.

This framework does not replace federal standards or assessment procedures. It provides a structured way to represent control status, ownership, evidence, and continuous monitoring signals.

---

## Goals

The Control Mapping Model is intended to:

- Provide a consistent data model for control implementation tracking
- Link controls to system components, responsible parties, and artifacts (SSP/SAP/SAR/POA&M)
- Support evidence traceability and versioning
- Enable integration with DevSecOps and security telemetry sources
- Improve continuous monitoring updates by connecting telemetry → evidence → control state

---

## Core Concepts

### 1) Control Object

A **Control Object** represents a single control (or control enhancement) and its lifecycle state.

Recommended fields:

- **control_id** (e.g., `AC-2`, `IA-5(1)`)
- **control_title**
- **control_family** (e.g., Access Control, Identification and Authentication)
- **baseline** (Low / Moderate / High and/or overlay references)
- **implementation_status** (Planned / Implemented / Partially Implemented / Not Implemented / N/A)
- **implementation_statement** (SSP-aligned narrative or structured summary)
- **responsible_role** (e.g., ISSO, DevSecOps Lead, System Owner)
- **system_components** (applications, services, networks, endpoints, data stores)
- **inheritance** (inherited / shared / system-specific)
- **evidence_links** (documents, scan outputs, configurations, tickets)
- **test_procedures** (SAP-aligned references)
- **assessment_results** (SAR-aligned outcomes)
- **poam_links** (associated weaknesses/remediation items)
- **last_reviewed_date**
- **version_history / change_log**

---

### 2) Control Implementation Types

Controls are implemented through different mechanisms. The model supports categorizing controls by type to better map evidence:

- **Technical** (e.g., MFA enforcement, logging configuration)
- **Administrative** (e.g., policy, procedures, training records)
- **Operational** (e.g., monitoring, incident response execution)

Many controls include a combination of these.

---

### 3) Evidence Objects

An **Evidence Object** is any artifact used to demonstrate implementation and operational verification.

Evidence may include:

- Policy documents and SOPs
- Architecture diagrams
- Configuration baselines
- Access control lists
- Audit logs
- Vulnerability scan reports
- CI/CD security pipeline outputs
- Change management records
- Incident response records
- Training completion records

Recommended evidence fields:

- **evidence_id**
- **evidence_type** (policy, config, scan, ticket, log, report, etc.)
- **source** (tool name or repository reference)
- **timestamp / collection_window**
- **integrity / provenance notes** (where it came from, how captured)
- **mapped_controls** (one-to-many mapping)
- **storage_reference** (link or location pointer)

---

### 4) Telemetry-to-Control Mapping

Continuous monitoring improves when operational telemetry is mapped to controls systematically.

Telemetry sources may include:

- SAST/DAST results
- Vulnerability scanners
- CSPM tools
- SIEM/SOAR alerts
- IAM/IdP access events
- Endpoint/EDR posture signals
- Configuration drift detection

The model maps telemetry events to:

**Telemetry Signal → Evidence Object → Control(s) → Control Status / POA&M Updates**

This creates traceable compliance updates rather than manual reporting.

---

## Control-to-Artifact Linkage

The model supports mapping controls to RMF/ATO artifacts:

- **SSP**: implementation statements, responsible roles, inheritance, component mapping
- **SAP**: assessment procedures and expected evidence types
- **SAR**: results, findings, risk statements mapped to controls
- **POA&M**: remediation items tied back to specific control deficiencies
- **Continuous Monitoring**: ongoing evidence refresh and telemetry linkage

---

## Workflow States (Control Lifecycle)

Recommended lifecycle states for each control:

1. **Planned**
2. **Implemented**
3. **Verified** (evidence + assessment validation)
4. **Monitoring** (continuous telemetry mapped)
5. **Degraded / At Risk** (telemetry indicates drift or deficiency)
6. **Remediation In Progress** (POA&M active)
7. **Restored / Re-Verified**

These states support a compliance lifecycle that evolves with operational reality.

---

## Example Control Mappings (Illustrative)

### Example 1: IA-2 (Identification and Authentication)

- **Implementation**: MFA enforced via centralized IdP
- **Evidence**:
  - IdP configuration export (policy rules)
  - System access logs showing MFA events
  - Account provisioning SOP
- **Telemetry**:
  - IdP authentication logs (MFA challenges/success/failure rates)
  - Alerts on suspicious login patterns
- **Artifacts**:
  - SSP implementation statement
  - SAP assessment procedure reference
  - SAR results mapped to IA-2 testing
- **POA&M Link**:
  - Any missing MFA enforcement for privileged roles

---

### Example 2: AU-2 (Audit Events) / AU-6 (Audit Review)

- **Implementation**: centralized logging with defined event categories
- **Evidence**:
  - Logging policy
  - SIEM ingestion configuration
  - Sample log records and retention settings
- **Telemetry**:
  - SIEM alert metrics and dashboards
  - Log ingestion health signals
- **Lifecycle Notes**:
  - If ingestion fails, AU controls shift to “Degraded / At Risk”

---

### Example 3: CM-6 (Configuration Settings)

- **Implementation**: baseline hardening and drift detection
- **Evidence**:
  - Configuration baselines
  - Approved change records
  - Hardening scripts or profiles
- **Telemetry**:
  - Drift detection outputs
  - Patch compliance status
- **POA&M Link**:
  - Drift exceptions and overdue remediation

---

### Example 4: RA-5 (Vulnerability Monitoring and Scanning)

- **Implementation**: recurring scans + triage workflow
- **Evidence**:
  - Scan schedules
  - Reports and tickets created from findings
  - SLA remediation policy
- **Telemetry**:
  - Vulnerability trend metrics
  - MTTR reporting
- **Automation Link**:
  - Findings can populate POA&M objects automatically (workflow-driven)

---

## Alignment With Vendor-Neutral Architecture

The Control Mapping Model is intentionally tool-agnostic.

- Evidence and telemetry are modeled through **abstract objects** and **source references**
- Organizations may integrate any security stack (commercial or open-source)
- The same control mapping structure supports different environments (cloud, hybrid, on-prem)

---

## Intended Outcome

This model helps shift compliance execution from static documentation toward a **traceable, workflow-driven lifecycle** where:

- controls are represented as structured objects,
- evidence is linkable and verifiable,
- telemetry supports continuous monitoring,
- POA&M items are systematically tied to control deficiencies,
- authorization readiness is more transparent and repeatable.

This supports modernization of governance execution while remaining aligned with established federal standards.
