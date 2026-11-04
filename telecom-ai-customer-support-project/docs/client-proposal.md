# Client Proposal

## 1. Problem Statement

This project aims to address delays in responding to recurring customer inquiries about plans, services, billing, and connectivity issues. Relying on manual responses to every inquiry increases the workload of the support team and limits how quickly customers can obtain information, especially outside working hours.

## 2. Target Users

The solution is intended for current and prospective company customers, customer support representatives, and staff responsible for updating service information and monitoring support quality.

## 3. Current Challenges

The main challenges include repetitive questions, growing support queues, scattered service information, and difficulty maintaining consistent answers. When an inquiry is escalated to a support representative, the representative also needs access to the conversation context so that customers do not have to explain their issue again.

## 4. Proposed Solution

I propose developing a conversational assistant using **n8n Cloud** and **Retrieval-Augmented Generation (RAG)**. RAG allows the assistant to search approved company documents before composing a response.

The solution will initially use one communication channel—either the company website or WhatsApp—to be selected before implementation.

The assistant will answer general service inquiries and escalate cases requiring human assistance, including the conversation history. Requests involving account-specific information or actions that require authorization will be referred to the appropriate team.

## 5. Key Features

- Provide answers in Arabic based on an approved, updateable knowledge base.
- Clearly indicate when reliable information is unavailable, without guessing.
- Maintain a separate short-term conversation context for each customer.
- Escalate inquiries to a support representative upon customer request or when an issue cannot be resolved, including the conversation history.
- Log inquiries and essential follow-up information while respecting customer privacy.
- Handle connection failures and send alerts when the service encounters an error.

## 6. Expected Value

The solution is expected to reduce customer waiting times and the effort spent answering repetitive questions, improve information consistency, and allow support representatives to focus more on complex cases.

These benefits will be evaluated after implementation by measuring answer accuracy, response time, and the success of escalations to human support.
