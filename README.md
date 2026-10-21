Markdown
# 🛡️ A2P 10DLC Compliance & Opt-In Guardrail Engine (n8n Middleware)

An automated middleware pipeline engineered in **n8n** that enforces **A2P 10DLC compliance**, sanitizes lead data, normalizes phone numbers into strict **E.164 format**, dynamically appends mandatory opt-out keywords (`STOP`), and suppresses non-compliant SMS dispatches to protect carrier reputation and prevent Twilio sub-account bans.

---

## 🎯 Problem Statement
Under US carrier regulations (AT&T, T-Mobile, Verizon), automated SMS dispatches without explicit consent or required opt-out language lead to heavy carrier filtering, failed message deliveries, and account suspensions. Relying solely on downstream CRM settings introduces compliance risks when leads enter from external webhooks, ads, or third-party forms.

---

## ⚙️ Solution Architecture

                  [Webhook Intake]
                         │
                         ▼
         [JavaScript Compliance Node]
   (Regex, Consent Check, E.164, STOP Injection)
                         │
                         ▼
                 [If/Else Branching]
                  /               \
         (True)  /                 \  (False)
                /                   \
[HTTP Request (SMS Dispatch)]   [Log Compliance Failure]
   (Sanitized Payload)             (Audit Log & Suppress)

### Key Technical Features
1. **Consent Flag Validation:** Verifies explicit boolean consent (`optInConsentChecked === true`) before approving execution.
2. **E.164 Phone Normalization:** Strips non-digit characters and normalizes US/CA phone numbers into standardized international format (`+1XXXXXXXXXX`).
3. **Dynamic Keyword Injection:** Scans message body using RegEx and automatically appends `Reply STOP to cancel.` if opt-out language is missing.
4. **Conditional Routing & Audit Logging:** Suppresses non-compliant sends, diverting them to an audit path to protect sub-accounts from carrier fines and delivery blocks.

---

## 📸 Execution Trace

### 1. Compliant Execution Path (Path A)
Leads with explicit consent are formatted, injected with compliance keywords, and routed to the primary API endpoint for dispatch.

### 2. Non-Compliant Execution Path (Path B)
Leads lacking explicit consent are intercepted, suppressed, and logged into an audit trail:

![Execution Path B](assets/canvas-execution.png)

---

## 🚀 Quickstart & Deployment

### Prerequisites
* **n8n** instance (Cloud or Self-hosted)
* Basic familiarity with importing workflows in n8n

### Installation Steps
1. Clone this repository:
   ```bash
   git clone [https://github.com/arfa-data/n8n-a2p10dlc-compliance-guardrail.git](https://github.com/arfa-data/n8n-a2p10dlc-compliance-guardrail.git)
