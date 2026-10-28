Shashti Amna — AI Digital Safety Awareness Chatbot

> An Arabic-language AI chatbot that helps followers of the Shashti Amna awareness campaign access information about online blackmail, digital violence, prevention, and safe response steps using a dedicated knowledge base.

---

## 🎯 Problem

### The Context

The **Shashti Amna (شاشتي آمنة)** campaign raises awareness about online blackmail and digital violence affecting women and girls, with a focus on the Gaza context. Followers may have questions about protecting their digital privacy, recognizing risks, and responding to threatening online behavior.

### The Problem

Important awareness information may be spread across long documents, making it difficult for followers to quickly find clear answers to their specific questions.

### Why It Matters

Accessible, understandable information can help people identify warning signs, avoid unsafe reactions, preserve relevant evidence, and seek appropriate support. A chatbot can make approved awareness materials easier to navigate, but it is **not** a replacement for trained professionals or emergency support.

---

## 💡 Solution

I built a knowledge-based chatbot in **Dify** to answer followers' questions using the campaign's uploaded awareness resources.

What does the system do?

1. Receives a question from a follower.
2. Searches the uploaded campaign documents for relevant information.
3. Passes the retrieved context and the question to a **Gemini** language model.
4. Displays the generated answer to the follower.

The chatbot is designed to support educational conversations rather than investigate individual cases or take action on a user's behalf.

---

## 👤 Users

**Primary Users:**  
Followers of the Shashti Amna campaign, particularly women and girls seeking accessible information about digital safety and online blackmail.

**User Need:**  
To obtain clear, relevant, Arabic-language guidance about prevention, digital violence, online blackmail, and possible next steps after an incident.

---

## 🧠 How I Designed the Solution

I chose a **Retrieval-Augmented Generation (RAG)** approach rather than relying only on the language model's general knowledge. I organized the available campaign resources into a Dify knowledge base and connected it to a simple Chatflow.

The flow separates **information retrieval** from **answer generation**. This design allows the model to use relevant passages from the uploaded materials when responding. It does not, by itself, guarantee that every answer is accurate or fully grounded; that requires testing and review.

I kept the workflow simple so its individual stages can be inspected and improved.

---

## ⚙️ How It Works

```text
Follower's Question
        ↓
User Input / Start
        ↓
Knowledge Retrieval
(Shashti Amna Knowledge Base)
        ↓
Gemini LLM
(Question + Retrieved Context)
        ↓
Answer
(Response to Follower)
```

The visible Dify workflow consists of four connected nodes: **User Input → Knowledge Retrieval → LLM (Gemini) → Answer**.

---

## 🤖 Role of AI

What does AI do in this solution?

- Interprets the follower's question.
- Uses retrieved document passages as context for generating a response.
- Produces a natural-language explanation of relevant awareness material.

### What AI Should NOT Do

- Invent facts, campaign policies, legal conclusions, or support contacts.
- Guarantee that an answer is correct merely because a relevant document was retrieved.
- Identify or confront an alleged blackmailer or access a follower's personal accounts.
- Provide risky instructions that could expose private information or destroy evidence.
- Present itself as a lawyer, counselor, investigator, or emergency-response service.

These are **intended safety boundaries** for the chatbot. The screenshots do not establish that all of them have been configured or verified in the model instructions.

---

## 🔄 Automation

What parts of the process are automated?

- Receiving a follower's question within the Dify chat interface.
- Retrieving relevant excerpts from the uploaded knowledge documents.
- Generating and displaying a response through the LLM and Answer nodes.

The supplied workflow does **not** show automatic reporting, human handoff, message delivery to external platforms, or case management.

---

## 🗂️ Data

### Input

- The follower's question or description of an issue.
- Retrieved context from the campaign's knowledge base.

The knowledge base shown in Dify includes three documents:

1. **العنف الإلكتروني وسبل المواجهة** — Digital Violence and Ways to Address It (PDF).
2. **التوعية والوقاية قبل التعرض للابتزاز الإلكتروني** — Awareness and Prevention Before Online Blackmail.
3. **التعامل مع الابتزاز أو العنف الإلكتروني بعد حدوثه** — Responding After Online Blackmail or Digital Violence Occurs.

### Output

- A generated chatbot response addressing the follower's question based on the retrieved context.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
| --- | --- |
| Dify | Build and manage the chatbot and its Chatflow. |
| Dify Knowledge Base | Store the three campaign awareness documents. |
| Knowledge Retrieval / RAG | Retrieve relevant passages for a question. |
| Gemini LLM | Generate a natural-language answer using the available context. |
| PDF and text documents | Provide the chatbot's awareness content. |
| GitHub | Document the project and showcase implementation evidence. |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:** Use a dedicated campaign knowledge base with RAG.

**Why:** To make responses more closely connected to the campaign's educational resources rather than depending solely on general model knowledge.

### Decision 2

**Decision:** Build a four-node Dify Chatflow: User Input, Knowledge Retrieval, LLM, and Answer.

**Why:** To keep the flow easy to understand, demonstrate, and troubleshoot while separating retrieval from response generation.

---

## 🧪 Testing

**Testing status:** The supplied screenshots confirm that the three knowledge documents are available in Dify and that the four workflow nodes are connected. They do **not** show chat test transcripts or measured answer quality. The following are suggested test cases, **not completed test results**.

### Test 1 — Prevention Question

**Input:**  
"كيف أحمي نفسي من الابتزاز الإلكتروني؟" (How can I protect myself from online blackmail?)

**Expected Result:**  
The chatbot retrieves relevant prevention material and returns clear, practical guidance without inventing unsupported details.

**Actual Result:**  
Not documented in the supplied evidence.

**Status:** ⏳ Not yet verified

---

### Test 2 — Response After an Incident

**Input:**  
"شخص يهددني بنشر صوري، ماذا أفعل؟" (Someone is threatening to share my photos. What should I do?)

**Expected Result:**  
The chatbot provides supportive, safety-conscious first steps consistent with the uploaded materials, avoids encouraging engagement with the threatening person, and acknowledges when professional support may be needed.

**Actual Result:**  
Not documented in the supplied evidence.

**Status:** ⏳ Not yet verified

---

## 📸 Evidence

### Final Result

<img width="1535" height="330" alt="image" src="https://github.com/user-attachments/assets/314db7e0-c259-4c3d-a4f9-a988a4623c2f" />
<img width="1530" height="717" alt="image" src="https://github.com/user-attachments/assets/da33dbd6-0963-4d32-b1dc-468c321b6850" />

<img width="1372" height="391" alt="image" src="https://github.com/user-attachments/assets/086024ca-37fa-4ce1-b201-60defd2a7853" />
<img width="1302" height="703" alt="image" src="https://github.com/user-attachments/assets/7267c0ee-cdae-4459-9a70-2d0169d9f1da" />
<img width="1397" height="710" alt="image" src="https://github.com/user-attachments/assets/e2a1c2d9-7bac-486a-a782-00f547ecb302" />





### Knowledge Base

**Screenshot provided:** Dify Knowledge Base listing three available awareness documents.

Suggested file path: `screenshots/knowledge-base.png`

> After uploading the screenshots to the actual project repository, replace these notes with relative links such as `![Workflow](screenshots/workflow.png)`. The URLs in the original template refer to a template repository and should not be presented as this project's evidence.

---

## ⚠️ Challenges & Failures

### What didn't work as expected?

The supplied material does not document a confirmed failure or debugging history for this chatbot. The following are **potential challenges to evaluate**, not claims about observed failures:

- Retrieval could return insufficient or only partially relevant passages.
- A generated response could omit important cautions or introduce unsupported advice.

### How would I address these challenges?

- Inspect the retrieved passages for representative follower questions.
- Review the knowledge documents and retrieval settings when context is missing.
- Refine assistant instructions to emphasize grounding, uncertainty, privacy, and safe responses.
- Retest both retrieval quality and final answers after each change.

---

## 📚 What I Learned

### Technical Lesson

Building a knowledge-based chatbot involves more than connecting an LLM. Document preparation, knowledge retrieval, and workflow configuration each influence the final response.

### Problem-Solving Lesson

Breaking the chatbot into separate retrieval and generation stages makes it easier to identify where an inaccurate or incomplete answer may originate.

### AI Lesson

RAG can provide relevant context, but it does not guarantee accurate answers. Safety-sensitive topics require deliberate instructions, realistic test cases, and human review.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- Create a structured evaluation dataset covering prevention, incident response, incomplete questions, out-of-scope requests, and safety scenarios.
- Improve grounding checks, uncertainty handling, and retrieval quality based on observed test results.
- Explore a clearly defined human-support referral process, with privacy safeguards and verified contact details.

---

## 🔗 Project Links

[Live Demo:](https://udify.app/chat/BKRF5rRdZXkcfTbU)



[Repository](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/edit/main/04-Grounding%20Thinking)

---

## 👩‍💻 Author

**[Your Name]**

[LinkedIn](https://www.linkedin.com/in/deema-radwan-a06a19370/) · [GitHub](https://github.com/deemaredwan-a11y)

---

*Project context: Shashti Amna digital safety awareness campaign, implemented by the Women's Legal Research, Counseling and Protection Center, with support from Heinrich Böll Stiftung (Palestine and Jordan).*
