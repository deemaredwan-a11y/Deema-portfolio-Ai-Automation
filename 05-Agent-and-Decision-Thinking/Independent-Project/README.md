ContentPilot AI — Research-to-LinkedIn Content Agent
> An n8n-based AI agent that processes a planned technical topic from Google Sheets, researches it with Firecrawl, drafts an Arabic LinkedIn post, and prepares delivery to Telegram for human review.

---

## 🎯 Problem

### The Context

Creating educational LinkedIn posts about AI, automation, and software development involves repeatedly selecting a topic, investigating specific research questions, assessing sources, writing in a consistent voice, and preparing a draft for publication.

### The Problem

This process is time-consuming and repetitive when dozens of topics are planned in advance. A simple text generator can draft a post, but may skip research, overlook required questions, or present unsupported claims.

### Why It Matters

A repeatable, evidence-conscious workflow can reduce manual preparation while preserving editorial control and making content production more consistent and traceable.

---

## 💡 Solution

**ContentPilot AI** uses a daily n8n workflow and a 71-topic Google Sheets content plan. It selects the first pending topic, provides its research points and suggested hook to an AI agent, and uses a connected Firecrawl search tool to support drafting. The intended outcome is a ready-to-review Arabic LinkedIn post delivered through a dedicated Telegram bot—not automatic publication.

What does the system do?

1. Reads planned topics from Google Sheets and selects one pending topic per run.
2. Updates the selected topic's processing status and passes its title, research points, and hook to the agent.
3. Uses an OpenAI chat model and Firecrawl `/search` to research and write a draft in approachable Arabic with English technical terms.
4. Sends the draft to Telegram for human review and, in the completed design, records the delivery outcome and final status in Google Sheets.

**Current scope:** The topic-selection nodes, status-update operation, AI drafting, and Firecrawl connection have been exercised. A draft was observed arriving in Telegram, but it initially went to the *older real-estate bot* because the wrong Telegram credential was selected. The corrected ContentPilot bot connection and the final end-to-end delivery have not been independently verified in the provided evidence.

---

## 👤 Users

**Primary Users:**  
Content creators, students, freelancers, and technical professionals who publish educational LinkedIn content.

**User Need:**  
Turn a pre-planned sequence of technical subjects into accurate, readable drafts without repeatedly doing every research and drafting step by hand, while retaining control over publication.

---

## 🧠 How I Designed the Solution

I separated predictable workflow operations from decisions that benefit from AI. Scheduling, retrieving rows, filtering by status, limiting the run to a single topic, and writing processing status belong to n8n and Google Sheets. The AI agent interprets the required research points, formulates search queries, checks whether evidence is sufficient, and drafts the post. Publication remains a human decision.

The content plan contains **71 existing topics**, not newly submitted messages. Therefore, **Schedule Trigger**, rather than Telegram Trigger or a row-added trigger, is the main entry point. The workflow uses the `الحالة` (status) column to find pending topics and processes a single row (`Limit = 1`) each run. This is a *next-pending-row* strategy, so reliable sequencing requires clear completion, failure, and retry handling; it is not yet proof that every calendar day advances exactly one row.

The writing instructions specify approachable Arabic, English technical terminology, a meaningful opening, careful sourcing, and a natural conclusion **without a closing question**. The prompt was revised after a test showed the agent producing a draft without calling any tools.

---

## ⚙️ How It Works

```text
Schedule Trigger (daily; timezone must be configured)
  ↓
Google Sheets — Get Row(s) (71 planned topics)
  ↓
Filter (الحالة = "لم يبدأ")
  ↓
Limit (first item only; preserve sheet order)
  ↓
Google Sheets — Update Row (الحالة = "قيد البحث")
  ↓
AI Agent (topic + exact research points + proposed hook)
  ├── OpenAI Chat Model
  ├── Simple Memory (optional context, not source of truth)
  └── Firecrawl /search (web research)
  ↓
Check result: DRAFT_READY or NEEDS_REVIEW [planned validation]
  ├── DRAFT_READY → Telegram Send Message → verify delivery
  └── NEEDS_REVIEW → review notice / safe failure path
  ↓
Google Sheets — Update status after verified outcome [planned]
  ↓
Human reviews and publishes manually on LinkedIn
```

**Important implementation note:** The status is currently set to `قيد البحث` *before* drafting. A failed run could otherwise leave the topic stuck or cause later runs to skip it. A controlled retry/recovery path is required to preserve strict sequential processing.

---

## 🤖 Role of AI

What does AI do in this solution?

- Interprets the topic and the specific research questions supplied by the sheet.
- Generates topic-specific Firecrawl queries and assesses whether retrieved evidence supports key claims.
- Drafts an educational LinkedIn post in simple Arabic while retaining technical terms in English.
- Chooses between `DRAFT_READY` and `NEEDS_REVIEW` when evidence or instructions are insufficient.
- Reviews clarity, repetition, technical accuracy, and the required no-question ending.

### What AI Should NOT Do

- Publish posts or claim to have published them without human action.
- Invent sources, research findings, technical facts, or personal experiences.
- Claim that Firecrawl was used when no tool call occurred.
- Treat a draft as approved, or assume Telegram delivery or Google Sheets updates succeeded without verification.
- Modify or delete original research topics or exceed the connected tools' permissions.

---

## 🔄 Automation

What parts of the process are automated or designed for automation?

- Scheduled execution of the content-production workflow.
- Retrieval and status-based filtering of planned topics.
- Selection of only one pending topic per run.
- Writing an in-progress status for the selected topic.
- AI-assisted research and draft generation.
- Telegram delivery of drafts (node configured/tested with an initially incorrect bot credential; correct-bot verification pending).
- Planned final status update and separate failure/review handling.

Publication to LinkedIn is intentionally **manual**.

---

## 🗂️ Data

### Input

The Google Sheets content plan contains these columns:

| Column | Purpose |
|---|---|
| `#` | Topic sequence number |
| `المحور` | Subject area / course unit |
| `اسم الموضوع (من Word)` | Main topic from the original document |
| `عن ماذا نبحث (من Word)` | Exact research points |
| `Hook مقترح` | Suggested opening |
| `الحالة` | Processing status, initially `لم يبدأ` |

The original Excel plan was prepared from the supplied Word topic list and imported into Google Sheets.

### Output

- A draft marked `STATUS: DRAFT_READY` with the topic, LinkedIn post, and sources actually used; or a `STATUS: NEEDS_REVIEW` result explaining what needs attention.
- A Telegram message containing the draft or review notice.
- Processing status in Google Sheets, with final delivery status handling to be completed and verified.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **n8n Cloud** | Workflow orchestration and scheduled execution |
| **Schedule Trigger** | Starts the workflow on a daily schedule |
| **Google Sheets** | Stores the 71-topic plan and processing statuses |
| **Filter + Limit** | Selects one pending topic at a time |
| **AI Agent** | Research decisions, content generation, and review statuses |
| **OpenAI Chat Model** | Language understanding and drafting |
| **Firecrawl `/search`** | Web search for relevant technical sources |
| **Simple Memory** | Optional conversation context; not a durable state database |
| **Telegram Bot API** | Delivers drafts for human review |
| **GitHub / Markdown** | Project documentation and evidence |

---

## 🔑 Key Design Decisions

### Decision 1 — Scheduled, sequential topic selection

**Decision:**  
Use Schedule Trigger, retrieve existing rows, filter for `لم يبدأ`, and keep the first eligible row.

**Why:**  
The plan is already populated. The goal is to process *one topic per day in sequence*, rather than react to new row creation. Tracking statuses is essential to avoid repeating topics; failures must be handled before advancing.

### Decision 2 — Mandatory research before a finished draft

**Decision:**  
Connect Firecrawl `/search` to the agent and instruct it to use search before returning `DRAFT_READY`.

**Why:**  
An early test produced a post without using any tools. Research must be observable, and unsupported material should go to `NEEDS_REVIEW`. A system-level research gate would be stronger than prompt instructions alone and remains a recommended improvement.

### Decision 3 — Human-controlled publication

**Decision:**  
Deliver drafts to Telegram for review; do not publish automatically to LinkedIn.

**Why:**  
Human approval protects writing quality, factual accuracy, and the author's professional voice while limiting agent autonomy.

---

## 🧪 Testing

### Test 1 — Read existing topics and select only the first one

**Input:**  
The populated Google Sheet with 71 topics; read rows, filter pending items, then apply `Limit = 1`.

**Expected Result:**  
Return a single pending topic in sheet order.

**Actual Result:**  
The Google Sheets node returned **71 items**, and the Limit output showed **1 item**, `row_number = 2` and `# = 1`.

**Status:** ✅ PASS (selection behavior observed)

---

### Test 2 — Update the processing status

**Input:**  
Selected topic `# = 1`, update `الحالة` to `قيد البحث`.

**Expected Result:**  
Target topic receives the in-progress status without overwriting other content fields.

**Actual Result:**  
The update node returned `# = 1` and `الحالة = قيد البحث`. A separate independent comparison of all other sheet fields was not supplied.

**Status:** ✅ PASS (status result observed; unchanged fields not independently audited)

---

### Test 3 — Detect missing research-tool usage

**Input:**  
A topic sent to the AI Agent before the mandatory research prompt revision.

**Expected Result:**  
Use Firecrawl for research before producing a finished draft.

**Actual Result:**  
The agent produced `DRAFT_READY`, while the run displayed **“None of your tools were used in this run.”**

**Status:** ❌ FAIL — prompted a revision to the System Message

---

### Test 4 — Repeat after mandatory research instructions

**Input:**  
A subsequent topic (observed: `02 — تحسين العملية قبل الأتمتة`) after adding mandatory Firecrawl instructions.

**Expected Result:**  
A Firecrawl search call with usable source evidence, followed by an accurate post or `NEEDS_REVIEW`.

**Actual Result:**  
A `DRAFT_READY` output was shown, and the tool run was later reported as working. The actual tool log and cited source URLs were not included in the shared evidence.

**Status:** ⚠️ PARTIALLY VERIFIED — archive Logs and source evidence

---

### Test 5 — Telegram delivery and credential routing

**Input:**  
An AI-generated draft sent via Telegram Send Message.

**Expected Result:**  
Deliver the draft to the dedicated `@ContentPilotDeema_bot` chat.

**Actual Result:**  
A draft arrived in `XYZRealEstateAssistant_bot`, revealing that the **older Telegram credential** had been selected. A new credential for ContentPilot was subsequently being configured; successful final delivery to the intended bot has not been shown.

**Status:** ❌ INITIAL FAIL — correct-bot retest pending

---

### Additional required tests before completion

- Two consecutive daily executions select two successive topics, without skipping or duplicating a topic.
- Failure after `قيد البحث` returns the same topic to a safe retry/review path rather than silently advancing.
- `NEEDS_REVIEW` is routed separately and is not described as ready for publication.
- Firecrawl fails or returns insufficient evidence, and the agent does **not** claim verified research.
- Correct ContentPilot Telegram credential delivers to the intended chat and not the real-estate bot.
- The final Google Sheets status changes only after the downstream delivery outcome is verified.

These are **NOT TESTED** until supported by actual execution logs and screenshots.

---

## 📸 Evidence

### Final Result
**workflow:**
<img width="1506" height="533" alt="image" src="https://github.com/user-attachments/assets/9f795414-23f0-4a46-81f8-fce662cacf99" />

**sheets**
<img width="1482" height="573" alt="image" src="https://github.com/user-attachments/assets/fae574ab-4bf3-4b12-b9db-82f9a9f8efa0" />

**Telegram bot:**
<img width="947" height="713" alt="image" src="https://github.com/user-attachments/assets/97dc429e-a345-43bc-ad28-3c295409468f" />


---

## ⚠️ Challenges & Failures

What didn't work as expected?

- **Tool not invoked:** The AI Agent initially generated a `DRAFT_READY` post without using Firecrawl.
- **Filter misconfiguration:** All rows were initially discarded when the comparison value was not set correctly; another attempt compared the status field with itself and passed all rows.
- **Wrong Telegram bot:** A draft went to the previous real-estate bot because the Telegram credential was reused.
- **State sequencing risk:** Marking a row `قيد البحث` before content generation can cause a failed topic to be skipped on the next scheduled run.

How did I address it or plan to address it?

- Strengthened the System Message to require Firecrawl searches and to return `NEEDS_REVIEW` when evidence is insufficient; follow-up log verification is still needed.
- Corrected the filter to compare `الحالة` to the fixed string `لم يبدأ`, then added Limit = 1.
- Created a separate credential for the ContentPilot Telegram bot; final send verification remains pending.
- Planned an explicit completion and retry/error path rather than relying on the initial in-progress status alone.

---

## 📚 What I Learned

### Technical Lesson

n8n can combine a scheduler, structured spreadsheet data, an AI Agent, web research, and Telegram messaging. Sub-node tools may not be called simply because they are connected, and the correct Telegram bot is determined by the selected credential—not the Chat ID alone.

### Problem-Solving Lesson

A workflow must be tested at the **actual output and tool-action level**, not only by checking green node indicators. Status changes, one-row selection, failure recovery, and external delivery each require explicit evidence.

### AI Lesson

Agent autonomy needs boundaries. System prompts influence tool use and writing style, but independent validation and workflow-level gates are more reliable for mandatory research, source verification, and safe progression.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Enforce a workflow-level research and source-verification gate before accepting `DRAFT_READY`.
- Add durable topic state, recovery/retry logic, deduplication, and a verified delivery update so no daily topic is silently skipped.
- Enable Telegram review commands (Approve / Rewrite / Reject) and store feedback without automatically publishing to LinkedIn.
- Add draft text and source URLs to a tracking sheet or separate review store while preserving the original topic fields.
- Measure research coverage, factual accuracy, draft quality, delivery success, and time saved across repeated runs.

---

## 🔗 Project Links

[Live Demo](t.me/ContentPilotDeema_bot. )

[Workflow](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/blob/main/05-Agent-and-Decision-Thinking/Independent-Project/ContentPilot%20AI.json)

[Repository](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/edit/main/05-Agent-and-Decision-Thinking)

---

## 👩‍💻 Author

**Deema Said Radwan**

[LinkedIn profile](https://www.linkedin.com/in/deema-radwan-a06a19370/) · [GitHub profile](https://github.com/deemaredwan-a11y)
