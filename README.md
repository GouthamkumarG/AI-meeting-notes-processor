<div align="center">
<img src="https://capsule-render.vercel.app/api?type=venom&color=0:0d1117,30:0a0a2e,60:0d1b4b,100:0a192f&height=280&section=header&text=AI%20Meeting%20Notes%20Processor&fontSize=45&fontColor=00d4ff&animation=fadeIn&fontAlignY=38&desc=End-to-End%20Agentic%20Pipeline%20%E2%80%A2%20RAG%20%E2%80%A2%20Multi-Tool%20Orchestration%20%E2%80%A2%20Zero%20Manual%20Intervention&descAlignY=60&descSize=15&descColor=93c5fd" width="100%"/>
<br/>
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
<br/>
Show Image
Show Image
</div>
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> What This Does
<table>
<tr>
<td width="60%" valign="top">
Drop in a raw, unstructured meeting transcript. The pipeline does the rest.
Claude reads the transcript, retrieves context from past meetings via RAG, extracts every action item, decision, and owner — then routes them to Notion and dispatches a formatted email summary. Fully automated. Zero manual intervention.
This is not a simple API call. This is a production-grade agentic system — with retrieval, reasoning, error handling, structured output parsing, and multi-tool action execution chained together.
</td>
<td width="40%" valign="top">
Pipeline highlights:
✅  RAG context from past meetings
✅  LLM-powered extraction (Claude)
✅  Structured JSON output parsing
✅  IF node error handling
✅  Auto Notion task creation
✅  Gmail summary dispatch
✅  Webhook + Schedule dual trigger
✅  Extensible to Slack, Jira, Teams
</td>
</tr>
</table>
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Pipeline Architecture
┌─────────────────────────────────────────────────────────────────┐
│                    TRIGGER LAYER                                 │
│         Webhook (real-time)  ◆  Schedule Trigger (9am daily)   │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    RAG INGESTION                                 │
│           HTTP Request1 → Store transcript in Supabase          │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    RAG RETRIEVAL                                 │
│        HTTP Request2 → Fetch last 3 transcripts from Supabase   │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    PROMPT ENGINEERING                            │
│     Code Node → Build contextual prompt with past + new data    │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    LLM REASONING                                 │
│         Claude Haiku → Extract action items + summary           │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    OUTPUT VALIDATION                             │
│         Code Node → Parse JSON  ◆  IF Node → Error check        │
└──────────────┬──────────────────────────────┬───────────────────┘
               │                              │
               ▼                              ▼
┌──────────────────────────┐    ┌─────────────────────────────────┐
│      NOTION TASKS        │    │        GMAIL SUMMARY            │
│  Split Out → Create one  │    │  Send formatted HTML summary    │
│  row per action item     │    │  with all action items          │
└──────────────────────────┘    └─────────────────────────────────┘
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Pipeline Screenshot
Show Image
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Key Features
<div align="center">
FeatureWhat It Does🧠 RAG ArchitectureStores every transcript in Supabase. Retrieves last 3 before each run. Claude gets historical context — not just the current meeting🤖 LLM ExtractionClaude Haiku extracts tasks, owners, and deadlines from unstructured natural language with high reliability🛡️ Error HandlingIF node validates Claude's JSON output. Malformed responses stop the pipeline gracefully📋 Notion IntegrationOne database row per action item, auto-mapped to Name, Owner, and Due Date columns📧 Email DispatchFormatted HTML summary sent once per meeting run⚡ Dual TriggerWebhook for real-time ingestion. Schedule trigger for daily automated runs🔗 ExtensibleCore pipeline logic unchanged when adding Slack, Jira, Teams, or any other tool🎯 Prompt EngineeringContext-injected prompt built dynamically — past transcripts + new transcript combined before Claude call
</div>
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Live Outputs
<table>
<tr>
<td width="50%">
Notion — Auto-Created Tasks
Show Image
</td>
<td width="50%">
Gmail — Summary Email
Show Image
</td>
</tr>
<tr>
<td width="50%">
Supabase — RAG Storage
Show Image
</td>
<td width="50%">
Webhook — Test Trigger
Show Image
</td>
</tr>
</table>
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Tech Stack
<div align="center">
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
</div>
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Setup Guide
Prerequisites

N8N account (cloud or self-hosted)
Anthropic API key → console.anthropic.com
Notion account + integration token → notion.so/profile/integrations
Supabase project → supabase.com
Gmail account with OAuth connected in N8N

Step 1 — Supabase
Create a table named transcripts with columns: id (int8), created_at (timestamptz), transcript (text). Disable Row Level Security.
Step 2 — Notion
Create a database named Meeting Action Items with columns: Name (Title), Owner (Text), Due Date (Text). Connect your N8N integration via ... → Connections.
Step 3 — Import Workflow

Download workflow.json
In N8N → New Workflow → ... → Import from file
Replace all placeholders:

PlaceholderReplace WithYOUR_ANTHROPIC_API_KEYYour Anthropic API keyYOUR_SUPABASE_URLYour Supabase project URLYOUR_SUPABASE_ANON_KEYYour Supabase anon public keyYOUR_NOTION_DATABASE_IDYour Notion database ID from URLYOUR_WEBHOOK_PATHAny unique path stringYOUR_EMAIL@gmail.comYour Gmail address
Step 4 — Test
bashcurl -X POST https://your-n8n-instance/webhook/YOUR_WEBHOOK_PATH \
  -H "Content-Type: application/json" \
  -d '{"transcript": "Alex will fix the login bug before Thursday demo. Priya will update the pitch deck with new pricing by tomorrow. We decided to postpone the launch to next Monday."}'
Check Notion for new tasks and Gmail for the summary email.
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Prompt Engineering
javascriptconst pastTranscripts = $('HTTP Request2').all()
  .map(t => t.json.transcript).join(' | ');

const newTranscript = $('Webhook').first().json.body.transcript;

const prompt = 'Past meetings context: ' + pastTranscripts +
  '. Extract action items from this new transcript. ' +
  'Return ONLY valid JSON: ' +
  '{"action_items": [{"task": "", "owner": "", "due_date": ""}], "summary": ""}. ' +
  'New transcript: ' + newTranscript;

const body = {
  model: "claude-haiku-4-5-20251001",
  max_tokens: 1024,
  messages: [{ role: "user", content: prompt }]
};

return [{ json: { body: JSON.stringify(body) } }];
Past transcripts retrieved from Supabase are prepended as context — enabling Claude to identify follow-up tasks, recurring owners, and cross-meeting continuity.
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%">
> Author
<div align="center">
Goutham Chowdary
AI-Powered Developer · Full Stack Engineer · Automation Architect
<br/>
Show Image
Show Image
Show Image
</div>
<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:0a0a2e,100:0d1b4b&height=120&section=footer&text=Star+this+repo+if+it+helped+you+%E2%AD%90&fontSize=15&fontColor=00d4ff&animation=fadeIn&fontAlignY=65" width="100%"/>
</div>
