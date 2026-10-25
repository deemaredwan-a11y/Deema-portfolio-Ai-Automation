# U1 — UNDERSTAND THE WORK

# AI Smart Inquiry Management System

> An AI-powered automation system that processes student course inquiries, retrieves verified course information, and routes requests to automatic email responses or fallback handling using Zapier.

---

## 🎯 Problem

### The Context

Training centers receive inquiries from prospective students about available courses, registration dates, fees, schedules, and course duration.

These inquiries are often collected through online forms and require staff to review the questions, search for relevant course information, and prepare responses.

### The Problem

Manual inquiry management can lead to:

- Delayed responses to students.
- Repetitive administrative tasks.
- Inconsistent or incomplete answers.
- Difficulty handling unclear questions.
- Risk of providing outdated or incorrect course information.

### Why It Matters

Automating this process can reduce repetitive work, improve response consistency, and allow staff to focus on inquiries that require human attention.

---

## 💡 Solution

I designed an AI-powered inquiry management workflow using Zapier, Google Sheets, JavaScript, and Gmail.

The system is designed to:

1. Receive student inquiries through Google Forms and Google Sheets.
2. Use AI to extract the selected course and relevant question details.
3. Retrieve matching course information from a structured Google Sheets database.
4. Validate the available information using JavaScript.
5. Route inquiries through either an automatic response path or a fallback path.

The main goal is to combine AI understanding with rule-based validation before sending responses.

---

## 👤 Users

**Primary Users:**

- Training center administrators.
- Student support teams.
- Course registration coordinators.

**User Need:**

Staff need a reliable way to process course inquiries efficiently, provide accurate information, and identify requests that cannot be answered automatically.

Students need clear and timely responses to their questions.

---

## 🧠 How I Designed the Solution

I started by analyzing the existing manual inquiry process and dividing it into smaller tasks.

The process was separated into five main stages:

1. **Data Collection:** Capture student information and inquiry details.
2. **AI Extraction:** Identify the selected course and the information requested.
3. **Data Retrieval:** Search the course database for relevant information.
4. **Decision Logic:** Determine whether sufficient verified data exists to generate an automatic response.
5. **Response Handling:** Send an email or direct the inquiry to fallback handling.

A key design principle was to separate AI interpretation from business decisions.

AI is responsible for understanding the inquiry, while JavaScript rules determine whether an automatic response is appropriate.

---

## ⚙️ How It Works

```text
Student submits Google Form
          ↓
Google Sheets stores new inquiry
          ↓
Zapier detects New Spreadsheet Row
          ↓
AI by Zapier extracts inquiry details
          ↓
Google Sheets looks up course data
          ↓
JavaScript validates the information
          ↓
       Zapier Paths
          ↓
    Decision Routing
       /        \
      /          \
 AUTO_REPLY     Fallback
      ↓             ↓
 Gmail Email    JavaScript
      ↓         Fallback Logic
Student receives   ↓
a response      Further handling
```

The workflow separates straightforward inquiries from cases where information is missing or cannot be validated.

---

## 🤖 Role of AI

AI by Zapier is used to:

- Understand student inquiry text.
- Extract the relevant course name.
- Identify the types of questions being asked.
- Structure extracted information for subsequent workflow steps.

### What AI Should NOT Do

- Invent course prices, dates, or schedules.
- Treat unverified information as factual.
- Override JavaScript validation rules.
- Automatically answer inquiries when required information is missing.
- Make the final routing decision without rule-based checks.

The system follows a principle: **AI interprets the request; verified data and rules control the response.**

---

## 🔄 Automation

The workflow automates:

- Detecting newly submitted inquiries.
- Extracting inquiry details using AI.
- Looking up course information.
- Validating available data.
- Selecting the appropriate processing path.
- Sending Gmail responses through the AUTO_REPLY path.
- Executing fallback logic for inquiries that cannot be automatically answered.

---

## 🗂️ Data

### Input

Student inquiry data collected through Google Forms and stored in Google Sheets.

Examples include:

- Student name.
- Email address.
- Selected course.
- Inquiry message.

**Course Reference Data:**

The course database contains:

- Course ID.
- Course Name.
- Start Date.
- Duration.
- Fees.
- Schedule.
- Status.

Example courses:

- AI Fundamentals.
- Web Development.
- Graphic Design.
- Other / Not Sure.

### Output

The workflow produces:

- Structured inquiry information.
- Extracted course and question details.
- Course lookup results.
- Validation results.
- Routing decision: AUTO_REPLY or Fallback.
- An email response when the automatic reply conditions are satisfied.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Google Forms | Collect student inquiries |
| Google Sheets | Store inquiries and course information |
| Zapier | Connect and automate workflow steps |
| AI by Zapier | Extract and interpret inquiry details |
| Code by Zapier | Execute JavaScript validation and processing logic |
| Paths by Zapier | Route inquiries based on conditions |
| Gmail | Send automated email responses |
| GitHub | Document and showcase the project |

---

## 🔑 Key Design Decisions

### Decision 1 — Separate AI from Validation

**Decision:**

Use AI to understand inquiries and JavaScript to validate the retrieved course information.

**Why:**

AI can interpret flexible natural language, but critical information such as course fees and schedules should come from a verified data source.

This separation reduces the risk of unsupported responses.

### Decision 2 — Use Conditional Routing

**Decision:**

Create two workflow paths: AUTO_REPLY and Fallback.

**Why:**

Not every inquiry can be answered automatically.

The AUTO_REPLY path handles inquiries that satisfy validation requirements, while the Fallback path handles cases requiring additional processing.

### Decision 3 — Use Google Sheets as a Reference Database

**Decision:**

Store course information in a structured Google Sheets worksheet.

**Why:**

This makes course information easier to maintain and allows the workflow to retrieve information without relying on AI-generated facts.

---

## 🧪 Testing

### Test 1 — Form Submission

**Input:**

A student submits an inquiry through Google Forms.

**Expected Result:**

A new row is created in the connected Google Sheets worksheet.

**Actual Result:**

A test form submission successfully created a new spreadsheet row.

**Status:** ✅ Passed

---

### Test 2 — AUTO_REPLY Path Condition

**Input:**

An inquiry produces a validation value of `Can Auto Reply = true`.

**Expected Result:**

Zapier allows the AUTO_REPLY path to continue.

**Actual Result:**

The Zapier path test indicated that the AUTO_REPLY path would have continued.

**Status:** ✅ Passed (path condition only)

---

### Test 3 — Missing Course Information

**Input:**

A student asks about a course with missing or unavailable information.

**Expected Result:**

The validation logic prevents an unsupported automatic response and routes the inquiry to Fallback.

**Actual Result:**

Not yet documented with a complete end-to-end test.

**Status:** ⏳ Pending verification

---

### Test 4 — Automatic Email Delivery

**Input:**

A valid inquiry containing sufficient course information.

**Expected Result:**

Gmail sends a response using verified course information.

**Actual Result:**

The Gmail Send Email action is configured in the AUTO_REPLY path, but successful end-to-end delivery requires separate verification.

**Status:** ⏳ Pending verification

---

## 📸 Evidence

### Final Result

<img width="826" height="365" alt="image" src="https://github.com/user-attachments/assets/9131170e-946d-45aa-85ed-4307d2c106c4" />
<img width="937" height="213" alt="image" src="https://github.com/user-attachments/assets/064bce22-8ef1-48ab-bcff-f992ab4584bc" />
### 
<img width="757" height="517" alt="image" src="https://github.com/user-attachments/assets/a6cd493b-edeb-49d3-8ece-58fe0e19e816" />
<img width="1600" height="754" alt="WhatsApp Image 2026-10-25 at 12 59 24 PM" src="https://github.com/user-attachments/assets/958a6f60-cb6e-4310-b44d-3874eb06fcbc" />






### Workflow

<img width="466" height="578" alt="image" src="https://github.com/user-attachments/assets/a887313b-06a9-4ef2-9aa5-517f4fc71daa" />


---

## ⚠️ Challenges & Failures

### Challenge 1 — Working Within Zapier Plan Limitations

The workflow contains multiple actions and conditional paths, which may exceed the capabilities of the Zapier Free plan.

**Approach:**

Preserve the advanced workflow as a project implementation and consider a simplified version for free-plan execution.

### Challenge 2 — Preventing Incorrect Automatic Responses

AI-generated interpretations are not always sufficient to guarantee accurate course information.

**Solution:**

Use Google Sheets as the reference source and JavaScript to validate the information before allowing automatic responses.

### Challenge 3 — Handling Missing or Unclear Information

Some inquiries may reference unavailable courses or lack sufficient details.

**Solution:**

Introduce a Fallback path so that these inquiries are not treated as eligible for automatic responses.

---

## 📚 What I Learned

### Technical Lesson

I learned how to connect multiple applications through Zapier, use AI by Zapier to extract structured information, retrieve records from Google Sheets, implement JavaScript validation, and configure conditional workflow paths.

### Problem-Solving Lesson

I learned that successful automation starts with understanding the process rather than immediately choosing tools.

Breaking the workflow into smaller tasks helped me identify where AI, business rules, and fallback handling were needed.

### AI Lesson

I learned that AI should not be responsible for every decision in an automated system.

Combining AI interpretation with structured data and deterministic validation creates a more controlled and reliable workflow.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Complete end-to-end testing for both workflow paths.
- Add automatic processing-status updates to Google Sheets.
- Implement a complete human-review notification process.
- Add duplicate inquiry detection.
- Improve error handling and logging.
- Build a dashboard to monitor inquiries and response performance.
- Explore a lower-cost deployment architecture.

---

## 🔗 Project Links

**Live Demo:** [https://docs.google.com/forms/d/e/1FAIpQLSelf74Ln50ehu3-D2KTjWqJ8RKg5ywsOQBZqLZHSp5Ew5cgpQ/viewform?usp=dialog]

**Workflow:** [https://zapier.com/editor/383331585/published/383331585/fields]

**Repository:** [https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/edit/main/01-Process%20%26%20Data%20Thinking]

---

## 👩‍💻 Author

**Deema Redwan **

[https://www.linkedin.com/in/deema-radwan-a06a19370/] · [https://github.com/deemaredwan-a11y]
