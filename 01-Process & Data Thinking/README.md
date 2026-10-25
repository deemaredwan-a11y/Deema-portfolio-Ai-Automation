# U1 — UNDERSTAND THE WORK

## Process & Data Thinking

> Before automating the work, understand the work.

In this unit, I learned how to transform a real-world problem into a clearly defined process that can be analyzed, structured, and improved using AI Automation.

The goal was not to start by choosing an automation tool, but to understand the problem, analyze the workflow and its data, and design the decisions and rules required to build an effective solution.

---

## 🎯 Capability

**Process & Data Thinking**

The ability to analyze real-world processes, break them down into clear steps, identify inputs and outputs, design decision-making logic, and recognize situations that require human intervention before implementing automation.

---

## 🧠 What I Learned

Throughout this unit, I developed the ability to:

- **Understand the Work:** Identify the real problem, understand the business context, and define the desired outcomes.
- **Decompose the Work:** Break down a process into workflows, tasks, and subtasks to simplify analysis and implementation.
- **Understand the Input:** Identify required data and distinguish between structured and unstructured information.
- **Design the Decision:** Determine which tasks benefit from AI and which are better handled using rule-based logic.
- **Handle Exceptions Safely:** Manage missing data, unclear inquiries, and situations requiring human review.
- **Design the Automation:** Transform process requirements into an executable automation workflow.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** AI Smart Inquiry Management System

An AI-powered automation system designed to manage student inquiries about training courses. The system analyzes incoming questions, retrieves relevant information from a structured database, and determines the appropriate processing path.

- **Problem:** Training centers receive repetitive inquiries about course fees, schedules, and duration. Handling these requests manually requires time and effort and may result in delayed responses or inaccurate information.
- **Solution:** Designed an automated workflow that collects inquiries through Google Forms, uses AI by Zapier to extract relevant information, retrieves course data from Google Sheets, and applies JavaScript validation to route requests through either AUTO_REPLY or Fallback.
- **Tools:** Google Forms, Google Sheets, Zapier, AI by Zapier, JavaScript, Paths by Zapier, Gmail.

[Explore the Project](./course-project/)

### 02 — Independent Project

**Project:** [Independent Project Name]

An independent project focused on applying Process & Data Thinking to a different real-world problem by analyzing the workflow, identifying required data, designing decision-making logic, implementing the solution, and evaluating its results.

- **Problem:** [Describe the real-world problem addressed by your independent project.]
- **Solution:** [Briefly explain how your solution addresses the problem.]
- **Tools:** [List the tools and technologies used.]

[Explore the Project](./independent-project/)

---

## 🔍 Engineering Focus

During this unit, I focused on six essential engineering questions and applied them to the AI Smart Inquiry Management System:

1. **What is the real problem?** Reduce the time and effort required to process student inquiries while improving response accuracy.
2. **How does the process work?** Receive an inquiry, understand its content, retrieve relevant information, validate the data, and determine the appropriate response.
3. **What data does the system need?** Course name, inquiry text, student email address, and course information stored in Google Sheets.
4. **When should AI be used instead of rules?** AI is used to understand and classify inquiries, while JavaScript handles data validation and routing decisions.
5. **What happens when information is missing?** The inquiry is routed to a Fallback path instead of generating an unsupported automatic response.
6. **How can the process become an automation workflow?** By integrating Google Forms, Google Sheets, AI by Zapier, Code by Zapier, Paths, and Gmail.

---

## 📌 Key Takeaway

**A good automation starts with a well-understood process, not with a tool.**

The most important lesson I learned is that successful automation is not simply about connecting tools. It begins with understanding the problem, designing the process, and clearly defining the responsibilities of each system component.

I also learned that AI is not always the best solution for every task. In some cases, deterministic rules provide greater control and reliability, especially when validating data and making decisions.

---

## 📈 Capability Progress

- [ ] Understand the problem and define the desired outcome.
- [ ] Decompose the work into clear steps.
- [ ] Identify inputs and outputs.
- [ ] Design AI and rule-based decision logic.
- [ ] Identify exceptions requiring human review.
- [ ] Implement the core workflow and document its design.
- [ ] Complete end-to-end testing and document the final results.

---

## 🔗 Related Projects

- [Guided Project — AI Smart Inquiry Management System](./course-project/)
- [Independent Project](./independent-project/)

---

**Next:** U2 — BUILD THE SYSTEM
