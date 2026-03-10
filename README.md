# Healthcare-n8n-Workflows

> **HIPAA-Compliant Automation Templates for Healthcare Operations**
> *by Muhammad Junaid Khan, PharmD*

---

## Overview

This repository documents n8n-based workflow automation frameworks designed for healthcare operations environments. These templates address the most common operational bottlenecks in clinical coordination, patient support programs, and healthcare administration.

All workflows are designed with HIPAA compliance principles at their foundation — data minimisation, secure routing, and audit-trail integrity.

---

## Workflow Categories

### Patient Coordination Automation

**Referral Follow-Up Workflow**
Automates outbound follow-up for pending diagnostic referrals. Triggers on scheduled intervals, checks referral status in EHR, and routes escalation alerts to clinical coordinators when timelines are exceeded.

```
Trigger: Schedule (daily 9am)
  → Pull pending referrals from EHR API
  → Filter: overdue > 48 hours
  → Branch: urgent / standard
  → Urgent → RingCentral alert to coordinator
  → Standard → Email reminder to clinic
  → Log outcome to Google Sheets audit trail
```

**MRI Appointment Confirmation Workflow**
Reduces missed MRI appointments through automated confirmation and rescheduling logic.

```
Trigger: 48h before appointment
  → Fetch patient contact from AdvancedMD
  → Send confirmation via preferred channel
  → Wait 4 hours for response
  → No response → escalate to coordinator queue
  → Confirmed → update EHR status
```

---

### Pharmacy Operations Automation

**Prescription Tracking Workflow**
Monitors prescription fulfillment lifecycle for pharmacy clients, with escalation triggers for stuck or delayed orders.

```
Trigger: Webhook from pharmacy system
  → Parse order status payload
  → Route by status: pending / fulfilled / delayed
  → Delayed → notify account manager
  → Log all events to audit database
```

**Medication Adherence Follow-Up**
Proactive outreach workflow for patients on long-term medication programs, supporting 83%+ adherence target achievement.

```
Trigger: Schedule (monthly, patient-specific)
  → Pull active patient list from PSP database
  → Check last refill date
  → Overdue → trigger outreach sequence
  → Log interaction outcome
```

---

### Compliance & Documentation Automation

**HIPAA Audit Preparation Workflow**
Automated compilation of documentation required for HIPAA audit readiness reviews.

```
Trigger: Manual / Scheduled quarterly
  → Pull PHI access logs
  → Cross-reference with authorised access list
  → Flag discrepancies
  → Generate audit-ready summary report
  → Route to compliance officer
```

**Incident Documentation Routing**
Ensures adverse event reports and clinical incidents are documented and routed within regulatory timeframes.

```
Trigger: Intake form submission
  → Classify: AE / near-miss / complaint
  → AE → route to PV coordinator within 24h
  → Assign case ID and timestamp
  → Notify relevant stakeholders
  → Schedule 7-day follow-up check
```

---

## Implementation Notes

These workflows are built for n8n (self-hosted preferred for HIPAA environments). Key configuration considerations:

- All webhooks should operate over HTTPS with verified certificates
- Patient identifiers should be handled as environment variables, never hardcoded
- All audit logs should write to append-only storage
- Access to workflow execution history should be role-restricted
- Regular credential rotation recommended (90-day cycle)

---

## Tools Referenced

| Tool | Role |
|------|------|
| n8n | Primary workflow engine |
| Athena Health API | EHR data source |
| AdvancedMD API | EMR data source |
| RingCentral | HIPAA-compliant communication |
| Google Sheets | Audit logging |
| Typebot | Patient-facing intake flows |

---

## Related Repositories

- [clinical-operations-systems](../clinical-operations-systems) — Operational frameworks and SOP architecture
- [pharmacovigilance-workflows](../pharmacovigilance-workflows) — ADR and safety reporting automation
- [digital-health-knowledge-systems](../digital-health-knowledge-systems) — Knowledge infrastructure and documentation

---

*Muhammad Junaid Khan, PharmD | [LinkedIn](https://www.linkedin.com/in/dr-mjunaidkhan-rph/) | junaidkhan.rph@gmail.com*
