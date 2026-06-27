# Telegram Study Helper — AI Automation with n8n

A Telegram chatbot that acts as a smart study assistant. Powered by **GPT-4** and automated using **n8n**, it can answer study questions, analyze images (diagrams, notes, textbook pages), remember your conversation history, and search Wikipedia for factual information.

---

## Features

- **Text Q&A** — Ask any study question and get a helpful, concise answer
- **Image Analysis** — Send a photo of your notes, diagrams, or textbook pages and the bot will analyze them
- **Conversation Memory** — The bot remembers the last 10 messages per user for context-aware replies
- **Wikipedia Search** — Automatically searches Wikipedia when factual information is needed
- **Per-User Sessions** — Each user gets their own independent memory session

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation (no-code/low-code) |
| Telegram Bot API | Chat interface |
| OpenAI GPT-4.1 Mini | AI responses and image analysis |
| Wikipedia Tool (n8n) | Factual lookups |

---

## Project Structure

```
telegram-study-helper-n8n/
├── workflow.json       # n8n workflow — import this into your n8n instance
├── .env.example        # Template showing which credentials are needed
├── .gitignore          # Prevents sensitive files from being committed
└── README.md           # This file
```

---

## How to Set Up

### Step 1 — Create a Telegram Bot
1. Open Telegram and message [@BotFather](https://t.me/BotFather)
2. Send `/newbot` and follow the instructions
3. Copy the **Bot Token** you receive

### Step 2 — Get an OpenAI API Key
1. Go to [platform.openai.com](https://platform.openai.com)
2. Create an account and generate an API key
3. Copy the key

### Step 3 — Set Up n8n
1. Install n8n locally or use [n8n Cloud](https://n8n.io/cloud)
   - Local: `npm install -g n8n` then run `n8n`
   - Cloud: Sign up at n8n.io and create a workspace

### Step 4 — Import the Workflow
1. Open your n8n dashboard
2. Click **"Add workflow"** → **"Import from file"**
3. Select the `workflow.json` file from this repo
4. The workflow will open in the editor

### Step 5 — Add Your Credentials in n8n
After importing, you need to connect your own credentials:

**Telegram credential:**
- Click any Telegram node → click the credential dropdown → **"Create new"**
- Enter your Bot Token from Step 1

**OpenAI credential:**
- Click the **GPT-4 Model** node → click the credential dropdown → **"Create new"**
- Enter your OpenAI API Key from Step 2

### Step 6 — Activate the Workflow
1. Click the **"Active"** toggle in the top right of n8n
2. Open your Telegram bot and send a message — it should reply!

---

## Environment Variables

This project uses credentials stored inside n8n (not in a `.env` file). However, if you are self-hosting n8n, you may need these in a `.env` file:

```
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
OPENAI_API_KEY=your_openai_api_key_here
N8N_WEBHOOK_URL=your_n8n_webhook_url_here
```

See `.env.example` for the full template.

---

## How the Workflow Works

```
User sends message on Telegram
        ↓
  Has Photo?
   ↙        ↘
Yes            No
 ↓              ↓
Download     Extract
Photo        Message
  ↓              ↓
AI Agent     AI Agent
(Photo)      (Text)
  ↓              ↓
Send Reply   Send Reply
```

- **Text messages** go through the text agent with per-user memory
- **Photo messages** are downloaded first, then analyzed by the photo agent
- Both agents use the same GPT-4 model and Wikipedia tool

---

## Notes

- The `workflow.json` in this repo has all sensitive credential IDs removed and replaced with placeholders — you must add your own credentials after importing
- Never commit your real `.env` file or expose your bot token publicly
- Memory is stored temporarily in n8n's buffer — it resets if n8n restarts (for permanent memory, you'd need a database node)

---

## Author

Made by [VandanLearns](https://github.com/VandanLearns)
