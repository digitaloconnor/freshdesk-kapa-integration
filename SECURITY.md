# Security

## Reporting a Vulnerability

If you discover a security issue in this workflow, please report it via [GitHub Issues](../../issues) or directly through LinkedIn: [linkedin.com/in/tony0connor](https://www.linkedin.com/in/tony0connor/).

Do not include credentials, API keys, or live ticket data in any issue report.

---

## Data This Workflow Touches

| Data | Source | Destination | Retained |
|------|--------|-------------|---------|
| Ticket subject and description | Freshdesk | Kapa AI API | No — passed in request only |
| Ticket ID | Freshdesk | Freshdesk API (update calls) | No |
| Requester details | Freshdesk | Not forwarded to Kapa | — |
| AI-generated response | Kapa AI | Freshdesk (public reply) | In Freshdesk only |
| Kapa conversation URL | Kapa AI | Freshdesk (private note) | In Freshdesk only |

**PII note:** Ticket descriptions may contain end-user PII. This data is forwarded to the Kapa AI API. Ensure your Kapa AI data processing terms are acceptable for your use case and comply with applicable data protection obligations (GDPR etc.).

---

## Credential Security

- All credentials are stored in n8n's encrypted credential store or as environment variables
- API keys are **never** hardcoded in workflow JSON
- The `.env` file is **never** committed to version control — see `.gitignore`
- Rotate Freshdesk and Kapa API keys immediately if either is exposed

---

## Human-in-the-Loop Design

This workflow is designed so that **no customer-facing action happens without human review**:

- The AI response is posted as a **private note**, not a direct reply
- The support agent reads the AI draft and decides whether to send it
- The workflow cannot be reconfigured to auto-send without a deliberate code change

This is a structural guarantee, not a configuration option.

---

## Network

- Outbound calls: Freshdesk API, Kapa AI API
- No inbound exposure beyond the n8n webhook endpoint
- Webhook URL should be treated as a secret — rotate if exposed
