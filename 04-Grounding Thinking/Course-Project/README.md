# TECHFIX — AI Computer Troubleshooting Assistant

> An Arabic-language AI assistant that helps users understand and troubleshoot common computer problems through clear, safe, knowledge-grounded guidance.

---

## 🎯 Problem

### The Context

Computer users frequently encounter slow performance, limited storage space, and Wi-Fi or internet connectivity issues. Identifying the cause can be difficult, particularly when available technical instructions are complex or do not match the user's situation.

### The Problem

Users need accessible, clear, and safe troubleshooting guidance in Arabic that is relevant to their reported symptoms. Generic or unsupported advice may confuse users or encourage inappropriate actions.

### Why It Matters

Reliable initial guidance helps users understand a problem, take appropriate first steps, and recognize when there is insufficient information or when a qualified technician is needed.

---

## 💡 Solution

**TECHFIX** is an AI assistant built with **Dify**. It uses **Retrieval-Augmented Generation (RAG)** to ground its responses in specialized technical knowledge documents rather than relying solely on general model-generated answers.

What does the system do?

1. Accepts users' computer problem descriptions in Arabic.
2. Retrieves relevant information from its knowledge base.
3. Uses a large language model to produce clear initial guidance based on the retrieved material and safety instructions.
4. Asks follow-up questions when necessary, communicates knowledge limitations, or recommends a technician when the issue exceeds its scope.

**Initial scope:** Slow computers, storage space, internet connectivity, and safety and assistance boundaries.

---

## 👤 Users

**Primary Users:**  
Computer users seeking basic technical assistance in Arabic, especially nontechnical users.

**User Need:**  
To understand symptoms and possible causes and receive clear, safe troubleshooting steps without being given an unjustifiably certain diagnosis.

---

## 🧠 How I Designed the Solution

I designed TECHFIX around a **structured knowledge base** instead of relying on the language model alone. The material is organized into four topic-specific documents to support relevant retrieval. **Knowledge Retrieval** searches for useful context, and the **LLM** uses that context together with system instructions to formulate its answer.

I also defined clear boundaries: the assistant cannot inspect a computer, perform repairs, confirm a root cause without enough evidence, or recommend unsupported or unsafe actions. The evaluation approach distinguishes **retrieval quality** from **answer quality**, because retrieving relevant information does not guarantee that the model will use it correctly or completely.

---

## ⚙️ How It Works

```text
User Question / Problem Description
                 ↓
               Start
                 ↓
       Knowledge Retrieval
                 ↓
       LLM + System Prompt
                 ↓
              Answer
```

- **Start:** Receives the user's question and reported symptoms.
- **Knowledge Retrieval:** Searches the knowledge base for relevant passages.
- **LLM:** Generates a response using the question, instructions, and retrieved context.
- **Answer:** Displays guidance, requests clarification, or explains assistance limitations.

---

## 🤖 Role of AI

What does AI do in this solution?

- Interprets the user's problem description and uses retrieved material to formulate a relevant response.
- Presents technical guidance in organized, understandable Arabic.
- Handles insufficient information by asking for clarification or acknowledging uncertainty according to the assistant's instructions.

### What AI Should NOT Do

- Invent technical facts or assert a definite cause without sufficient evidence.
- Claim to have inspected the device or performed repairs.
- Suggest steps that could damage the system or cause data loss without respecting safety constraints and warnings.
- Operate outside its knowledge scope; it should recommend a qualified technician when necessary.

---

## 🔄 Automation

What parts of the process are automated?

- Routing the user's question through the **Dify Chatflow**.
- Retrieving relevant passages from the knowledge base.
- Passing the question and retrieved context to the language model, generating an answer, and displaying it.

**Note:** This automates the guidance workflow; it does **not** automatically diagnose or repair a user's device.

---

## 🗂️ Data

### Input

- User questions and descriptions of computer symptoms.
- Four technical knowledge documents available for retrieval.
- Assistant instructions defining scope, safety rules, and response behavior.

### Output

- Arabic answers with initial troubleshooting information or steps grounded in the available context.
- Clarifying questions when the problem description is incomplete.
- A statement of knowledge limitations or a recommendation to consult a technician when appropriate.

**Knowledge Base:**

```text
knowledge-base/
├── 01-بطء-الحاسوب.md
├── 02-مساحة-التخزين.md
├── 03-اتصال-الإنترنت.md
└── 04-السلامة-وحدود-المساعدة.md
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
| --- | --- |
| Dify | Build and manage the AI assistant application. |
| Chatflow | Structure the question, retrieval, generation, and response stages. |
| Knowledge Base | Store organized technical reference documents. |
| RAG / Knowledge Retrieval | Retrieve relevant information to ground responses. |
| LLM | Generate answers using the question, instructions, and retrieved context. |
| System Prompt | Define the assistant's role, response rules, and safety boundaries. |
| Markdown | Prepare knowledge documents and project documentation. |
| GitHub | Organize project files, documentation, and evidence. |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:**  
Use a specialized knowledge base and **RAG** rather than unconstrained, source-independent responses.

**Why:**  
To guide the assistant toward relevant documented information and reduce unsupported answers, while recognizing that the final output still needs evaluation.

### Decision 2

**Decision:**  
Define a limited troubleshooting scope and include a dedicated document covering safety and assistance boundaries.

**Why:**  
To avoid unjustified diagnoses or potentially harmful instructions, protect users' systems and data, and identify cases requiring professional assistance.

---

## 🧪 Testing

**Preliminary tests** covered slow computer performance, storage issues, internet connectivity, and safety-related questions. The results below reflect the available project documentation; exact test prompts and performance scores were not supplied.

### Test 1 — Common Troubleshooting Questions

**Input:**  
Initial questions about slow computer performance or limited storage space.

**Expected Result:**  
Retrieve relevant knowledge and provide clear, safe troubleshooting guidance.

**Actual Result:**  
Early tests showed useful guidance for some cases, but the documentation does not report an individual result or success score for each question.

**Status:** ⚠️ Preliminary observation; detailed test cases and results still need to be documented.

---

### Test 2 — Internet Connection Issue

**Input:**  
A question about an internet connection problem. The exact wording of the question was not documented.

**Expected Result:**  
Retrieve appropriate information and produce a sufficiently detailed, clear response using that information.

**Actual Result:**  
One response was insufficiently detailed despite retrieving results from the relevant knowledge document.

**Status:** ❌ The response did not meet the expected completeness level in this case.

**Evaluation Focus:**

- **Retrieval Quality:** Were the retrieved passages relevant and sufficient?
- **Answer Quality:** Was the answer correct, clear, complete, grounded, and safe?

> These observations are not a comprehensive evaluation. A structured test dataset and documented results are needed to assess overall performance.

---

## 📸 Evidence

<img width="1276" height="353" alt="image" src="https://github.com/user-attachments/assets/578ec4a2-0884-481f-88b3-6923e49191a4" />
<img width="1317" height="680" alt="image" src="https://github.com/user-attachments/assets/924817ab-ca00-4a62-ac1c-4e6bd218504f" />
<img width="1270" height="708" alt="image" src="https://github.com/user-attachments/assets/cc592e66-518b-4a44-842c-90920650ac68" />
<img width="1277" height="722" alt="image" src="https://github.com/user-attachments/assets/8b93b919-e7d4-4dad-9d8c-d8af4cf2dbac" />






Other evidence described in the project plan includes screenshots of assistant setup, the knowledge base, retrieval settings, and test results. Screenshot filenames in the source documentation are organizational examples, not verified links.

---

## ⚠️ Challenges & Failures

What didn't work as expected?

- Some preliminary responses lacked sufficient detail even when relevant information had been retrieved, particularly in one internet connectivity case.
- The initial tests demonstrated that relevant retrieval results alone do not guarantee a complete final answer.

How did I address it?

- Identified the need to evaluate retrieval and answer generation separately to locate the source of the problem.
- Defined an improvement process: inspect retrieved passages, evaluate the final answer, identify the failure type, adjust the appropriate component, and retest.

**Current status:** The supplied documentation does not confirm that a specific improvement was implemented and successfully retested. These steps are therefore presented as an improvement plan, not a completed fix.

---

## 📚 What I Learned

### Technical Lesson

Building a **RAG** assistant involves more than connecting an LLM to documents. Knowledge quality, document preparation, chunking, and retrieval affect the usefulness of the response.

### Problem-Solving Lesson

When an answer is weak, it is important to identify the cause before changing the system: the knowledge content, retrieval process, available context, or answer generation may be responsible.

### AI Lesson

**Groundedness**, **completeness**, and **safety** are distinct evaluation dimensions. Retrieved information may be relevant while the final answer remains inadequate. Acknowledging uncertainty is better than inventing a solution.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Build a documented test dataset covering direct questions, paraphrased questions, missing knowledge, incomplete questions, out-of-scope requests, and safety-related scenarios.
- Improve knowledge content, retrieval settings, and model instructions based on analysis of observed failures.
- Retest scenarios after changes and assess **relevance, correctness, groundedness, completeness, clarity, safety**, and **uncertainty handling**.

---

## 🔗 Project Links

[Live Demo:](https://udify.app/chat/LiQAnMjvd4jTWINg)


[Repository:](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/edit/main/04-Grounding%20Thinking)

---

## 👩‍💻 Author

**[Deema Redwan ]**

[LinkedIn](https://www.linkedin.com/in/deema-radwan-a06a19370/) · [GitHub](https://github.com/deemaredwan-a11y)

---

*TECHFIX — Turning Technical Knowledge into Practical Guidance.*
