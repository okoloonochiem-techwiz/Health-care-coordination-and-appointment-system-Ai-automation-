# AI-Powered Healthcare Appointment & Care Coordination System Flowchart Diagram

This flowchart diagram documents the workflow described in the project README. It distinguishes the new-patient path from the returning-patient path and keeps human review in the loop for complex or escalated requests.

```mermaid
flowchart TB
    PATIENT[Patient]
    FORM[Google Form]
    SHEETS[(Google Sheets)]
    ZAPIER[Zapier intake automation]
    RID[Generate request ID]
    VALIDATE[Validate appointment request]

    PATIENT --> FORM
    FORM --> SHEETS
    SHEETS --> ZAPIER
    ZAPIER --> RID
    RID --> VALIDATE

    VALIDATE --> CHECK{Patient already exists?}

    CHECK -->|Yes| LOOKUP[Patient record lookup]
    CHECK -->|No| UUID[Generate patient UUID]

    UUID --> NEWREC[Create new patient record]
    NEWREC --> NEWQUEUE[Add to ai_review_queue\nNew patient — no history to evaluate]

    LOOKUP --> CONTEXT[Build patient context]
    CONTEXT --> CONDITIONS[Conditions lookup]
    CONTEXT --> MEDS[Medications lookup]
    CONDITIONS --> PROVIDER[Provider lookup]
    MEDS --> PROVIDER
    PROVIDER --> CLASSIFY[AI request classifier]

    CLASSIFY --> DECISION{Classification}
    DECISION -->|ROUTINE| AUTO[Auto-schedule appointment]
    DECISION -->|COMPLEX| HUMAN[Human staff review]
    DECISION -->|ESCALATE| URGENT[Urgent staff review]

    AUTO --> CONFIRM[Send appointment confirmation]
    HUMAN --> REVIEWQ[Add to ai_review_queue]
    URGENT --> REVIEWQ
    NEWQUEUE --> STAFF[Staff review / follow-up]
    REVIEWQ --> STAFF

    CONFIRM --> AUDIT[Write audit log]
    STAFF --> AUDIT
    AUDIT --> ERROR[Error handling / exception tracking]
    ERROR --> DASH[Operational dashboard]
```

## Responsibility split

- **Google Form:** patient-facing appointment request intake.
- **Google Sheets:** stores submitted intake data and structured patient/provider records.
- **Zapier:** starts the intake automation, assigns a request ID and coordinates the initial validation steps.
- **Patient lookup and UUID generation:** identifies returning patients or creates a unique ID and record for a new patient.
- **Context lookup:** retrieves available patient conditions and medication information for returning patients, then looks up suitable provider context.
- **AI classifier:** classifies requests as ROUTINE, COMPLEX or ESCALATE to support administrative routing.
- **Appointment scheduling:** handles routine, low-complexity requests automatically.
- **Human review queue:** sends complex, escalated and new-patient cases for staff attention.
- **Notifications and audit trail:** communicates outcomes and records workflow activity for operational tracking.
- **Error handling and dashboard:** supports exception tracking and workflow monitoring.

## Important workflow boundary

The AI classification is administrative decision support, not a clinical diagnosis or a substitute for professional medical judgment. New patients are placed in the review queue because there is no existing patient history to evaluate.
