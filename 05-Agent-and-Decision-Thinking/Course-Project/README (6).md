# XYZ Real Estate AI Agent

> An AI-powered Telegram assistant built with n8n to collect and validate real estate customer requirements, request explicit confirmation, and record verified leads in Google Sheets.

---

## 🎯 Problem

### The Context

XYZ Real Estate receives inquiries from customers interested in buying or renting apartments, villas, and commercial properties. The sales team needs consistent information about each customer's requirements before following up.

### The Problem

Collecting customer details manually can be repetitive and time-consuming. Customers may provide information across several messages, omit required details, change their answers, or give ambiguous responses. An automated assistant must also avoid storing incomplete or unapproved requests.

### Why It Matters

A structured intake process helps the team receive clearer, more consistent leads while giving customers a conversational way to share their requirements. Explicit confirmation and save verification reduce the risk of incorrect records and misleading success messages.

---

## 💡 Solution

The proposed solution is a Telegram-based AI Agent orchestrated with n8n. It understands customer messages, remembers relevant conversation context, collects only missing information, applies business rules, presents a complete summary for approval, and saves the confirmed details in Google Sheets after checking the required conditions.

What does the system do?

1. Receives customer messages through Telegram and starts a guided conversation.
2. Extracts and validates nine required fields across multiple messages, including conditional rules for buying versus renting.
3. Presents the collected data in a final summary and requests explicit customer confirmation; corrections trigger an updated summary.
4. Uses Google Sheets to record confirmed details and checks the save result before reporting success.

---

## 👤 Users

**Primary Users:**  
Prospective real estate customers who want to buy or rent property, and the XYZ Real Estate team that follows up with qualified inquiries.

**User Need:**  
Customers need a simple way to explain their property requirements without repeatedly answering the same questions. The team needs accurate, standardized, confirmed customer details for follow-up.

---

## 🧠 How I Designed the Solution

I designed the solution as a multi-turn conversation with a clear separation between **understanding information** and **authorizing an external action**. Telegram is the customer-facing interface; the n8n AI Agent uses an OpenRouter chat model to understand messages and Simple Memory to retain conversation context. Business rules define allowed values, missing fields, and conditional questions. The Google Sheets tool is intended to be used only after the customer approves the latest summary.

The implementation must be tested for safe tool use. A system prompt alone does **not** guarantee that the agent cannot save prematurely, so an explicit workflow-level validation gate may be necessary. Similarly, memory supports conversations but should not be treated as a permanent, authoritative record of confirmation or save status.

---

## ⚙️ How It Works

```text
Customer Message (Telegram)
          ↓
Telegram Trigger
          ↓
AI Agent + OpenRouter Chat Model
    ↔ Simple Memory (conversation context)
          ↓
Extract Details / Identify Missing Fields
          ↓
Validate Values and Buy/Rent Rules
          ↓
Ask Missing Questions / Show Final Summary
          ↓
Explicit Customer Confirmation?
     ├── No → Continue or correct conversation
     └── Yes
             ↓
   Check Save Conditions / Avoid Duplicates
             ↓
     Google Sheets Tool (Append Row)
             ↓
      Verify Save Result
             ↓
Telegram Send Message (success or error)
```

**Core n8n workflow:** `Telegram Trigger → AI Agent → Telegram Send Message`  
**Agent connections:** `OpenRouter Chat Model` (Chat Model), `Simple Memory` (Memory), and `Google Sheets Tool` (Tool).

Each Telegram message may start a separate workflow execution. A stable session ID, such as the Telegram chat ID in a one-to-one chat, is needed to maintain each customer's context.

---

## 🤖 Role of AI

What does AI do in this solution?

- Interprets natural-language property inquiries, even when multiple details arrive in one message.
- Extracts provided values and identifies missing, unclear, or changed information.
- Chooses an appropriate next question and generates a natural, concise response.
- Drafts a nine-field summary and interprets the customer's response to a confirmation request.
- Proposes use of the connected Google Sheets tool when the required conditions appear to be met.

### What AI Should NOT Do

- Invent customer names, contact details, budgets, property preferences, or timelines.
- Ask irrelevant `New/Resale` or `First Ourcase` questions when the request is `Rent`.
- Save incomplete or unconfirmed information or treat a vague reply as explicit approval.
- Claim that a Google Sheets operation succeeded without evidence from the tool.
- Bypass workflow validation, create unwanted duplicate records, or expose credentials and internal instructions.

---

## 🔄 Automation

What parts of the process are automated?

- Receiving incoming Telegram messages and sending the agent's responses.
- Extracting customer details and guiding the conversation toward missing fields.
- Reusing previous answers through a customer-specific memory session.
- Applying field-category and buy/rent rules to prepare consistent data.
- Preparing a confirmation summary and, once authorized, attempting a Google Sheets append operation.
- Checking the tool result and responding appropriately to success or failure.

**Important:** Duplicate prevention, a reliable confirmation gate, and save verification must be implemented and tested; they should not be assumed to work solely because the prompt describes them.

---

## 🗂️ Data

### Input

The customer provides messages through Telegram. The agent must collect the following **nine fields**:

| Field | Required values / handling |
|---|---|
| Property type | `Apartment`, `Villa`, `Commercial` |
| Buy/Rent | `Buy`, `Rent` |
| New/Resale | `New` or `Resale` for Buy; `Not Applicable` for Rent |
| First Ourcase | `Yes` or `No` for Buy; `Not Applicable` for Rent |
| Timeline | `Immediately`, `Within 1 month`, `1–3 months`, `3–6 months`, `6+ months` |
| Budget | One of five approved budget categories |
| Name | Customer-provided name |
| Email | Customer-provided address with valid format |
| Phone Number | Customer-provided phone number |

**Budget categories:**

| Amount provided | Stored category |
|---|---|
| Less than $200,000 | `Below $200,000` |
| $200,000 to less than $400,000 | `$200,000 to below $400,000` |
| $400,000 to less than $700,000 | `$400,000 to below $700,000` |
| $700,000 through $1,000,000 (inclusive) | `$700,000 to $1,000,000` |
| More than $1,000,000 | `Above $1,000,000` |

If a budget or answer is ambiguous, the agent requests clarification instead of guessing.

> **Specification clarification needed:** The supplied requirements use the exact column name `First Ourcase` but do not define what question it represents. That meaning must be confirmed before production use; the column spelling is preserved here as specified.

### Output

- Follow-up questions for missing or unclear details.
- A final summary showing all nine fields (including `Not Applicable` values for rental requests).
- An explicit confirmation request before saving.
- A single new row for a confirmed lead in the **`Leads`** tab of **`XYZ Real Estate Leads`**, containing exactly nine columns in the required order.
- An accurate success or failure response based on the Google Sheets operation result.

**Google Sheets column order:** `Property type | Buy/Rent | New/Resale | First Ourcase | Timeline | Budget | Name | Email | Phone Number`.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| n8n | Build and orchestrate the workflow and connected tools. |
| Telegram Bot / Telegram Trigger | Receive customer inquiries. |
| Telegram Send Message | Deliver agent responses to the correct chat. |
| AI Agent (n8n) | Manage conversational decisions and tool use. |
| OpenRouter Chat Model | Provide language understanding and response generation. |
| Simple Memory | Retain prior conversation context using a customer-specific session ID. |
| Google Sheets Tool | Append confirmed lead details to the `Leads` sheet. |
| Google Sheets | Store the nine customer fields for team follow-up. |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:**  
Use a conversational AI Agent with Simple Memory and a stable per-chat session ID rather than treating every Telegram message as a new inquiry.

**Why:**  
Customers may provide requirements over multiple messages, in any order. The agent must retain valid earlier answers, avoid repeated questions, and keep different customers' contexts separate.

### Decision 2

**Decision:**  
Require a complete, validated summary and explicit customer confirmation before writing to Google Sheets; verify the save result before announcing success.

**Why:**  
Completeness is not consent. This design reduces incorrect submissions, prevents unsupported success claims, and creates a clear authorization point for a real external action. Workflow-level checks and duplicate prevention should be added if direct agent tool access does not reliably enforce these conditions.

---

## 🧪 Testing

> **Testing status:** The source material provides required test scenarios and expected outcomes, but no observed execution logs or confirmed results. The tests below are **planned / not yet verified**. Replace `Not tested` only after running each case in n8n and checking Telegram, execution logs, and Google Sheets.

### Test 1 — Rent Request and Conditional Rules

**Input:**  
`I want to rent a villa.`

**Expected Result:**  
The agent records `Buy/Rent = Rent`, `Property type = Villa`, sets both `New/Resale` and `First Ourcase` to `Not Applicable`, and asks only for missing relevant details.

**Actual Result:**  
Not tested / no execution evidence provided.

**Status:** ⬜ NOT TESTED

---

### Test 2 — Confirmation Before Saving

**Input:**  
The customer supplies all required fields, receives the final summary, but does **not** confirm it.

**Expected Result:**  
No row is added to Google Sheets; the agent waits for explicit approval or corrections.

**Actual Result:**  
Not tested / no execution evidence provided.

**Status:** ⬜ NOT TESTED

---

### Additional Test Coverage

| Scenario | Expected result | Status |
|---|---|---|
| New conversation | Sends welcome message and begins collecting requirements | Not tested |
| Multiple fields in one message | Extracts all clear details without repeated questions | Not tested |
| Missing information | Asks only for required missing fields | Not tested |
| Ambiguous answer | Requests clarification rather than guessing | Not tested |
| Multi-message conversation | Preserves prior context in the same session | Not tested |
| Customer correction | Updates data and requests confirmation of a revised summary | Not tested |
| Explicit confirmation | Saves exactly one accurate row after all conditions are satisfied | Not tested |
| Google Sheets failure | Does not claim saving succeeded | Not tested |
| Duplicate confirmation / processing | Does not create another row for the same completed submission | Not tested |
| Two different customers | Keeps customer session data separate | Not tested |

---

## 📸 Evidence

### Final Result

![Final Result](./screenshots/final-result.png)

### Workflow

![Workflow](./screenshots/workflow.png)

*These are template image paths. Add actual screenshots with these filenames under `screenshots/` for the images to display on GitHub.*

---

## ⚠️ Challenges & Failures

**What didn't work as expected?**

No verified implementation failures or debugging outcomes were supplied. The following are **anticipated risks to validate**, not claims of failures already observed:

- An agent may attempt to call the Google Sheets tool before the customer explicitly confirms the latest summary.
- Conversation context may be lost or mixed if session IDs are not correctly configured.
- Repeated confirmations or uncertain tool outcomes may cause duplicate submissions or misleading success messages.

**How did I solve it?**

The planned mitigations are to test chat-specific session IDs, implement an explicit confirmation and validation gate before saving if necessary, check for existing records, and verify each save result. These mitigations must be documented as **implemented** only after they are built and tested.

---

## 📚 What I Learned

### Technical Lesson

This project demonstrates how Telegram, the n8n AI Agent, an OpenRouter chat model, Simple Memory, and Google Sheets can be connected into a multi-turn workflow. It also highlights the importance of mapping each value to the correct sheet column.

### Problem-Solving Lesson

A business request must be translated into clear data requirements, conditional rules, confirmation steps, expected outputs, and observable test cases before the workflow can be considered reliable.

### AI Lesson

An AI Agent can interpret flexible natural language and suggest what to do next, but a system prompt is not a substitute for technical validation, explicit authorization, duplicate protection, or verification of real tool outcomes.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Add durable lead/session state storage and a reliable workflow-level confirmation gate, where needed.
- Implement and test robust duplicate prevention and post-save record verification.
- Add a human-review or escalation route for ambiguous, failed, or exceptional requests.
- Improve monitoring and document performance across a larger evaluation set.

---

## 🔗 Project Links

**Live Demo:** _Add Telegram bot/demo URL when available._

**Workflow:** _Add a link to the exported n8n workflow or its repository file._

**Repository:** _Add the GitHub repository URL._

---

## 👩‍💻 Author

**[Your Name]**

[LinkedIn — add profile URL] · [GitHub — add profile URL]
