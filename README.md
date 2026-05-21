# 🚀 LeadFlow Automation System

<div align="center">

### Production-Grade Workflow Automation Platform Built Using n8n

Automated Lead Intake • Spam Detection • Routing • Notifications • Reporting • Reliability Engineering

</div>

---

# 📌 Overview

LeadFlow Automation System is a production-style backend workflow automation platform designed using **n8n**.

The platform automates the complete lifecycle of customer lead processing, including:

* Webhook ingestion
* Validation
* Spam filtering
* Company enrichment
* Idempotency checks
* Lead storage
* Priority routing
* Slack escalation
* Trello ticket generation
* Gmail notifications
* Daily digest reporting
* Error handling & dead-letter queue logging

The architecture follows modern enterprise workflow orchestration patterns commonly used in backend automation systems.

---

# 🏗️ System Architecture

```text
┌──────────────────────┐
│   Incoming Webhook   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Validation Layer    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Spam Detection      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Spam Routing Layer   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Company Enrichment   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Idempotency Check    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Duplicate Detection  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Google Sheets Store  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Urgency Routing      │
└───────┬───────┬──────┘
        │       │
        │       │
        ▼       ▼

 HIGH PRIORITY      NORMAL PRIORITY
 ─────────────      ───────────────

 Slack Alert        Gmail Confirmation
        │                   │
        ▼                   ▼
 Trello Ticket       Final Logging Sheet
        │                   │
        └──────────┬────────┘
                   ▼
         Webhook Success Response
```

---

# ⚙️ Core Features

## ✅ Lead Intake Automation

Processes incoming customer support requests via HTTP webhook.

---

## ✅ Validation Engine

Validates mandatory payload fields before processing.

---

## ✅ Spam Detection Layer

Detects spam requests using keyword and content analysis.

---

## ✅ Company Enrichment

Extracts inferred company names from customer email domains.

Example:

```text
john@google.com → google
```

---

## ✅ Idempotency & Duplicate Protection

Prevents duplicate webhook retries and repeated lead processing.

---

## ✅ Priority-Based Routing

### High Priority Leads

* Slack alert generation
* Trello incident ticket creation

### Normal Priority Leads

* Gmail confirmation emails
* Final lead logging

---

## ✅ Daily Digest Reporting

Automatically generates operational summary reports every day at 6 PM.

Digest includes:

* Lead count by urgency
* Lead count by product
* Top 5 recent leads

---

## ✅ Reliability Engineering

Implemented production-grade reliability features:

* Retry handling
* Failure logging
* Dead-letter queue
* Error observability

---

# 🧰 Technology Stack

| Technology    | Purpose                 |
| ------------- | ----------------------- |
| n8n           | Workflow orchestration  |
| Google Sheets | Lead database & logging |
| Slack         | Incident notifications  |
| Trello        | Ticket management       |
| Gmail         | Email notifications     |
| JavaScript    | Custom logic layers     |

---

# 📂 Project Structure

```text
project/
│
├── workflows/
│   ├── leadflow_main_workflow.json
│   ├── leadflow_digest_workflow.json
│   └── leadflow_error_handler.json
│
├── payloads/
│   └── sample_payloads.json
│
├── screenshots/
│   ├── high_urgency_flow.png
│   ├── normal_flow.png
│   ├── failure_dead_letter.png
│   └── daily_digest_execution.png
│
└── README.md
```

---

# 🚀 Installation & Setup

# 1️⃣ Install Node.js

Download and install Node.js:

https://nodejs.org

Verify installation:

```bash
node -v
npm -v
```

---

# 2️⃣ Install n8n

```bash
npm install -g n8n
```

---

# 3️⃣ Start n8n

```bash
n8n start
```

n8n UI will be available at:

```text
http://localhost:5678
```

---

# 🔐 Required Credentials

Configure the following integrations inside n8n.

| Service       | Authentication  |
| ------------- | --------------- |
| Google Sheets | OAuth2          |
| Gmail         | OAuth2          |
| Slack         | OAuth2          |
| Trello        | API Key + Token |

---

# 📄 Google Sheets Setup

Create the following spreadsheets manually.

---

## 📘 LeadFlow_Leads_Database

### Required Columns

| Column Name     |
| --------------- |
| leadId          |
| customerName    |
| customerEmail   |
| message         |
| urgency         |
| product         |
| inferredCompany |
| createdAt       |

---

## 📕 LeadFlow_Failure_Logs

### Required Columns

| Column Name   |
| ------------- |
| failedPayload |
| failedNode    |
| errorMessage  |
| failedAt      |

---

# 📥 Webhook Payload Format

Example request payload:

```json
{
  "name": "John",
  "email": "john@google.com",
  "message": "Payment gateway issue",
  "urgency": "high",
  "product": "AI Platform"
}
```

---

# 🔄 Workflow Breakdown

# 🔹 Main Workflow

Handles complete customer lead lifecycle.

### Processing Steps

```text
Webhook
   ↓
Validation
   ↓
Spam Detection
   ↓
Enrichment
   ↓
Idempotency Check
   ↓
Duplicate Prevention
   ↓
Storage
   ↓
Urgency Routing
```

---

# 🔹 Daily Digest Workflow

Runs automatically every day at 6 PM.

### Responsibilities

* Fetch all leads
* Aggregate metrics
* Generate operational summaries
* Send Slack digest report

---

# 🔹 Error Handler Workflow

Captures workflow failures automatically.

### Responsibilities

* Failure detection
* Error logging
* Dead-letter queue storage
* Failure observability

---

# 🔁 Retry Strategy

Critical external integrations implement automatic retries.

| Integration   | Retry Count |
| ------------- | ----------- |
| Google Sheets | 3           |
| Slack         | 3           |
| Gmail         | 3           |
| Trello        | 3           |

Retry delay:

```text
2000 ms
```

---

# 🚨 Dead Letter Queue (DLQ)

Workflow failures are automatically captured inside:

```text
LeadFlow_Failure_Logs
```

Stored information:

* Failed payload
* Failed node
* Failure reason
* Timestamp

---

# 📊 Daily Digest Output

Daily digest includes:

```text
✔ Lead count by urgency
✔ Lead count by product
✔ Top 5 recent leads
✔ Operational summary
```

---

# 🧪 Testing Scenarios

## ✅ High Priority Flow

Expected:

* Slack alert generated
* Trello card created

---

## ✅ Normal Priority Flow

Expected:

* Gmail confirmation email sent
* Final logs updated

---

## ✅ Duplicate Lead Flow

Expected:

* Request ignored
* Duplicate prevention triggered

---

## ✅ Failure Flow

Expected:

* Error Trigger activated
* Failure stored in DLQ sheet

---

# 📸 Required Submission Screenshots

| Screenshot                 | Description              |
| -------------------------- | ------------------------ |
| high_urgency_flow.png      | Slack + Trello execution |
| normal_flow.png            | Gmail + Final logs       |
| failure_dead_letter.png    | DLQ failure capture      |
| daily_digest_execution.png | Daily digest execution   |

---

# 📦 Deliverables

The submission package contains:

```text
✔ Exported n8n workflow JSON files
✔ Setup documentation
✔ Sample webhook payloads
✔ Workflow execution screenshots
✔ Reliability & DLQ implementation
```

---

# 🔮 Future Enhancements

* PostgreSQL / MongoDB integration
* AI-powered spam classification
* Authentication & API security
* Docker containerization
* Kubernetes deployment
* Monitoring dashboards
* Multi-tenant workflow orchestration

---

# 👨‍💻 Author

Developed as part of a Backend Workflow Automation Assignment using n8n.

Designed with production-grade workflow orchestration principles and reliability engineering concepts.
