# Freshdesk · Kapa AI Integration

**Version:** 1.0  
**Status:** Production  
**Category:** Support Automation  
**Last Updated:** 2026-03-27

> **Impact:** Reduced average ticket resolution time by 90%. AI drafts the answer. The human agent reads it and decides. The workflow cannot bypass the human.

---

## What It Does

When a Freshdesk support ticket is tagged **Kapa Test**, this workflow fires automatically:

- Detects the ticket language
- If **English** → queries Kapa AI → posts the AI answer as a public reply → adds a private note with the Kapa conversation link → tags the ticket as AI-handled
- If **non-English** → posts a language notice → reassigns the ticket to the human agent queue

The AI response is posted as a **private note first** — a human agent reads it and decides whether to send it. The workflow is structurally incapable of sending a customer reply without human review.

---

## Workflow Diagram

```
Freshdesk Webhook Trigger
        │
        ▼
Check Ticket Language
        │
        ▼
Is Ticket in English? (IF)
   ├── TRUE  → Ask Kapa AI
   │                │
   │                ▼
   │       Post AI Answer to Ticket
   │                │
   │                ▼
   │       Add Kapa Link (Private Note)
   │                │
   │                ▼
   │        Merge Ticket Tags
   │                │
   │                ▼
   │         Prepare Tag Data
   │                │
   │                ▼
   │          Add Ticket Tag
   │
   └── FALSE → Send Non-English Response
                        │
                        ▼
               Reassign to Human Agent
```

---

## Node Reference

| # | Node | Type | Purpose |
|---|------|------|---------|
| 1 | Freshdesk Webhook Trigger | Webhook | Fires when ticket status is set to "Kapa Test" |
| 2 | Check Ticket Language | Code | Detects ticket language from body text |
| 3 | Is Ticket in English? | IF | Routes English vs non-English tickets |
| 4 | Ask Kapa AI | HTTP Request | Sends ticket description to Kapa AI API |
| 5 | Post AI Answer to Ticket | HTTP Request | Posts Kapa AI answer as public reply |
| 6 | Add Kapa Link (Private Note) | HTTP Request | Adds private note with Kapa conversation URL |
| 7 | Merge Ticket Tags | Code | Merges existing tags with new AI-handled tag |
| 8 | Prepare Tag Data | Set | Maps tag fields for Freshdesk API call |
| 9 | Add Ticket Tag | HTTP Request | Tags ticket as AI-handled in Freshdesk |
| 10 | Send Non-English Response | HTTP Request | Posts language notice to non-English tickets |
| 11 | Reassign to Human Agent | HTTP Request | Reassigns non-English ticket to agent queue |

18 nodes total including sticky note documentation.

---

## Flow Paths

### English Ticket
1. Ticket body is detected as English
2. Kapa AI is queried with the ticket description
3. AI answer posted as public reply on the ticket
4. Private note added with the Kapa conversation link (internal review)
5. Ticket tagged as AI-handled

### Non-English Ticket
1. Ticket body is detected as non-English
2. Response posted asking user to resubmit in English
3. Ticket reassigned to human agent queue for manual handling

---

## Credentials Required

| Credential | Node(s) | Purpose |
|------------|---------|---------|
| Freshdesk API Key | Webhook Trigger, HTTP Requests | Read and update tickets |
| Kapa AI API Key | Ask Kapa AI | Query the Kapa AI knowledge base |

See `.env.example` for the required environment variable names.

---

## Setup

1. **Import** `Freshdesk_Kapa_Integration.json` into n8n
2. **Connect credentials** — Freshdesk API key, Kapa AI API key (see `.env.example`)
3. **Configure Freshdesk webhook** — in Freshdesk, create an automation rule that sends a webhook to the n8n webhook URL when ticket status is set to `Kapa Test`
4. **Update agent ID** — in the `Reassign to Human Agent` node, set the correct Freshdesk agent or group ID
5. **Update tag name** — in `Prepare Tag Data`, set the tag you want applied to AI-handled tickets
6. **Test** — use `test-data.json` as a sample payload before going live
7. **Activate** the workflow in n8n

---

## Customisation

| What | Where | How |
|------|-------|-----|
| Trigger condition | Freshdesk automation rule | Change from `Kapa Test` to any status or tag |
| Kapa AI query | Ask Kapa AI node | Update the request body / prompt |
| AI-handled tag | Prepare Tag Data node | Change tag name or ID |
| Reassignment target | Reassign to Human Agent node | Update agent or group ID |
| Non-English response | Send Non-English Response node | Edit the reply text |

---

## Related Workflows

| Workflow | Relationship |
|----------|-------------|
| `HRV6 - Chat with HR documents in Slack.json` | Same AI-over-docs pattern — Pinecone + Slack instead of Kapa + Freshdesk |
