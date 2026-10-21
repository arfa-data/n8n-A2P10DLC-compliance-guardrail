# 🛡️ A2P 10DLC Compliance Guardrail Engine (n8n Middleware)

An automated n8n middleware engine that enforces A2P 10DLC compliance before sending SMS messages. It sanitizes lead data, normalizes phone numbers into strict E.164 format (+1XXXXXXXXXX), dynamically appends mandatory opt-out keywords ("STOP"), and suppresses non-compliant dispatches to protect carrier reputation and avoid Twilio bans.

---

## ⚙️ Solution Architecture

[Webhook Intake] ──► [JavaScript Compliance Node] ──► [If/Else Branching]
                                                           ├── (True)  ──► [HTTP Request (SMS Dispatch)]
                                                           └── (False) ──► [Log Compliance Failure (Audit Suppress)]

---

## ✨ Key Features
* Consent Verification: Blocks sends if optInConsentChecked is false.
* Phone Normalization: Converts phone numbers into standard E.164 format (+1XXXXXXXXXX).
* Auto Opt-Out Injection: Automatically appends "Reply STOP to cancel" if missing from message body.
* Audit Logging: Logs suppressed leads for compliance tracking without firing SMS APIs.

---

## 🧪 Quick Test Payloads

Compliant Payload (Sends SMS):
{
  "fullName": "Jane Smith",
  "phone": "5559876543",
  "optInConsentChecked": true,
  "messageBody": "Hi Jane, your quote is ready!"
}

Non-Compliant Payload (Blocks & Logs):
{
  "fullName": "John Doe",
  "phone": "5551234567",
  "optInConsentChecked": false,
  "messageBody": "Hey John, checking in."
}

---

## 🛠️ Tech Stack
* n8n (Workflow Automation Engine)
* JavaScript ES6 (Data Transformation & RegEx)
* REST APIs & Webhooks
