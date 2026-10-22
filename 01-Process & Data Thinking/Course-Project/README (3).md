# AI Client Intake & Request Routing System

> An AI-powered automation that reads free-text client requests from a Google Form, extracts the key details, checks if the request is complete, and routes it to the web team or to human review.

---

## 🎯 Problem

### The Context

A services company (web design, digital marketing, app development) receives requests from potential clients through an online form. Every client writes in their own way: some explain everything in detail, some send a short message, and some forget important information like the budget or the deadline.

### The Problem

Each request is handled manually. An employee has to read it, understand what the client needs, organize the information, check that nothing is missing, and then decide the next step. As the number of requests grows, this repetitive manual work takes a lot of time, and some requests get delayed or important details get lost.

### Why It Matters

This is not only a "receiving messages" problem. It is a problem of **how client requests are processed, organized, and routed**. Solving it means faster follow-up, fewer lost details, and employees spending their time on real decisions instead of repetitive reading.

---

## 💡 Solution

An automation workflow built with Zapier that turns an unstructured client message into structured data, applies clear rules to decide if the request is ready, and sends it to the right place, while keeping a human in the loop for unclear or incomplete requests.

What does the system do?

1. Receives a new client request from a Google Form (name, email, company, free-text request).
2. Uses AI to extract `service_type`, `timeline`, `budget`, a short summary, and a list of missing information.
3. Decides the request status: `VALID` (all required info is present) or `NEEDS_REVIEW` (something is missing or vague).
4. Routes the request: `VALID` → web team, `NEEDS_REVIEW` → human review; saves a record in Google Sheets and sends an email notification to the right person.

---

## 👤 Users

**Primary Users:**  
The company's intake / operations staff and the web team who receive the routed requests.

**User Need:**  
They need every client request to arrive already organized (service, budget, timeline, summary) with a clear decision: is it ready to follow up, or does it need a human to review it first?

---

## 🧠 How I Designed the Solution

I did not start by building the Zap. I followed the engineering journey from the course: **Understand → Design → Build → Test → Refine**.

- **Business analysis:** defined the work, the pain point, the current manual process, and the success criteria.
- **Workflow decomposition:** broke the process into 7 tasks (Receive → Understand → Structure → Validate → Route → Record → Notify) and mapped their dependencies.
- **Data design:** defined the input fields, the unstructured field (`request_text`), the structured fields AI must produce, and the result fields saved in the sheet.
- **Decision & exception design:** wrote clear rules for `VALID` vs `NEEDS_REVIEW`, and separated *business exceptions* (missing budget) from *technical errors* (a step failed).
- **Automation design:** mapped each task to a tool and a step in Zapier.

The main mental model behind every decision:

> **AI → Understand · Rule / Logic → Control · Human → Judge**

---

## ⚙️ How It Works

```text
Client submits Google Form
  ↓
Trigger: New Form Response (Google Forms)
  ↓
Unstructured request text
  ↓
AI by Zapier → extracts service_type, timeline, budget,
               request_summary, missing_information, request_status
  ↓
Paths by Zapier (decision on request_status)
  ↓
  ┌──────────────────┬──────────────────────┐
  │ VALID            │ NEEDS_REVIEW         │
  │ → web_team       │ → human_review       │
  └──────────────────┴──────────────────────┘
  ↓
Google Sheets → new row in "AI Client Intake Records"
  ↓
Email notification to the responsible person
```

---

## 🤖 Role of AI

What does AI do in this solution?

- Reads the free-text request and understands what the client is asking for.
- Extracts the required fields: `service_type`, `timeline`, `budget`.
- Writes a short `request_summary` using only what the client actually said.
- Lists `missing_information` (which required fields are absent or vague).
- Produces one clear signal, `request_status` (`VALID` / `NEEDS_REVIEW`), that the automation can act on.

### What AI Should NOT Do

- Invent, assume, or estimate any information the client did not provide (no guessing a budget, no turning "soon" into a specific date).
- Make the final operational decision on its own or talk to the client; routing follows fixed rules, and unclear requests always go to a human.

---

## 🔄 Automation

What parts of the process are automated?

- Capturing the request the moment the form is submitted (trigger).
- Extracting and structuring the information with AI.
- Deciding the path based on `request_status` (Paths by Zapier).
- Creating the record in Google Sheets with the extracted fields plus `requires_human` and `routing`.
- Sending the correct email: "New Valid Client Request" or "Client Request Requires Human Review".

The only manual step left is the human review of incomplete or unclear requests, and that is by design.

---

## 🗂️ Data

### Input

From the Google Form **Client Intake Form**:

| Field | Type |
|---|---|
| Client Name | Short answer |
| Email | Email |
| Company | Short answer |
| Request | Paragraph (unstructured text) |

### Output

One row per request in the Google Sheet **AI Client Intake Records**:

`client_name, email, company, request_text, service_type, request_summary, timeline, budget, missing_information, request_status, requires_human, routing, processed_at`

Plus an email notification to the web team (valid request) or to the reviewer (needs review).

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Google Forms | Entry point: receives client requests |
| Zapier | Builds and runs the automation workflow |
| AI by Zapier | Extracts structured information from the free-text request |
| Paths by Zapier | Routes the request based on `request_status` |
| Google Sheets | Stores one record per processed request |
| Email by Zapier | Notifies the web team or the human reviewer |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:**  
Let AI output a single clear signal, `request_status` (`VALID` or `NEEDS_REVIEW`), and build the Paths on that signal only.

**Why:**  
Instead of making the path logic interpret several fields (`budget = null`, `timeline = null`, `missing_information = ...`), one clear value keeps the decision logic simpler, easier to test, and easier to debug.

### Decision 2

**Decision:**  
Use Paths by Zapier with a fallback "Needs Human Review" path instead of Filter by Zapier.

**Why:**  
A filter would *stop* incomplete requests. The goal is to *route* the exception, not stop it. Anything that is not exactly `VALID` goes to human review, so no request is lost.

### Decision 3

**Decision:**  
Give AI strict extraction rules: use only explicit information, return `null` for missing fields, and treat vague timelines ("soon", "ASAP") as missing.

**Why:**  
Garbage in → garbage out. If AI guesses, the automation makes a wrong decision with confidence. Strict rules keep AI in the "Understand" role and leave judgment to humans.

---

## 🧪 Testing

I tested the AI output first (before building the Paths), then ran each case through the full workflow by submitting the Google Form and comparing **Expected vs Actual** at every layer (trigger → AI → status → path → sheet → email).

### Test 1 — Complete Request

**Input:**  
"We need a website for our company. Our budget is $5000. We need it next month."

**Expected Result:**  
`service_type = website`, `timeline = next month`, `budget = $5000`, `missing_information = NONE`, `request_status = VALID` → path *Valid Request* → sheet row with `requires_human = false`, `routing = web_team` → email "New Valid Client Request".

**Actual Result:**  
Matched the expected output at every layer.

**Status:** ✅

---

### Test 2 — Missing Budget

**Input:**  
"We need a website for our company. We need it next month."

**Expected Result:**  
`budget = null`, `missing_information = budget`, `request_status = NEEDS_REVIEW` → path *Needs Human Review* → `requires_human = true`, `routing = human_review` → email "Client Request Requires Human Review".

**Actual Result:**  
Matched the expected output at every layer.

**Status:** ✅

---

### Test 3 — Ambiguous Timeline

**Input:**  
"We need a website soon. Our budget is $5000."

**Expected Result:**  
`timeline = null`, `missing_information = timeline`, `request_status = NEEDS_REVIEW` → path *Needs Human Review*.

**Actual Result:**  
AI did not convert "soon" into a date; request routed to human review as expected.

**Status:** ✅

---

### Test 4 — Multiple Missing Fields

**Input:**  
"We need a website for our company."

**Expected Result:**  
`timeline = null`, `budget = null`, `missing_information = timeline, budget`, `request_status = NEEDS_REVIEW` → path *Needs Human Review*.

**Actual Result:**  
Matched the expected output at every layer.

**Status:** ✅

### Test Matrix

| Test | Scenario | Expected Status | Expected Path | Result |
|---|---|---|---|---|
| 01 | Complete request | `VALID` | Valid Request | ✅ |
| 02 | Missing budget | `NEEDS_REVIEW` | Human Review | ✅ |
| 03 | Ambiguous timeline | `NEEDS_REVIEW` | Human Review | ✅ |
| 04 | Multiple missing fields | `NEEDS_REVIEW` | Human Review | ✅ |

---

## 📸 Evidence

### Final Result

![Final Result](./screenshots/final-result.png)

### Workflow

![Workflow](./screenshots/workflow.png)

---

## ⚠️ Challenges & Failures

What didn't work as expected?

- Distinguishing a *business exception* (the client forgot the budget) from a *technical error* (the AI step or the Sheets step failed). At first both looked like "the request did not go through".
- Making sure the AI never fills in missing data on its own, especially for vague timelines like "soon" or "as soon as possible".

How did I solve it?

- Debugging by layer: when something failed, I looked for the **first** layer with a wrong result (Trigger → Input → AI → Status → Path → Sheets → Email) instead of changing everything at random.
- Tightening the AI prompt with explicit rules and examples (`"soon" → null`, "never estimate or guess") and testing the AI output alone before building the Paths.

---

## 📚 What I Learned

### Technical Lesson

How to turn unstructured text into structured data with AI by Zapier, build conditional routing with Paths by Zapier, map data into Google Sheets, and send dynamic email notifications. Also the difference between a **Trigger** (the event) and the **Input** (the data that comes with it).

### Problem-Solving Lesson

Think first, build later. Breaking the work into tasks, defining the data, and writing the decision rules *before* opening Zapier made the build straightforward and made testing systematic (Expected vs Actual, one layer at a time).

### AI Lesson

AI is best used to **understand**, not to **decide**. Giving it one clear job with strict rules and a single clear output signal, then letting fixed logic control the flow and a human judge the edge cases, is the difference between *using* AI and *engineering* with AI.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Update the original form row in Google Sheets instead of creating a new row, so each request has exactly one record.
- Add more routing paths by `service_type` (web team, marketing team, app team) instead of a single web team path.
- Send an automatic reply to the client asking for the specific missing information when the request is `NEEDS_REVIEW`.
- Add a `processed_at` timestamp and a simple dashboard to track how many requests are valid vs. need review.

---

## 🔗 Project Links

**Live Demo:** [Google Form link]

**Workflow:** [Zapier Zap link]

**Repository:** [GitHub repo link]

---

## 👩‍💻 Author

**Deema Redwan**
 
[LinkedIn](https://www.linkedin.com/in/YOUR-PROFILE) · [GitHub](https://github.com/YOUR-USERNAME)
