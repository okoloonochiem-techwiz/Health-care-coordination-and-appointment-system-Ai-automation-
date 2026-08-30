
# AI-Powered Healthcare Appointment & Care Coordination Workflow

An automation system that streamlines patient appointment requests and care coordination using AI for classification, routing, and administrative decision support.

# What it does

- Accepts and validates incoming patient appointment requests
- Identifies whether a patient is new or returning
- Generates a unique patient ID (UUID) for new patients not yet in the system
- Retrieves and cross-references existing patient records
- Gathers relevant patient and provider context
- Matches patients with providers based on department
- Uses AI to classify requests as ROUTINE, COMPLEX, or ESCALATE
- Auto-schedules routine, low-complexity appointments
- Sends confirmation notifications for successfully scheduled appointments
- Routes complex or escalated requests to a human staff review queue
- Notifies staff when manual review is required
- Maintains structured operational records and an audit trail for tracking and workflow management

# Why

Manual appointment intake and care coordination can be repetitive, time-consuming, and prone to administrative errors.
This workflow reduces repetitive coordination tasks by automating validation, patient matching, contextual lookup, classification, and routing while keeping a human in the loop for complex or escalated cases.

# Workflow architecture 

                    PATIENT
                       ↓
                 GOOGLE FORM
                       ↓
                GOOGLE SHEETS
                       ↓
                   ZAPIER
                       ↓
                REQUEST ID
                       ↓
                  VALIDATION
                       ↓
             ┌─────────┴─────────┐
             ↓                   ↓
        EXISTING              NEW PATIENT
             ↓                   ↓
       PATIENT LOOKUP        GENERATE UUID
             ↓                   ↓
             ↓              CREATE RECORD
             ↓                   ↓
             ↓              ai_review_queue
             ↓              (new patient — no
             ↓               history to evaluate)
             ↓                   ↓
             └─────────┬─────────┘
                       ↓ (existing patients only,
                       ↓  continues to AI Classifier)
                PATIENT CONTEXT
                  ├── CONDITIONS LOOKUP
                  └── MEDICATIONS LOOKUP
                       ↓
                PROVIDER LOOKUP
                       ↓
                  AI CLASSIFIER
                       ↓
          ┌────────────┼────────────┐
       ROUTINE       COMPLEX     ESCALATE
          ↓            ↓            ↓
     appointments   HUMAN         URGENT
                    REVIEW        REVIEW
                       ↓            ↓
                  ai_review_queue (both)
          └────────────┼────────────┘
                       ↓
                  NOTIFICATION
                       ↓
                  AUDIT LOG
                       ↓
                 ERROR HANDLER
                       ↓
                  DASHBOARD  

# project structure 

HEALTHCARE-AI-CARE-COORDINATION/
│
├── data/
│   ├── raw/
│   │   ├── patients.csv
│   │   ├── providers.csv
│   │   ├── conditions.csv
│   │   └── medications.csv
│   └── operational/
│       ├── appointments.csv
│       ├── patients_new.csv
│       └── ai_review_queue.csv
├── automation/
│   └── zapier_workflow_documentation.md
├── architecture/
│   └── workflow_diagram.png
├── screenshots/
│   ├── form_screenshot.png
│   ├── zapier_flow_screenshot.png
│   └── sheet_output_screenshot.png
├── docs/
│   └── known_limitations.md
└── README.md
                  




                  
