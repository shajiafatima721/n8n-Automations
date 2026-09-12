# n8n Lead Automation ⚡

A self-hosted **n8n** workflow: a web form submits a lead → the lead is appended
to **Google Sheets** → a confirmation **email** is sent to the customer → a
notification is pushed to a **Telegram** chat. One workflow, four nodes, zero code.

> **Demo:** _record a 60s screen capture after importing (see below)_ ·
> **Source:** https://github.com/yusizer/n8n-lead-automation

## What the workflow does

```
[ Web form ] → POST /webhook/lead
   → [ Webhook ] → [ Append to Google Sheets ]
                  → [ Send confirmation email ]
                  → [ Notify Telegram chat ]
```

A visitor fills in the form → within seconds: a new row in your spreadsheet, an
auto-reply in their inbox, and a ping in your Telegram so you never miss a lead.

## What's in this repo

```
n8n-lead-automation/
├── workflows/
│   └── lead-to-sheet-email-telegram.json   # importable n8n workflow (4 nodes)
├── form/
│   └── index.html                          # demo HTML form that posts to the webhook
├── docker-compose.yml                      # self-host n8n in one command
├── .env.example                            # n8n config template
└── README.md
```

## Run n8n locally

```bash
cp .env.example .env          # Windows: copy .env.example .env
# edit .env: set a strong N8N_BASIC_AUTH_PASSWORD and a random N8N_ENCRYPTION_KEY

docker compose up -d          # starts n8n on http://localhost:5678
```

Open http://localhost:5678, log in with the credentials from `.env`.

## Import the workflow

1. In n8n: **Workflows → Import from File** → choose
   `workflows/lead-to-sheet-email-telegram.json`.
2. Open each node and connect its credentials (created once in n8n under
   **Credentials → New**):
   - **Google Sheets** — OAuth2 (sign in with the Google account that owns the sheet).
   - **Send Email** — SMTP (e.g. Gmail SMTP, Brevo, Mailgun).
   - **Telegram** — create a bot via [@BotFather](https://t.me/botfather), paste the token.
3. In the **Append to Google Sheets** node, replace `PASTE_YOUR_GOOGLE_SHEET_ID`
   with your spreadsheet ID (the long string in the sheet URL).
4. In the **Notify in Telegram** node, replace `PASTE_YOUR_TELEGRAM_CHAT_ID`
   (get it from [@userinfobot](https://t.me/userinfobot)).
5. **Activate** the workflow (top-right toggle). Copy the **Webhook URL** n8n shows
   on the Webhook node (e.g. `http://localhost:5678/webhook/lead`).

## Wire up the demo form

Edit `form/index.html` — replace `WEBHOOK_URL` in the `<form action="...">` with
your webhook URL. Open the form in a browser, submit it, and watch:
- a new row appear in your Google Sheet,
- a confirmation email arrive in the inbox,
- a message land in your Telegram chat.

## Deploy n8n (always-on)

The included `docker-compose.yml` runs n8n anywhere with Docker. For a permanent
public webhook URL:

- **Railway:** create a project from this repo (or a Docker image of n8n) and set
  the variables from `.env.example`, with `WEBHOOK_URL` = your Railway domain.
- **VPS (Hetzner / Oracle always-free):** `docker compose up -d` behind a
  Caddy/Nginx reverse proxy with HTTPS; set `WEBHOOK_URL` to your domain.

Once deployed, set the form's `WEBHOOK_URL` to the public webhook and the demo
works for anyone, anywhere.

## Record the demo (for the portfolio)

1. Show the form submission.
2. Cut to the Google Sheet — a new row appears.
3. Cut to the inbox — confirmation email.
4. Cut to Telegram — the notification.
5. Upload to YouTube/Imgur and link it from the portfolio card.

## Notes

- This is a **no-code** project — the "source" is the workflow JSON + self-host
  config + demo form, not application code (hence no unit tests).
- The workflow JSON contains credential **placeholders** (`PASTE_CRED_ID`,
  `PASTE_YOUR_GOOGLE_SHEET_ID`, …) — real credentials live in n8n's encrypted
  store, never in this repo.
- n8n is MIT-licensed and free to self-host; the official cloud has a free tier too.

## Screenshots

| Workflow graph | Form → Sheet → Telegram |
|:---:|:---:|
| ![workflow](docs/workflow.png) | ![demo](docs/demo.png) |

_Drop screenshots in `docs/` after importing._
