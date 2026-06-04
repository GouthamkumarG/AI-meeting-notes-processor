---

## `> Pipeline Screenshot`

![N8N Workflow](screenshots/workflow.png)

---

## `> Live Demo`

**Input — raw transcript via webhook:**
```json
{
  "transcript": "Alex will fix the login bug before the demo on Thursday. Priya will update the pitch deck with new pricing by tomorrow. Marcus is blocked on the API integration. We decided to postpone the launch to next Monday."
}
```

**Output — Notion tasks auto-created:**

![Notion Output](screenshots/notion.png)

**Output — Gmail summary dispatched:**

![Gmail Output](screenshots/gmail.png)

**RAG storage — Supabase transcripts table:**

![Supabase RAG](screenshots/supabase.png)

---

## `> Key Features`

| Feature | Details |
|---------|---------|
| 🤖 **LLM Extraction** | Claude Haiku extracts tasks, owners, deadlines from unstructured text |
| 🧠 **RAG Context** | Retrieves last 3 transcripts from Supabase before each run |
| ✅ **Error Handling** | IF node validates output — stops pipeline on malformed JSON |
| 📋 **Notion Integration** | Auto-creates one database row per action item |
| 📧 **Email Dispatch** | Sends formatted HTML summary to attendees |
| ⚡ **Dual Trigger** | Webhook (real-time) + Schedule (daily 9am) |
| 🔗 **Extensible** | Add Slack, Jira, or any tool without re-engineering core logic |

---

## `> Tech Stack`

![N8N](https://img.shields.io/badge/N8N-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Anthropic Claude](https://img.shields.io/badge/Claude_Haiku-D97757?style=flat-square&logo=anthropic&logoColor=white)
![Notion API](https://img.shields.io/badge/Notion_API-000000?style=flat-square&logo=notion&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=black)
![Gmail API](https://img.shields.io/badge/Gmail_API-D14836?style=flat-square&logo=gmail&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Webhook](https://img.shields.io/badge/Webhooks-FF6C37?style=flat-square&logo=postman&logoColor=white)

---

## `> Setup Guide`

### Prerequisites
- N8N account (cloud or self-hosted)
- Anthropic API key
- Notion account + integration token
- Supabase project
- Gmail account

### Step 1 — Supabase
Create a `transcripts` table with columns: `id`, `created_at`, `transcript`

### Step 2 — Notion
Create a database with columns: `Name` (title), `Owner` (text), `Due Date` (text)
Connect your N8N integration via database settings → Connections

### Step 3 — Import Workflow
1. Download `workflow.json`
2. In N8N → New Workflow → Import from file
3. Replace all placeholders:

| Placeholder | Replace With |
|-------------|-------------|
| `YOUR_ANTHROPIC_API_KEY` | Your Anthropic API key |
| `YOUR_SUPABASE_URL` | Your Supabase project URL |
| `YOUR_SUPABASE_ANON_KEY` | Your Supabase anon key |
| `YOUR_NOTION_DATABASE_ID` | Your Notion database ID |
| `YOUR_WEBHOOK_PATH` | Any unique string |
| `YOUR_EMAIL@gmail.com` | Your Gmail address |

### Step 4 — Test
Send a POST request to your webhook URL:
```bash
curl -X POST https://your-n8n-url/webhook/YOUR_WEBHOOK_PATH \
  -H "Content-Type: application/json" \
  -d '{"transcript": "John will finish the landing page by Friday. Sarah will send the proposal by Monday."}'
```

Check Notion and Gmail for output.

---

## `> Prompt Engineering`

The extraction prompt is built dynamically in the Code node:

```javascript
const prompt = 'Past meetings context: ' + pastTranscripts + 
  '. Extract action items from this new transcript. ' +
  'Return ONLY valid JSON: {"action_items": [{"task": "", "owner": "", "due_date": ""}], "summary": ""}. ' +
  'New transcript: ' + newTranscript;
```

RAG context is injected before the new transcript so Claude can identify follow-up tasks, recurring owners, and continuity across meetings.

---

## `> Author`

<div align="center">

**Goutham Chowdary**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/goutham-chowdary-679744280/)
[![Email](https://img.shields.io/badge/Email-Hire_Me-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:gouthamchowdary00@gmail.com)

</div>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a1a2e,100:16213e&height=100&section=footer&text=Star+this+repo+if+it+helped+you&fontSize=14&fontColor=00d4ff&animation=fadeIn&fontAlignY=65" width="100%"/>

</div>
