# Testing Evidence & Validation Report

**Project:** Telecom AI Customer Support — n8n + RAG + Supabase  
**Repository:** [Deema Portfolio / Telecom AI Customer Support](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/tree/main/telecom-ai-customer-support-project)  
**Workflows:** `01-knowledge-base.json`, `02-customer-support.json`, `03-error-handling.json`  
**Test period:** November 2–4, 2026  
**Document status:** Evidence-based record; screenshots and execution links to be added before final submission.

> **Scope of evidence.** This report records outcomes observed during the guided build and testing sessions. A successful observed scenario does not establish production reliability, and the limitations below remain open unless separately retested and evidenced.

## 1. Purpose and acceptance criteria

Validate that the solution can: (1) load approved telecom knowledge into Supabase; (2) answer grounded customer questions through Telegram; (3) use conversation history for follow-up questions; (4) escalate unsupported or personal-account requests to a support employee through Gmail; (5) persist messages, tickets, and interaction logs; and (6) notify an administrator if a production workflow fails.

**Test environment:** n8n Cloud; Telegram bot; Supabase PostgreSQL + pgvector; OpenAI embeddings and chat model; Gmail; Google Sheets. The "Wasla Telecom" information pack is **scenario/test content**, not verified operational pricing from a real provider.

## 2. Evidence index

Save redacted screenshots under `assets/screenshots/` in this repository. Replace each `ADD_LINK` with a **relative Markdown image path or a verified execution link**. Redact Telegram user IDs, names, private emails, credentials, tokens, and other customer details before publishing.

| Evidence ID | What to capture | Placeholder |
|---|---|---|
| EV-01 | Workflow 01 successful insertion (26 items) | <img width="1033" height="612" alt="image" src="https://github.com/user-attachments/assets/61794195-f93a-49ab-ad1d-33646a2926c2" />|
| EV-02 | `documents` table with populated content/embedding |<img width="982" height="673" alt="image" src="https://github.com/user-attachments/assets/8c144284-471d-4e29-bc78-0a908dc60642" />|
| EV-03 | Telegram answer for M15 | <img width="576" height="242" alt="image" src="https://github.com/user-attachments/assets/cf01221e-c3b9-49ec-a41d-a13ba5ac333c" />|
| EV-04 | Telegram follow-up about automatic renewal | <img width="565" height="135" alt="image" src="https://github.com/user-attachments/assets/79f990dd-877c-44e7-8df4-cceb3ceab930" />|
| EV-05 | `messages` records for customer and assistant |<img width="1221" height="446" alt="image" src="https://github.com/user-attachments/assets/b01335b9-d119-42d7-817b-521e152c98da" />|
| EV-06 | Gmail human escalation HTML | <img width="558" height="146" alt="image" src="https://github.com/user-attachments/assets/b2489ae9-96d6-41d8-bb27-68af0b013e2c" />

<img width="756" height="1046" alt="image" src="https://github.com/user-attachments/assets/31ef3c63-1fd5-4848-a6e4-899972a3f687" />
|
| EV-07 | `support_tickets` row with `pending` and non-null `email_sent_at` |<img width="1221" height="241" alt="image" src="https://github.com/user-attachments/assets/7d0f8f30-661a-44d8-8dfb-b347f9beb985" />
 |
| EV-08 | M30 response after an escalation | `ADD_LINK: assets/screenshots/ev-08-answer-after-escalation.png` |
| EV-09 | Google Sheets answered/escalated rows | `ADD_LINK: assets/screenshots/ev-09-google-sheets.png` |
| EV-10 | Intentional Stop and Error failed production execution | `ADD_LINK: assets/screenshots/ev-10-intentional-error.png` |
| EV-11 | Administrator Gmail failure alert | `ADD_LINK: assets/screenshots/ev-11-error-email.png` |
| EV-12 | Full Workflow 02 canvas | `ADD_LINK: assets/screenshots/ev-12-workflow-02.png` |

## 3. Executed test cases

**Status legend:** **Observed pass** = output was shared and verified in the project walkthrough; **Partially evidenced** = some expected effects were observed, but not all; **Not fully verified** = additional evidence or controlled test required.

| ID | Scenario / input | Expected result | Observed result | Status | Evidence |
|---|---|---|---|---|---|
| T01 | Upload `Wasla_Telecom_Knowledge_AR.txt` using n8n Form | Chunks and embeddings stored in `documents` | n8n insert returned **26 items**; Supabase table showed **26 records** with content and embedding | **Observed pass** | EV-01, EV-02 |
| T02 | “كم سعر باقة M15 وكم مدة صلاحيتها؟” | Grounded answer with M15 price, size, validity | 45 ILS, 15 GB; 30 days or until exhausted | **Observed pass** | EV-03 |
| T03 | Ask about automatic renewal after asking about M15 | Resolve follow-up reference and answer using approved knowledge | Bot answered that mobile bundles do not automatically renew in this scenario | **Observed pass (tested context)** | EV-04 |
| T04 | “كم رصيدي الحالي في خط الجوال؟” | Do not fabricate private account balance; trigger `NEEDS_HUMAN` | Model returned `NEEDS_HUMAN` after prompt refinement; escalation path tested | **Observed pass** | EV-06 |
| T05 | Customer + assistant messages processed | Save both sides in `messages` with same conversation ID | `messages` table screenshot showed customer entries and an `assistant` entry | **Observed pass** | EV-05 |
| T06 | Request employee help | Save support ticket, email the support address, acknowledge through Telegram | Ticket row shown with `pending`, matching message ID, non-null `email_sent_at`; Telegram confirmation observed | **Observed pass** | EV-06, EV-07 |
| T07 | “كم سعر باقة M30؟” after an unresolved support request | Answer independently of open ticket | Bot answered 75 ILS for 30 GB with applicable validity | **Observed pass** | EV-08 |
| T08 | Normal response and escalation | Log each tested interaction to Google Sheets | Spreadsheet append success and rows observed in earlier tests | **Observed pass for tested cases** | EV-09 |
| T09 | Trigger intentional production error via test Webhook → Stop and Error | Workflow 03 catches failure and sends Gmail alert | Failed test execution and formatted Gmail error alert both observed | **Observed pass** | EV-10, EV-11 |
| T10 | Same Telegram update is delivered/retried multiple times | Avoid duplicate messages/tickets/emails and recover safely | Database constraints and a ticket-existence check are configured; no controlled concurrent/retry matrix evidenced | **Not fully verified** | `ADD_LINK` |
| T11 | Gmail fails after support ticket is saved | Preserve ticket; handle retry without duplicate email or lost escalation | No verified full recovery path for pending ticket in supplied workflow export | **Not fully verified** | `ADD_LINK` |
| T12 | Escalation on long conversation (more than history window) | Employee gets complete, safely redacted conversation | Gmail template uses short `history`; complete-history escalation not verified | **Not fully verified** | `ADD_LINK` |
| T13 | Upload PDF document | Extract PDF text into knowledge base | Default Data Loader configured as text loader; PDF extraction not evidenced | **Not fully verified** | `ADD_LINK` |
| T14 | Brand-new Telegram chat after reset | Create conversation, save messages, answer without `undefined` | New-conversation branch exists; exported mappings warrant a separate fresh-user test | **Not fully verified** | `ADD_LINK` |

## 4. Representative observed outcomes

### A. Knowledge ingestion and RAG retrieval

- **Input:** Approved scenario `.txt` knowledge pack through an n8n Form Trigger.
- **Processing:** Default Data Loader → Recursive Character Text Splitter → OpenAI Embeddings → Supabase Vector Store.
- **Observed:** 26 document chunks inserted in `public.documents`.
- **Retrieval check:** FAQ-007 was returned for M15; the relevant document gives **15 GB for 45 ILS**, lasting **30 days or until data is exhausted**, whichever comes first.
- **Screenshot:** `ADD_LINK: EV-01 / EV-02`

### B. Grounded answer and conversation history

- **Direct question:** “كم سعر باقة M15 وكم مدة صلاحيتها؟”
- **Observed answer:** “45 شيكلًا ... 15 جيجابايت ... 30 يومًا ... أو عند نفاد البيانات”.
- **Follow-up:** “طيب هل يتم تجديدها بشكل تلقائي بعد الانتهاء؟”
- **Observed answer:** The scenario does not automatically renew mobile bundles.
- **Fix applied:** Search preparation was changed to include previous-question context so RAG could retrieve a more relevant policy passage.
- **Screenshot:** `ADD_LINK: EV-03 / EV-04`

### C. Human escalation without blocking future questions

- **Trigger:** Personal-balance question or explicit request to speak with staff.
- **Behavior:** AI returns `NEEDS_HUMAN` → ticket lookup → create a new ticket if absent → send formatted Gmail → update `email_sent_at` → notify customer via Telegram → append row in Google Sheets.
- **Observed:** `public.support_tickets` showed a `pending` ticket with a non-null mail-sent timestamp. After escalation, a fresh M30 pricing question received a correct automated answer.
- **Important boundary:** `email_sent_at` indicates the Gmail send node completed; it does **not** prove the employee read or resolved the request.
- **Screenshot:** `ADD_LINK: EV-06 / EV-07 / EV-08`

### D. Error monitoring

- **Test-only workflow:** Production Webhook (GET) → Stop and Error.
- **Injected error:** `TEST_ERROR: Intentional failure to verify Gmail alert`.
- **Observed:** n8n recorded a failed production execution; Workflow 03 sent a red HTML email including workflow name, failing node, error message, time, and execution link.
- **Cleanup required:** Ensure `TEST — Error Handling` is **unpublished/deactivated** after test. Do not leave an unauthenticated public error-generating Webhook running.
- **Screenshot:** `ADD_LINK: EV-10 / EV-11`

## 5. Problems discovered and corrections

| Issue observed | Cause / evidence | Correction | Remaining caution |
|---|---|---|---|
| Telegram `Bad Request: chat not found` | Send Message could not reach targeted chat | Checked bot credentials and tested a new live Telegram chat | Retest on fresh user/chat after import or credential change |
| Model replied with explanatory text plus `NEEDS_HUMAN` | Prompt output not constrained enough | Strengthened instruction to emit only `NEEDS_HUMAN` for escalation | Exact string matching is still fragile if model output changes |
| Follow-up question incorrectly escalated | RAG queried short ambiguous question without preceding topic | Added contextual `search_query` preparation in existing JavaScript node | Test multi-topic conversations; heuristic may select wrong earlier question |
| `Check Conversation Status` routed incorrectly / type mismatch | `status` missing and later numeric ID compared as a string | Fixed input mapping and ID-condition type | Retest both brand-new and existing conversations |
| Supabase field dropdown unavailable | n8n couldn't fetch column choices temporarily | Reconnected/reloaded and used supported field mappings | Confirm connection and schema when importing |
| Repeated test messages / records | Multiple manual test submissions | Cleaned test data before final testing | Use fresh message IDs and maintain evidence elsewhere |

## 6. Known limitations and production-readiness checklist

- [ ] **Idempotency and retries:** Test repeated message delivery, simultaneous messages, pending tickets, Gmail failure, and `email_sent_at` update failure. The current existing-ticket branch does not fully recover incomplete deliveries.
- [ ] **Complete human handoff:** Gmail currently uses the short conversation history rather than a separate full-history query. Redact sensitive content before emailing transcripts.
- [ ] **New conversation branch:** Verify `New Conversation` and `Prepare Conversation` correctly set `status` and avoid references to nodes from an unexecuted branch.
- [ ] **Input validation:** Telegram non-text messages, empty input, malformed file uploads, file type and file size checks.
- [ ] **Knowledge quality:** Test absent, contradictory, outdated, and adversarial content. The corpus is scenario data and must be replaced or approved before any real customer rollout.
- [ ] **Error handling:** Verify real failures of API integrations, and make sure a failed Google Sheets append or Gmail send is visible rather than silently treated as success.
- [ ] **Privacy and credentials:** Ensure service credentials stay in n8n, review RLS and least privilege, data retention/access policies, support-email recipients, and redaction in public evidence.
- [ ] **Operational configuration:** Confirm Workflow 01/02/03 published as intended and `02 → Error Workflow = 03` after final edits.
- [ ] **PDF support:** Validate PDF extraction separately; text input test alone is not proof of PDF ingestion.

## 7. Final acceptance record (fill after last verification)

| Item | Value |
|---|---|
| Tester | `ADD_NAME_OR_TEAM` |
| Test completion date | `ADD_DATE` |
| n8n version / deployment | `ADD_VERSION` |
| Verified commit or workflow export version | `ADD_COMMIT_HASH` |
| Total cases passed / failed / pending | `ADD_COUNTS` |
| Link to demo recording | `ADD_DEMO_URL` |
| Link to live project / portfolio | `ADD_PORTFOLIO_URL` |
| Final reviewer / sign-off | `ADD_REVIEWER` |

## 8. Evidence formatting example

Replace placeholder entries above with actual Markdown references once **redacted** screenshots are uploaded:

```markdown
![M15 answer shown in Telegram](../assets/screenshots/ev-03-m15-answer.png)

[View recorded n8n execution](ADD_VERIFIED_EXECUTION_LINK)
```

**Reminder:** Do not publish Telegram chat IDs, personal email addresses, customer names, API credentials, full private transcripts, or unredacted project URLs that grant access. Keep detailed/raw test records in a private evidence location when necessary.
