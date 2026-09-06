# n8n Automations

A collection of workflow automation projects built with **n8n**, covering
email management and lead capture — each connecting multiple services
(Gmail, OpenAI, Google Sheets, SMTP, Telegram) to remove manual, repetitive
work.

## Projects

### 1. [Email Management](./Email-Management)

Monitors a Gmail inbox every 5 minutes, classifies and summarizes new
emails using AI, and logs the results to a Google Sheet.

- **Flow:** Gmail Trigger → Text Classifier → OpenAI Summary → Save to Google Sheets
- **Tools:** n8n, Gmail API, OpenAI (`gpt-4o-mini`), Google Sheets API
- **Files:** `Email_Management.json`

### 2. [Lead Capture Form Automation](./Lead-Capture-Automation)

A frontend lead form that posts to an n8n webhook, which logs the lead
to Google Sheets, sends the visitor a confirmation email, and notifies
the team on Telegram — in real time.

- **Flow:** Lead Form (HTML) → Webhook → Google Sheets → Confirmation Email → Telegram
- **Tools:** n8n, HTML/Bootstrap, Google Sheets API, SMTP, Telegram Bot API
- **Files:** `index.html`, `lead-to-sheet-email-telegram.json`

## Folder Structure

```
n8n Automations/
├── README.md                              # This file
├── Email-Management/
│   ├── README.md
│   └── Email_Management.json
└── Lead-Capture-Automation/
    ├── README.md
    ├── index.html
    └── lead-to-sheet-email-telegram.json
```

## Tools & Technologies Used Across Projects

| Tool | Purpose |
|---|---|
| n8n | Workflow automation platform (self-hosted or cloud) |
| Gmail API | Email trigger/source |
| OpenAI API | AI text classification and summarization |
| Google Sheets API | Data logging/storage |
| SMTP | Transactional email sending |
| Telegram Bot API | Real-time team notifications |

## How to Use

Each project has its own README with detailed setup instructions. In
general, for any workflow in this folder:

1. Open the project folder you're interested in.
2. Import the `.json` workflow file into your n8n instance
   (**Workflows → Import from File**).
3. Connect the required credentials for each node (Gmail, OpenAI, Google
   Sheets, SMTP, Telegram — as applicable).
4. Replace placeholder values (credential IDs, sheet IDs, chat IDs) with
   your own.
5. Activate the workflow.

## Prerequisites

- An [n8n](https://n8n.io/) instance (self-hosted or [n8n Cloud](https://n8n.io/cloud/))
- Accounts/API access for the services each workflow connects to (see
  each project's README for specifics)
