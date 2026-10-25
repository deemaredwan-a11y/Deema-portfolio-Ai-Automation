# U1 — UNDERSTAND THE WORK

## Process & Data Thinking

> Before automating the work, understand the work.

In this unit, I learned how to transform real-world business problems into structured processes that can be analyzed, designed, and improved using AI Automation.

Through two practical projects, I applied process decomposition, data analysis, AI-based information extraction, rule-based decision-making, and exception handling.

The main focus was to understand the work before choosing the tools, identify what information the system needs, and determine when AI, fixed rules, or human intervention should be used.

---

## 🎯 Capability

**Process & Data Thinking**

The ability to analyze real-world business processes, break them down into workflows, tasks, and subtasks, identify structured and unstructured data, and design reliable decision-making logic before implementing automation.

---

## 🧠 What I Learned

Throughout this unit, I developed the ability to:

- **Understand the Work:** Identify business problems, understand their context, and define expected outcomes.
- **Decompose the Work:** Break down complex processes into workflows, tasks, and subtasks with clear dependencies.
- **Understand the Input:** Identify required data and distinguish between structured and unstructured information.
- **Design the Decision:** Determine when AI is appropriate and when deterministic, rule-based logic provides better control.
- **Handle Exceptions Safely:** Recognize missing information, ambiguous requests, and cases requiring human review.
- **Design the Automation:** Convert process requirements into executable workflows using automation tools.
- **Test and Validate:** Compare expected and actual results to identify workflow issues and evaluate system behavior.

---

## 🛠️ Projects

### 01 — Guided Project

**Project: AI Client Intake & Request Routing System**

An AI-powered automation system that processes incoming client requests for a digital services company, extracts relevant information, validates request completeness, and routes requests to the appropriate team or human reviewer.

- **Problem:** Service companies receive client requests in different formats, often with missing budgets, unclear timelines, or incomplete project details. Manual processing requires time and increases the risk of delays and lost information.
- **Solution:** Built a Zapier workflow that receives client requests through Google Forms, uses AI to extract structured information, determines whether the request is `VALID` or `NEEDS_REVIEW`, and routes it to the web team or human review. The system records the results in Google Sheets and sends email notifications.
- **Tools:** Google Forms, Zapier, AI by Zapier, Paths by Zapier, Google Sheets, Email by Zapier.

**Key Achievement:** Documented four successful test scenarios covering complete requests, missing budgets, ambiguous timelines, and multiple missing fields.

https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/tree/main/01-Process%20%26%20Data%20Thinking/Course-Project
### 02 — Independent Project

**Project: AI Smart Inquiry Management System**

An AI-powered automation system designed to manage student inquiries about training courses by understanding incoming questions, retrieving course information, validating available data, and selecting the appropriate response path.

- **Problem:** Training centers receive repetitive inquiries about course fees, schedules, and duration. Manual handling can cause delayed responses, repetitive administrative work, and inconsistent information.
- **Solution:** Designed a workflow that collects inquiries through Google Forms and Google Sheets, uses AI by Zapier to extract inquiry details, retrieves matching course information, validates the results using JavaScript, and routes requests through either AUTO_REPLY or Fallback.
- **Tools:** Google Forms, Google Sheets, Zapier, AI by Zapier, Code by Zapier (JavaScript), Paths by Zapier, Gmail.

**Key Achievement:** Implemented a workflow combining AI extraction, database lookup, JavaScript validation, and conditional routing. Form submission and automatic-reply path conditions were tested successfully; additional end-to-end verification remains pending.

https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/tree/main/01-Process%20%26%20Data%20Thinking/Independent-Project
---

## 🔍 Engineering Focus

During this unit, I focused on six essential engineering questions and applied them across both projects.

1. **What is the real problem?** Identify repetitive manual processes, delays, missing information, and opportunities for improvement.
2. **How does the process work?** Break down each process into clear stages, from receiving a request to producing the appropriate output.
3. **What data does the system need?** Identify structured fields such as names, email addresses, budgets, course information, and unstructured inputs such as free-text inquiries.
4. **When should AI be used instead of rules?** Use AI to interpret natural language and extract information, while relying on explicit rules to control validation and routing.
5. **What happens when information is missing or unclear?** Design fallback paths and human-review mechanisms to prevent unsupported responses or incorrect processing.
6. **How can the process become an automation workflow?** Map each task to the appropriate tools, connect workflow steps, and evaluate the results through testing.

---

## 📌 Key Takeaway

**A good automation starts with a well-understood process, not with a tool.**

The most important lesson I learned is that building an effective AI automation system requires more than connecting applications.

It requires understanding the business problem, defining the data structure, separating AI responsibilities from deterministic rules, and designing safe ways to handle exceptions.

Both projects reinforced a core engineering principle:

**AI → Understand | Rules → Control | Humans → Judge**

AI helps interpret information, rules control workflow behavior, and humans handle situations requiring judgment.

---

## 📈 Capability Progress

- [x] Understand the problem and define the desired outcome.
- [x] Decompose the work into clear steps.
- [x] Identify structured and unstructured inputs and outputs.
- [x] Design AI and rule-based decision logic.
- [x] Identify exceptions requiring human review.
- [x] Implement and document automation workflows.
- [x] Test and document key scenarios in the Guided Project.
- [ ] Complete end-to-end testing for all Independent Project scenarios.

---

## 🔗 Related Projects

- [Guided Project — AI Client Intake & Request Routing System](./course-project/)
- [Independent Project — AI Smart Inquiry Management System](./independent-project/)

---

**Next:** U2 — BUILD THE SYSTEM
