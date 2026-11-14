
# RAG with n8n — Knowledge Pipeline & RAG AI Agent

> A two-part Retrieval-Augmented Generation (RAG) system built with n8n and Supabase: an automated knowledge pipeline that transforms PDF documents into searchable knowledge, and an AI agent that answers user questions using retrieved document content.

---

## 🎯 Problem

### The Context

Large Language Models (LLMs) do not automatically have access to the contents of specific documents, such as course guides, organizational policies, or technical manuals.

When users need accurate information from a particular document, the AI system must be able to access and retrieve relevant information from that source.

### The Problem

Manually copying documents into a chat is inefficient and does not scale.

Additionally, an AI model answering questions from its general knowledge may generate responses that sound convincing but are not supported by the original document.

This creates challenges related to:

- Information accuracy.
- Document accessibility.
- Knowledge retrieval.
- Answer reliability.
- Efficient document management.

### Why It Matters

A document-based question-answering system allows users to access relevant information without manually reading entire documents.

By connecting AI responses to retrieved knowledge, the system can improve answer relevance and reduce the risk of unsupported responses.

---

## 💡 Solution

I developed a **Retrieval-Augmented Generation (RAG)** system using **n8n, Supabase, OpenAI Embeddings, and Google Gemini**.

The solution consists of two independent but connected workflows:

**1. Knowledge Pipeline**

Automatically processes PDF documents from Google Drive, extracts their content, divides the text into chunks, generates embeddings, and stores the resulting knowledge in a Supabase vector database.

**2. RAG AI Agent**

Receives user questions through a chat interface, retrieves relevant information from the Supabase vector store, and generates answers using the Google Gemini language model.

### What Does the System Do?

1. Monitors a Google Drive folder for newly created files.
2. Extracts, chunks, embeds, and stores PDF content in Supabase.
3. Retrieves relevant document passages when the AI agent searches the knowledge base.
4. Generates contextual answers while maintaining conversation history.

---

## 👤 Users

**Primary Users:**

- Students and learners.
- Course participants.
- Researchers.
- Educators.
- Anyone who needs answers from specific documents.

**User Need:**

Users need a simple way to ask questions in natural language and receive answers based on relevant document content without manually searching through the entire PDF.

---

## 🧠 How I Designed the Solution

The main architectural decision was to separate document processing from question answering.

The system is divided into two stages:

### 1. Knowledge Preparation (Ingestion)

This process prepares documents before users ask questions.

It includes:

- Detecting a new file in Google Drive.
- Downloading the document.
- Extracting text from the PDF.
- Splitting the text into smaller chunks.
- Generating vector embeddings.
- Storing the processed content in Supabase.

### 2. Knowledge Retrieval and Answer Generation

This process runs when a user sends a question.

It includes:

- Receiving the user's message.
- Using an AI agent to interpret the question.
- Searching the Supabase vector database through a retrieval tool.
- Retrieving relevant document chunks.
- Generating an answer using Google Gemini.
- Maintaining conversational context using Simple Memory.

### Additional Design Considerations

**Knowledge and memory serve different purposes.**

The Supabase vector store contains the knowledge extracted from documents, while the Simple Memory node maintains conversational context.

**Embeddings are used in both workflows.**

Document chunks are embedded during ingestion, and questions are embedded during similarity search.

**The vector store is connected as an AI tool.**

The AI agent can decide when it needs to search the knowledge base instead of performing retrieval for every message automatically.

---

## ⚙️ How It Works

### Part 1: Knowledge Pipeline

```text
Google Drive Trigger
    |
    v
Edit Fields
(Extract File ID)
    |
    v
Google Drive
(Download File)
    |
    v
Extract from File
(Extract PDF Text)
    |
    v
Supabase Vector Store
(Insert Documents)
    |
    +-- Default Data Loader
    |       |
    |       +-- Recursive Character
    |           Text Splitter
    |
    +-- OpenAI Embeddings
    |
    v
Supabase Database
(documents table)
```

### Part 2: RAG AI Agent

```text
When Chat Message Received
            |
            v
         AI Agent
            |
            +-- Google Gemini
            |   Chat Model
            |
            +-- Simple Memory
            |
            +-- Supabase Vector Store
            |       |
            |       +-- OpenAI Embeddings
            |
            v
     Retrieved Knowledge
            |
            v
      Generated Answer
            |
            v
       Chat Response
```

### End-to-End Process

The knowledge pipeline prepares the PDF content and stores it in Supabase.

When a user asks a question, the AI agent can call the Supabase Vector Store tool to retrieve relevant information.

The Google Gemini model then uses the retrieved content to generate a response.

Conversation memory maintains the context of previous messages.

---

## 🤖 Role of AI

### What Does AI Do in This Solution?

**1. Semantic Embeddings**

OpenAI Embeddings converts document chunks into numerical vectors that represent semantic meaning.

These vectors enable similarity-based retrieval rather than relying only on exact keyword matching.

**2. Question Understanding**

The Google Gemini model interprets the user's natural-language question.

**3. Tool-Based Retrieval**

The AI agent can decide to use the Supabase Vector Store tool to search for relevant document content.

**4. Answer Generation**

The language model generates a response based on the information returned by the retrieval system.

**5. Conversational Context**

Simple Memory helps maintain context across messages in the same conversation.

### What AI Should NOT Do

- Invent information that is not supported by the retrieved material.
- Present unsupported information as a verified fact.
- Assume that the retrieved document contains information it does not provide.
- Confuse conversation history with the actual knowledge source.
- Claim to have verified information when no supporting content was retrieved.

The goal is to encourage grounded responses, although retrieval alone does not guarantee that every generated answer will be accurate.

---

## 🔄 Automation

### What Parts of the Process Are Automated?

**Document Processing**

- Detecting newly created files in Google Drive.
- Downloading PDF files.
- Extracting document text.
- Splitting text into chunks.
- Generating embeddings.
- Storing document chunks in Supabase.

**Question Answering**

- Receiving user messages.
- Calling the retrieval tool when selected by the agent.
- Performing vector similarity search.
- Retrieving relevant document passages.
- Generating AI responses.
- Maintaining conversational context.

### Automation Benefits

- Reduces manual document processing.
- Makes document content searchable.
- Simplifies information retrieval.
- Connects multiple services through n8n.
- Enables reusable document-based AI workflows.

---

## 🗂️ Data

### Input

The system receives two main types of input:

**1. Document Input**

A PDF uploaded to a monitored Google Drive folder.

**2. User Input**

Natural-language questions submitted through the chat interface.

### Knowledge Source

The project uses:

**The AI Automation Starter Guide (2026 Edition)**

This third-party PDF serves as the knowledge source for testing the RAG system.

The original PDF is not included in this repository.

### Stored Data

Document chunks and embeddings are stored in the Supabase `documents` table.

| Field | Description |
|---|---|
| `id` | Unique identifier for each stored record |
| `content` | Extracted text chunk |
| `metadata` | JSON containing document location information |
| `embedding` | Numerical vector representing semantic meaning |

### Output

The system produces:

- AI-generated answers.
- Responses based on retrieved document passages.
- Context-aware conversations.

### Knowledge Ingestion Result

During testing, the Supabase `documents` table contained:

**233 stored records**

The stored content included passages extracted from the course guide, such as its table of contents and chapters.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| n8n Cloud | Building and orchestrating the automation workflows |
| Google Drive | Hosting the PDF knowledge source and triggering ingestion |
| Extract from File | Extracting text from PDF documents |
| Recursive Character Text Splitter | Dividing document text into smaller chunks |
| OpenAI Embeddings | Generating embeddings for document chunks and questions |
| Supabase | Storing document chunks, metadata, and embeddings |
| PostgreSQL + pgvector | Supporting vector storage and similarity search |
| Google Gemini | Understanding questions and generating responses |
| n8n AI Agent | Coordinating the language model and retrieval tool |
| Simple Memory | Maintaining conversational context |
| GitHub | Hosting project documentation and screenshots |

---

## 🔑 Key Design Decisions

### Decision 1 — Separate Ingestion and Answering

**Decision:**

Create two independent n8n workflows: one for document ingestion and another for question answering.

**Why:**

Document processing and question answering are different operations.

Separating them makes the system easier to maintain, test, and extend.

It also avoids repeating document processing for every user question.

### Decision 2 — Use Supabase as a Vector Database

**Decision:**

Store document chunks and embeddings in Supabase.

**Why:**

Supabase provides PostgreSQL-based storage with vector search capabilities.

This allows the system to retrieve document passages based on semantic similarity.

### Decision 3 — Connect the Vector Store as an AI Tool

**Decision:**

Provide the Supabase Vector Store to the AI agent as a retrieval tool.

**Why:**

The agent can determine when document retrieval is needed.

This supports flexible, tool-based question answering.

### Decision 4 — Use the Same Embedding Approach

**Decision:**

Use OpenAI Embeddings for both document ingestion and question retrieval.

**Why:**

Documents and questions must be represented in a compatible vector space for similarity search.

Using the same embedding model and configuration helps ensure compatibility.

### Decision 5 — Separate Knowledge from Conversation Memory

**Decision:**

Use Supabase for document knowledge and Simple Memory for conversation history.

**Why:**

Document knowledge and conversational context have different responsibilities.

This separation makes the architecture clearer and helps prevent confusion between stored facts and previous messages.

---

## 🧪 Testing

### Test 1 — Knowledge Ingestion

**Input:**

Add the PDF document to the monitored Google Drive folder.

**Expected Result:**

The knowledge pipeline should:

1. Detect the new file.
2. Download the PDF.
3. Extract its text.
4. Split the content into chunks.
5. Generate embeddings.
6. Store the processed chunks in Supabase.

**Actual Result:**

The Supabase `documents` table contained **233 records**.

The stored `content` fields included passages extracted from the guide, including sections from its table of contents and chapters.

**Status:** ✅ Passed

---

### Test 2 — Knowledge Retrieval

**Input:**

Submit a question through the AI agent chat interface.

**Expected Result:**

The AI agent should call the Supabase Vector Store tool to retrieve relevant document chunks.

**Actual Result:**

The execution logs showed the AI agent using:

- Supabase Vector Store.
- OpenAI Embeddings.
- Google Gemini Chat Model.
- Simple Memory.

The retrieval tool was called during execution.

**Status:** ✅ Passed

---

### Test 3 — Course-Related Question

**Input:**

```text
Whats the name of Course ?
```

**Expected Result:**

The agent should respond based on the retrieved course material without inventing an official course title.

**Actual Result:**

The agent explained that the retrieved content focused on workflow automation using n8n.

It indicated that no official course title was explicitly identified in the retrieved sections.

The response summarized topics including:

- Introduction to automation.
- n8n core concepts and nodes.
- Building and managing workflows.
- AI in automation.

**Status:** ✅ Passed

---

## 📸 Evidence

### [Final Result](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/tree/main/07-Reliability%20%26%20Production%20Thinking/Screenshot)







---

## ⚠️ Challenges & Failures

### Challenge 1 — Verifying Document Ingestion

**Challenge:**

A successful workflow execution alone is not sufficient evidence that document content was stored correctly.

**Approach:**

Inspect the Supabase `documents` table and verify that stored records contain extracted text and embeddings.

**Evidence:**

The ingestion test confirmed 233 stored records containing passages from the source guide.

### Challenge 2 — Verifying AI Tool Usage

**Challenge:**

An AI agent may produce an answer without necessarily retrieving information from the intended knowledge source.

**Approach:**

Inspect the n8n execution logs to verify that the Supabase Vector Store tool was called.

**Evidence:**

Execution logs confirmed that the agent called the retrieval tool.

### Challenge 3 — Avoiding Unsupported Answers

**Challenge:**

The model may generate an answer even when the retrieved information does not explicitly contain the requested detail.

**Approach:**

Evaluate whether the response distinguishes retrieved information from unsupported assumptions.

**Evidence:**

In the course-name test, the agent did not invent an official title when the retrieved material did not explicitly provide one.

### Reliability Limitations

The completed tests verify ingestion, tool invocation, and one document-related response.

They do not yet establish comprehensive reliability under conditions such as:

- External API failures.
- Invalid or corrupted PDF files.
- Duplicate document uploads.
- Missing or irrelevant retrieval results.
- Large document processing failures.
- Multiple simultaneous requests.

These scenarios require additional testing before describing the system as production-ready.

---

## 📚 What I Learned

### Technical Lesson

I learned how to build a RAG architecture by combining several independent components:

- PDF text extraction.
- Document chunking.
- Vector embeddings.
- Vector database storage.
- Semantic retrieval.
- AI agent tool integration.
- Language model response generation.

I also learned how n8n can coordinate these components across multiple services.

### Problem-Solving Lesson

When an AI answer is incorrect or incomplete, the problem may originate from different stages of the system.

A useful troubleshooting approach is to investigate:

1. Was the document processed correctly?
2. Were the chunks stored successfully?
3. Did the retrieval system return relevant information?
4. Did the language model use the retrieved information appropriately?

This helps isolate problems rather than treating the entire RAG system as a single component.

### AI Lesson

RAG is not a single AI node.

It is an architecture that combines retrieval and generation.

The retrieval system identifies relevant knowledge, while the language model interprets and communicates that information.

Both components must be evaluated independently.

### Reliability Lesson

A successful execution does not automatically guarantee a correct result.

Reliable AI automation requires verification of intermediate steps, examination of execution logs, and testing of both expected and unexpected conditions.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

### 1. Expand the Knowledge Base

Support additional documents and organize them using useful metadata.

### 2. Improve Retrieval Quality

Experiment with different:

- Chunk sizes.
- Chunk overlap values.
- Similarity thresholds.
- Numbers of retrieved chunks.

Change one configuration at a time to evaluate its effect on retrieval quality.

### 3. Add Source Attribution

Store document names and source information in chunk metadata so generated answers can reference their original documents.

### 4. Improve Error Handling

Add dedicated error-handling paths for:

- Failed Google Drive downloads.
- PDF extraction errors.
- Embedding API failures.
- Supabase insertion errors.
- AI model errors.

### 5. Prevent Duplicate Ingestion

Introduce document identifiers or content hashes to detect files that have already been processed.

### 6. Improve Answer Reliability

Add response instructions and validation mechanisms that encourage the agent to acknowledge when the retrieved knowledge is insufficient.

### 7. Add Monitoring and Logging

Track workflow execution results, errors, processing times, and retrieval activity.

### 8. Expand Testing

Create additional test cases covering successful operations, invalid inputs, missing information, service failures, and repeated requests.

---


## 👩‍💻 Author

**[Dima Redwan ]**

AI Automation & AI Agent Development

[LinkedIn Profile](https://www.linkedin.com/in/deema-radwan-a06a19370/) · [GitHub Profile](https://github.com/deemaredwan-a11y)

---

## 📌 Project Summary

This project demonstrates how to build a document-based AI question-answering system using Retrieval-Augmented Generation.

By combining n8n automation, Google Drive, OpenAI Embeddings, Supabase, and Google Gemini, the system transforms a PDF into searchable knowledge and allows users to ask questions through an AI agent.

The completed tests demonstrate successful knowledge ingestion, retrieval-tool execution, and a grounded response to a course-related question.

The project also highlights the importance of separating knowledge preparation from question answering, verifying intermediate processing results, and evaluating AI behavior before deploying automated systems into production.
