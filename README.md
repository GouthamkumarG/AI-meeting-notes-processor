<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=200&section=header&text=AI%20Meeting%20Notes&fontSize=55&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Agentic%20Meeting%20Intelligence%20Pipeline&descAlignY=55&descSize=18&descColor=a0aec0"/>

<br/>

[![N8N](https://img.shields.io/badge/N8N-Workflow%20Automation-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io)
[![Claude AI](https://img.shields.io/badge/Claude%20Haiku-AI%20Extraction-D97706?style=for-the-badge&logo=anthropic&logoColor=white)](https://anthropic.com)
[![Supabase](https://img.shields.io/badge/Supabase-RAG%20Memory-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Notion](https://img.shields.io/badge/Notion-Action%20Items-000000?style=for-the-badge&logo=notion&logoColor=white)](https://notion.so)
[![Gmail](https://img.shields.io/badge/Gmail-Meeting%20Summary-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](https://gmail.com)

<br/>

> Post your transcript. Get structured notes. Never take manual meeting notes again.
>
> An end-to-end agentic AI pipeline that receives raw meeting transcripts,
> extracts action items with owners and due dates, stores context for future meetings,
> syncs everything to Notion, and emails a clean summary — automatically.

<br/>

![Pipeline Demo](https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&duration=2000&pause=500&color=16A34A&center=true&vCenter=true&multiline=true&repeat=true&width=650&height=100&lines=Receiving+raw+meeting+transcript+via+webhook...;Extracting+action+items+with+Claude+AI...;Syncing+tasks+to+Notion+and+delivering+summary...)

</div>

---

## What Is This?

Most meeting notes are broken.

Someone writes them half-heartedly during the call, action items get buried in a wall
of text, owners are ambiguous, due dates are missing, and by next week nobody
remembers what was agreed. The same topics come up again because nothing was tracked.

This pipeline fixes all of that automatically.

Send a raw transcript to the webhook and within seconds Claude extracts every action
item with its owner and due date, stores the transcript in Supabase so future
meetings have full context of what was previously discussed, creates individual task
cards in Notion for each action item, and fires a clean HTML summary email — all
without touching a single tool manually.

---

## How The Pipeline Works

~~~
+------------------------------------------------------------------+
|                                                                  |
|   Webhook            Supabase             Supabase               |
|   Receives   --->    Saves New   --->     Fetches Last           |
|   Transcript          Transcript           3 Transcripts         |
|                                            (RAG Context)         |
|                                                                  |
|                           |                                      |
|                           v                                      |
|                                                                  |
|   Claude              JavaScript          JavaScript             |
|   Haiku AI   <---     Prompt     <---    Formatter               |
|   Extracts            Builder             Combines context       |
|   Action Items                            and transcript         |
|                                                                  |
|                           |                                      |
|                           v                                      |
|                                                                  |
|   Notion              Gmail               If Node                |
|   Task Cards  <---    Summary    <---     Checks for             |
|   Per Item            Email               Action Items           |
|                                                                  |
+------------------------------------------------------------------+
~~~

---

## Step By Step Breakdown

### Step 1 - Webhook Trigger

A POST request to the N8N webhook endpoint delivers the raw meeting transcript
as a JSON body. Any recording tool, Zapier, or a simple curl command can trigger it.
No manual login or copy-paste required.

### Step 2 - Supabase Transcript Storage

The incoming transcript is immediately saved to a Supabase PostgreSQL table.
Every meeting is persisted so the pipeline builds a growing memory of past discussions
that can be referenced in future sessions.

### Step 3 - RAG Context Retrieval

Supabase is queried for the three most recent transcripts ordered by creation date.
These are passed alongside the new transcript into Claude so it understands prior
commitments, repeated topics, and ongoing threads — not just the current meeting in isolation.

### Step 4 - JavaScript Prompt Builder

A JavaScript node assembles a structured prompt combining the past meeting context
and the new transcript. Claude is instructed to return only valid JSON with no
preamble or markdown — clean structured output every time.

### Step 5 - Claude Haiku AI Extraction

The core of the pipeline. Claude receives the full prompt and returns a structured
JSON object with two fields:

- action_items is an array where each item has a task description, an owner name, and a due date
- summary is a concise paragraph capturing the key decisions and outcomes of the meeting

Claude uses the past transcript context to resolve ambiguous ownership references
and carry forward any unresolved items from prior meetings.

### Step 6 - JavaScript Parser

Parses and cleans the raw Claude response, stripping any accidental markdown fences,
and passes the structured object downstream. An If node checks whether any action
items were extracted before triggering the Notion and Gmail branches.

### Step 7 - Notion Task Creation

Each action item is split out individually and written as a separate database page
in Notion with the task title, owner, and due date populated as properties. Every
meeting produces a clean set of trackable cards without any manual entry.

### Step 8 - Gmail Summary Email

A formatted HTML email is sent automatically with the meeting summary paragraph
followed by a structured list of all action items showing task, owner, and due date.
Delivered to your inbox within seconds of the transcript arriving.

---

## Key Features

| Feature | What It Does |
|---|---|
| RAG Memory | Uses last 3 meeting transcripts as context for smarter extraction |
| Action Item Extraction | Pulls every task with owner and due date from raw transcript |
| Notion Sync | Creates individual database cards for each action item automatically |
| Gmail Summary | Sends a clean HTML summary email immediately after processing |
| Supabase Storage | Persists every transcript for growing meeting memory over time |
| Conditional Branching | Only triggers Notion and email if action items are actually found |
| Zero Manual Input | Transcript in — structured notes out, end to end |
| JSON Output | Claude returns clean structured JSON, no parsing guesswork |
| Contextual Intelligence | Past meeting context prevents duplicate tasks and resolves owner ambiguity |
| Webhook Trigger | Works with any tool that can send a POST request |

---

## Tech Stack

~~~javascript
const pipeline = {
  orchestration : "N8N Cloud",
  ai            : "Claude Haiku API (Anthropic)",
  memory        : "Supabase PostgreSQL (RAG)",
  taskTracking  : "Notion API (OAuth2)",
  delivery      : "Gmail via N8N",
  trigger       : "N8N Webhook",
  formatting    : "JavaScript (Node.js)",
}
~~~

---

## Repository Structure

~~~
ai-meeting-notes-processor/
├── workflow/
│   └── workflow.json           <- N8N workflow export
├── assets/
│   └── pipeline.png            <- N8N pipeline screenshot
└── README.md
~~~

---

## How To Use This

1. Import workflow.json into your N8N instance
2. Connect your Anthropic, Supabase, Notion, and Gmail credentials
3. Create a transcripts table in Supabase with a transcript text column and created_at timestamp
4. Create a Notion database with Task, Owner, and Due Date properties
5. Update the webhook path and your email address in the relevant nodes
6. Activate the workflow and send a POST request with your transcript to start

---

## Built By

Goutham Gorthi — AI Engineer and Full Stack Developer

Open to AI Engineer, AI Automation, and Full Stack Developer roles
across the UK. Available immediately.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/goutham-chowdary-679744280/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/GouthamkumarG)

---

<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:16213e,100:1a1a2e&height=100&section=footer"/>
</div>
