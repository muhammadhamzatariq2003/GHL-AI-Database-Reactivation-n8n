# AI Database Reactivation & Booking Engine (n8n + GoHighLevel + Gemini)

Turn old, unresponsive leads into booked calls automatically.

This workflow reads a lead's reply, uses AI to understand their intent, and takes the right next step: send a booking link, keep the conversation warm, or stop outreach. All without manual follow-up.

> **Demo note:** This repository demonstrates the full logic using simulated GoHighLevel webhook payloads. The CRM actions (pipeline move, tagging, email send) are produced as structured output, ready to be swapped for live GoHighLevel API calls on a client account.

## The Problem

Agencies and B2B teams sit on thousands of old leads that never responded. Re-engaging them by hand is slow, and most replies get lost or answered late.

## How It Works

1. **Reply arrives**: a webhook receives the lead's name, email, and message (from GoHighLevel).
2. **AI classification**: Google Gemini labels the reply as `Interested`, `Question`, or `Not Interested`.
3. **Smart routing**: a Switch node sends each lead down the right path:

| Intent | Action | Pipeline stage | Tag |
|---|---|---|---|
| Interested | Send booking link email | Appointment Scheduled | Interested |
| Question | Send info reply with booking option | Contacted | Needs-Answer |
| Not Interested | Stop sequence, no message | Not Interested | Do-Not-Contact |

4. **Structured result**: the workflow returns the action, stage, tag, and drafted email, ready to push into the CRM.

## Tech Stack

- **n8n**: workflow automation
- **Google Gemini API**: intent classification
- **GoHighLevel**: CRM, calendar, and pipeline (simulated in this demo)

## Quick Start

1. Import `ghl-reactivation-final-demo.json` into n8n (**Workflows → Import from file**).
2. Add your Gemini API key as a credential on the **Classify Intent (Gemini)** node.
3. Set your booking link in the **Get Data** node (`booking_link`).
4. Open the Webhook node, click **Listen for test event**, and send a test:

```bash
curl -X POST "YOUR_WEBHOOK_TEST_URL" \
  -H "Content-Type: application/json" \
  -d '{"name":"Ali","email":"ali@example.com","message":"Yes, I am interested. When can we talk?"}'
```

Try these messages to see each path:

- `Yes, I am interested. When can we talk?` → Interested
- `Can you share pricing first?` → Question
- `Please remove me from your list` → Not Interested

## Screenshots

Add your n8n workflow screenshot here:

`![Workflow](./screenshots/workflow.png)`

## Going Live With GoHighLevel

To move from demo to production, replace the three result nodes with GoHighLevel API calls:

- Send email: Conversations API
- Move pipeline stage: Opportunities API
- Add tag: Contacts API

## Want This Built for Your Business?

I build AI agents and automation workflows (n8n, Make, GoHighLevel, voice AI, RAG) for founders and agencies. Reach out on Upwork.
