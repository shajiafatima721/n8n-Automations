# Lead Capture Form → Google Sheets → Email → Telegram (n8n Automation)

A lead-generation form that captures visitor submissions and automatically
saves them to Google Sheets, sends the visitor a confirmation email, and
notifies a team via Telegram — all powered by a single n8n workflow.

## Overview

A simple HTML contact/lead form posts directly to an n8n webhook. From
there, the workflow handles everything: logging the lead, replying to the
visitor, and alerting the team in real time.

## How It Works

```
Lead Form (HTML) → n8n Webhook → Google Sheets → Confirmation Email → Telegram Notification
```

1. **Lead Form Webhook** — receives the form submission (`name`, `email`,
   `message`) via a `POST` request to the `/lead` webhook path.
2. **Append to Google Sheets** — logs the lead (name, email, message,
   submission timestamp) as a new row in a connected Google Sheet.
3. **Send Confirmation Email** — emails the visitor a thank-you message
   confirming their submission was received.
4. **Notify in Telegram** — sends a formatted alert with the lead's
   details to a Telegram chat, so the team is notified instantly.

## Project Files

```
├── index.html                            # Frontend lead capture form
└── lead-to-sheet-email-telegram.json     # n8n workflow (import into n8n)
```

## Frontend (`index.html`)

A clean, single-page lead form built with Bootstrap 5, containing:

- **Name**, **Email**, and **Message** fields (all required)
- Submits via `fetch()` (no page reload) directly to the n8n webhook URL
- Shows inline success (`✅ Thanks! Your message was sent.`) or error
  feedback if the webhook can't be reached

### Setup

Replace `WEBHOOK_URL` in the form's `action` attribute with your live
n8n webhook URL, e.g.:

```html
<form id="lead-form" action="https://your-n8n-instance.com/webhook/lead" method="post">
```

## n8n Workflow Nodes

| Node | Type | Purpose |
|---|---|---|
| Lead Form Webhook | `n8n-nodes-base.webhook` | Receives POST submissions at `/lead` |
| Append to Google Sheets | `n8n-nodes-base.googleSheets` | Logs Name, Email, Message, SubmittedAt |
| Send Confirmation Email | `n8n-nodes-base.emailSend` | Sends a thank-you email to the visitor |
| Notify in Telegram | `n8n-nodes-base.telegram` | Posts a new-lead alert to a Telegram chat |

## Prerequisites

- An [n8n](https://n8n.io/) instance (self-hosted or n8n Cloud)
- A Google Sheet + Google Sheets OAuth2 credentials configured in n8n
- An SMTP account for sending the confirmation email
- A Telegram bot + chat ID for notifications

## Setup

1. **Import the workflow**: n8n → *Workflows* → *Import from File* →
   select `lead-to-sheet-email-telegram.json`.
2. **Configure credentials** on each node:
   - *Append to Google Sheets* — connect your Google account, then
     replace `PASTE_YOUR_GOOGLE_SHEET_ID` with your sheet's ID and
     confirm the sheet/tab name (`Sheet1` by default).
   - *Send Confirmation Email* — connect an SMTP account, and update
     `fromEmail` to your sending address.
   - *Notify in Telegram* — connect your Telegram bot credentials and
     replace `PASTE_YOUR_TELEGRAM_CHAT_ID` with your chat ID.
3. **Activate the workflow** in n8n so the webhook goes live.
4. **Copy the webhook's production URL** from the *Lead Form Webhook*
   node and paste it into `index.html`'s form `action` attribute.
5. Host `index.html` anywhere (GitHub Pages, Netlify, or any static
   host) and test a submission end-to-end.

## Google Sheet Columns

| Column | Description |
|---|---|
| `Name` | Visitor's name |
| `Email` | Visitor's email address |
| `Message` | Visitor's message |
| `SubmittedAt` | ISO timestamp of submission |

## Customization Ideas

- Add spam protection (e.g. a honeypot field or CAPTCHA) before the
  webhook node.
- Add a "source" hidden field to track which page/campaign the lead
  came from.
- Route high-priority leads (based on message keywords) to a different
  Telegram chat or add a Slack notification branch.
- Add an "IF" node to validate email format before saving/notifying.

## Tech Stack

| Tool | Purpose |
|---|---|
| HTML + Bootstrap 5 | Lead capture form UI |
| n8n | Workflow automation (webhook → sheets → email → Telegram) |
| Google Sheets API (OAuth2) | Lead storage |
| SMTP | Confirmation email delivery |
| Telegram Bot API | Real-time team notification |