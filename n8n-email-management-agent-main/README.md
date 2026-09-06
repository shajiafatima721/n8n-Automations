# Email Management Automation (n8n Workflow)

An automated email management workflow built with **n8n** that monitors a
Gmail inbox, classifies and summarizes incoming emails using AI, and logs
the results to a Google Sheet — with no manual effort required.

## Overview

This workflow polls Gmail every 5 minutes for new emails, runs each one
through a text classifier and an AI summarizer, then appends the results
(subject, summary, snippet, date) to a Google Sheet for easy tracking and
review.

## How It Works

1. **Gmail Trigger** — polls the connected Gmail inbox every 5 minutes
   (`*/5 * * * *`) for new emails.
2. **Text Classifier** — passes the email's subject and snippet through a
   text classification step to categorize the message.
3. **OpenAI Summary** — sends the subject and snippet to `gpt-4o-mini`
   with a "friendly summarizer" prompt to generate a concise summary.
4. **Save to Google Sheets** — appends (or updates) a row in a Google
   Sheet with the email's ID, date, subject, AI-generated summary, and
   original snippet.

## Workflow Diagram

```
Gmail Trigger → Text Classifier → OpenAI Summary → Save to Google Sheets
```

## Nodes Used

| Node | Type | Purpose |
|---|---|---|
| Gmail Trigger | `n8n-nodes-base.gmailTrigger` | Watches inbox for new emails every 5 minutes |
| Text Classifier | `@n8n/n8n-nodes-langchain.textClassifier` | Classifies email content |
| OpenAI Summary | `@n8n/n8n-nodes-langchain.openAi` | Summarizes the email using `gpt-4o-mini` |
| Save to Google Sheets | `n8n-nodes-base.googleSheets` | Logs results into a Google Sheet |

## Prerequisites

- An [n8n](https://n8n.io/) instance (self-hosted or n8n Cloud)
- A Gmail account with OAuth2 access configured in n8n
- An OpenAI API key (for the summarization step)
- A Google Sheet to log results into, with Google Sheets OAuth2 access
  configured in n8n

## Setup

1. Import `Email_Management.json` into your n8n instance:
   **Workflows → Import from File**.
2. Set up credentials for each connected service:
   - **Gmail** — connect your Gmail account (OAuth2) on the *Gmail
     Trigger* node.
   - **OpenAI** — add your OpenAI API key on the *OpenAI Summary* node.
   - **Google Sheets** — connect your Google account and select the
     target spreadsheet and sheet name on the *Save to Google Sheets*
     node.
3. Replace the placeholder values in the workflow:
   - `<your-credential-id>` — auto-filled once credentials are connected
   - `<your-google-sheet-id>` — your target Google Sheet's ID
   - `<your-sheet-name>` — the tab/sheet name to write to
4. Activate the workflow.

## Google Sheet Columns

The workflow expects (or creates) the following columns in your sheet:

| Column | Description |
|---|---|
| `ID` | Gmail message ID |
| `Date` | Timestamp the row was written |
| `Subject` | Email subject line |
| `Summary` | AI-generated summary of the email |
| `Snippet` | Original email snippet/preview text |

## Customization Ideas

- Change the polling interval by editing the cron expression on the
  *Gmail Trigger* node.
- Swap `gpt-4o-mini` for a different OpenAI model on the *OpenAI Summary*
  node.
- Add a filter step to only process emails from specific senders or
  labels.
- Add a Slack/Telegram notification step after summarization for urgent
  emails.

## Tech Stack

| Tool | Purpose |
|---|---|
| n8n | Workflow automation platform |
| Gmail API (OAuth2) | Email trigger/source |
| OpenAI (`gpt-4o-mini`) | Email summarization |
| Google Sheets API (OAuth2) | Result logging/storage |