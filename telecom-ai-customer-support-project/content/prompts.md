# Prompts Documentation — Telecom AI Support

This file documents the main prompts that had a meaningful impact on building the **Telecom AI Support** solution and producing its final content.

Only the prompts that influenced the system behavior, RAG workflow, escalation logic, and content creation are included.

---

## 1. AI Customer Support Agent Prompt

### Purpose

This prompt defines how the AI assistant should answer customer questions using only the approved telecom knowledge base.

### Prompt

```text
You are an AI customer-support assistant for a telecommunications company.

Your job is to answer customer questions using ONLY the information provided in the approved knowledge-base context.

Rules:
1. Do not invent package details, prices, speeds, validity periods, or service information.
2. If the answer exists in the supplied knowledge-base context, answer clearly and briefly.
3. Use a friendly and direct tone suitable for telecom customers.
4. If the answer is not supported by the provided context, return exactly:

NEEDS_HUMAN

5. Do not guess or use outside knowledge.
6. When possible, mention the relevant package name, price, allowance, speed, or validity period exactly as stated in the context.

Customer question:
{{question}}

Conversation history:
{{history}}

Knowledge-base context:
{{context}}
```

### Why It Was Important

This prompt is the core behavior prompt for the AI assistant.
It prevents hallucinated telecom information and ensures that unsupported questions are routed to human support.

---

## 2. RAG Context Prompt

### Purpose

This prompt combines the customer question, recent conversation history, and retrieved knowledge-base chunks before sending them to the AI model.

### Prompt

```text
Answer the customer's latest question using the retrieved telecom knowledge below.

Customer question:
{{question}}

Recent conversation:
{{history}}

Retrieved knowledge:
{{retrieved_documents}}

Instructions:
- Use the retrieved knowledge as the only factual source.
- Prefer the most directly relevant document.
- Keep the answer concise and easy to understand.
- Preserve package names, numbers, prices, speeds, and validity periods exactly.
- If the retrieved knowledge does not contain enough information to answer safely, return:

NEEDS_HUMAN
```

### Why It Was Important

This prompt connected the RAG retrieval step with the final model response.
It ensured that the model used the retrieved Supabase/pgvector results instead of answering from general model knowledge.

---

## 3. Human Escalation Prompt / Rule

### Purpose

This instruction defines when the workflow should stop automated answering and create a support ticket.

### Prompt

```text
If the customer request cannot be answered using the approved knowledge base, do not provide a speculative answer.

Return exactly:

NEEDS_HUMAN

The workflow will use this value to:
- create a support ticket,
- send an escalation email to the support team,
- notify the customer that a human agent will follow up.
```

### Why It Was Important

The exact keyword made the AI response machine-readable by n8n.
It allowed the workflow to branch reliably into the human-support escalation path.

---

## 4. Carousel Content Generation Prompt

### Purpose

This prompt was used to structure the social-media carousel around mobile internet, home internet, and AI support.

### Prompt

```text
Create a 5-slide Instagram carousel for a telecom AI support service.

The goal is to introduce internet packages and show that customers can ask an AI assistant for package and service information through Telegram.

Use short, direct, easy-to-read Arabic copy.

Slides:

1. Cover
Headline:
"إنترنت يناسب يومك… وين ما كنت!"

2. Mobile Internet
Headline:
"باقات تناسب استخدامك"
Example offer:
M15 — 15 GB for 45 NIS.

3. Home Internet
Headline:
"اتصال منزلي لكل احتياجاتك"
Example offer:
H50 — download speed up to 50 Mbps for 100 NIS per month.

4. AI Support
Headline:
"استفسارك؟ جوابك أقرب مما تتخيل!"
Explain that users can ask the telecom AI assistant about packages and services.

5. CTA
Headline:
"جاهز تختار باقتك؟"
Encourage the user to contact Telecom AI Support through Telegram.

Keep the tone friendly, modern, simple, and suitable for the Palestinian telecom market.
```

### Why It Was Important

This prompt established the final information hierarchy of the carousel and kept each slide focused on one message.

---

## 5. Visual Direction Prompt

### Purpose

This prompt guided the visual generation of the carousel so that all slides followed a consistent telecom identity.

### Prompt

```text
Create a modern telecom advertising visual for an Instagram carousel.

Visual identity:
- Vibrant green: #76BC21
- Blue gradients
- Turquoise accents
- White
- Light gray: #F5F5F7

Style:
- Modern telecom advertising
- Clean blue-to-turquoise gradient backgrounds
- Bright green highlights for offers and important numbers
- 3D smartphones and digital elements
- Wi-Fi, Telegram, internet, and connectivity icons
- Subtle neon lighting
- Rounded cards
- Strong visual hierarchy
- Large package numbers, prices, and speeds
- Friendly, modern, technology-focused mood

Do not copy the reference image directly.
Use it only as inspiration for composition, depth, lighting, and visual energy.
Create an original composition that matches the Telecom AI Support brand.
```

### Why It Was Important

The prompt helped maintain visual consistency across the carousel while using reference images only as inspiration instead of copying them.

---

# Prompt Improvement

## Example — Improving the RAG Answering Prompt

### 1. Before

```text
Answer the customer's question based on the provided telecom information.

Question:
{{question}}

Context:
{{context}}
```

### 2. Issue

The original prompt was too general.

It did not explicitly prevent the model from using its own general knowledge when the retrieved context was incomplete.

As a result, there was a risk that the assistant could:
- guess missing package information,
- provide unsupported prices or service details,
- answer questions that should have been escalated to a human agent.

The prompt also did not provide a reliable machine-readable signal for the n8n workflow when human support was required.

### 3. After

```text
You are an AI customer-support assistant for a telecommunications company.

Answer the customer's question using ONLY the approved knowledge-base context provided below.

Rules:
1. Never invent or guess package information.
2. Do not use outside knowledge.
3. Preserve package names, prices, data allowances, speeds, and validity periods exactly as they appear in the context.
4. If the context contains enough information, answer clearly and briefly.
5. If the context does not contain enough information, return exactly:

NEEDS_HUMAN

Customer question:
{{question}}

Conversation history:
{{history}}

Approved knowledge-base context:
{{context}}
```

### 4. Impact

The improved prompt produced a clear behavioral difference in testing.

**Before improvement:**
The model could attempt to answer even when the retrieved information was incomplete because there was no strict fallback rule.

**After improvement:**
The assistant successfully answered supported questions from the knowledge base, such as the M15 package with:

- 15 GB
- 45 NIS
- 30-day validity

For unsupported questions, the model returned:

```text
NEEDS_HUMAN
```

This value successfully triggered the n8n escalation branch, which created a support ticket, sent the escalation email, and notified the customer that human support would follow up.

Therefore, the improvement did not only make the response safer; it also made the AI output compatible with the automation workflow.

---

## Summary

The most important prompt-design decisions in this project were:

- grounding all telecom answers in the approved knowledge base,
- preventing unsupported answers and hallucinations,
- using `NEEDS_HUMAN` as a deterministic escalation signal,
- including conversation history in the RAG prompt,
- structuring the carousel around one clear message per slide,
- and defining a consistent visual direction for AI-generated marketing content.
