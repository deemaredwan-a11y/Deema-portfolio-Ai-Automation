# TaskBridge

> **Turn Unclear Requests into Clear, Actionable Plans.**

TaskBridge is a responsive web application that helps freelancers, recent graduates, and early-career professionals transform unclear work requests and personal goals into structured, actionable plans with tasks, priorities, status tracking, and progress monitoring.

---

## 🎯 Problem

### The Context

Freelancers, recent graduates, and early-career professionals often receive work requests or set personal goals without having a clear process for achieving them.

They may understand the desired outcome but struggle to identify the required tasks, organize their responsibilities, and determine where to begin.

### The Problem

The main challenge is turning an unclear goal into an organized, actionable plan.

Users may struggle with:

- Defining a clear desired outcome.
- Breaking complex goals into manageable tasks.
- Identifying priorities.
- Tracking completed and pending activities.
- Knowing what to work on next.
- Monitoring overall progress.

Without a structured planning system, work can become difficult to organize and track.

### Why It Matters

Solving this problem helps users:

- Organize their work more effectively.
- Convert abstract goals into concrete actions.
- Identify important tasks.
- Track progress toward their objectives.
- Reduce confusion when starting new projects.
- Maintain visibility over unfinished work.

---

## 💡 Solution

TaskBridge provides a centralized workspace for managing personal work plans and their associated tasks.

The system follows a structured workflow:

**Goal → Plan → Tasks → Priorities → Status → Progress**

What does the system do?

1. **Plan Management:** Allows users to create, view, edit, and delete plans with titles, descriptions, goals, and optional deadlines.
2. **Task Management:** Enables users to organize tasks within each plan and assign priorities and execution statuses.
3. **Progress Tracking:** Calculates plan completion percentages based on completed tasks.
4. **Secure Data Management:** Stores user information in Supabase and uses authentication and database-level access control to protect individual records.

The first version focuses on reliable manual planning and task management rather than automatic AI-generated plans.

---

## 👤 Users

**Primary Users:**

- Freelancers managing client requests and personal projects.
- Recent graduates organizing professional goals.
- Early-career professionals planning their work.
- Individuals who need structured task and goal management.

**User Need:**

Users need a simple and secure system that helps them transform unclear objectives into actionable plans, organize tasks, assign priorities, and monitor progress over time.

---

## 🧠 How I Designed the Solution

I designed TaskBridge around the engineering principle:

**Build the System, Not the Screen.**

The project focuses on understanding and designing the underlying system rather than only generating a visually attractive interface.

The main design considerations include:

**1. Problem Definition**

Identifying the difficulties users face when organizing unclear goals and work requests.

**2. Requirements Analysis**

Defining essential application functions, including authentication, plan management, task management, progress tracking, and persistent data storage.

**3. Database Design**

Structuring the application around two primary tables:

- `plans`
- `plan_tasks`

Each plan belongs to a specific user, and each task belongs to a plan owned by that same user.

**4. Security by Design**

Using Supabase Authentication and Row Level Security (RLS) to control access to user data.

**5. Data Integrity**

Defining primary keys, foreign keys, valid task statuses, priority constraints, and ownership relationships.

**6. MVP Scope**

Prioritizing core application functionality before introducing advanced features such as AI-generated plans and external automation integrations.

**7. Testing and Validation**

Preparing functional, persistence, and security test cases to evaluate the application before deployment.

---

## ⚙️ How It Works

```text
User Input
   |
   v
User Authentication
(Supabase Auth)
   |
   v
Create a Plan
(Title, Description, Goal, Deadline)
   |
   v
Save Plan in Supabase Database
   |
   v
Add Tasks to the Plan
   |
   v
Assign Priorities
(High / Medium / Low)
   |
   v
Update Task Status
(To Do / In Progress / Done)
   |
   v
Application Logic
(Calculate Progress)
   |
   v
Progress Dashboard
   |
   v
Output:
Organized Plan + Tasks + Progress %
```

**Progress Calculation:**

```text
Progress = (Completed Tasks / Total Tasks) × 100
```

Example:

A plan contains five tasks, and two tasks are completed.

```text
Progress = (2 / 5) × 100 = 40%
```

If the plan has no tasks, the progress percentage is `0%`.

---

## 🤖 Role of AI

AI is used primarily during the application development process through Lovable, an AI-assisted application builder.

AI contributes to:

- Generating application interfaces based on defined requirements.
- Assisting with application structure and implementation.
- Supporting iterative development and refinement.
- Accelerating the translation of technical specifications into application components.

The project emphasizes human responsibility for defining requirements, reviewing generated implementations, and validating the resulting system.

### What AI Should NOT Do

- Automatically generate user plans or tasks in the current MVP.
- Execute user tasks independently.
- Replace database-level security controls.
- Make unauthorized changes to user information.
- Be considered proof that a feature works without actual testing.

**Important:** AI-powered plan generation is a proposed future enhancement, not an implemented MVP feature.

---

## 🔄 Automation

TaskBridge includes application-level operations designed to reduce repetitive manual work.

These include:

- Calculating progress percentages from task completion statuses.
- Updating progress when tasks are added, removed, or their statuses change.
- Saving and retrieving plans and tasks through the database.
- Associating records with authenticated users.
- Deleting related tasks when a plan is deleted through database cascading relationships.

These are application logic and database operations, not external workflow automation.

Integration with platforms such as Zapier or n8n is outside the current MVP scope.

---

## 🗂️ Data

### Input

The system accepts the following information:

**User Information:**

- Authentication credentials managed by Supabase Auth.
- Authenticated user identifier.

**Plan Information:**

- Plan title.
- Description.
- Goal or desired outcome.
- Optional deadline.
- Deliverables.
- Optional next step.

**Task Information:**

- Task title.
- Optional description.
- Priority: `High`, `Medium`, or `Low`.
- Status: `To Do`, `In Progress`, or `Done`.
- Task display order.
- Associated plan identifier.

### Output

The system provides:

- Saved user plans.
- Organized task lists.
- Task priorities.
- Current task execution statuses.
- Plan completion percentages.
- Updated plan information.
- A view of outstanding and completed work.

### Database Structure

**Table 1: `plans`**

Stores user plans, their goals, descriptions, deadlines, deliverables, and timestamps.

**Table 2: `plan_tasks`**

Stores tasks associated with individual plans, including priorities, statuses, and display order.

**Relationship:**

```text
User
 |
 +-- Plan 1
 |     |
 |     +-- Task 1
 |     +-- Task 2
 |     +-- Task 3
 |
 +-- Plan 2
       |
       +-- Task 4
       +-- Task 5
```

The database design specifies a composite foreign key to ensure that tasks belong to plans owned by the same user.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Lovable | AI-assisted web application development |
| Supabase Auth | User registration, login, and authentication |
| Supabase PostgreSQL | Persistent storage of plans and tasks |
| Supabase RLS | Database-level user access control |
| GitHub | Source code management and project documentation |

**Technical Concepts:**

- AI-assisted application development.
- Relational database design.
- Authentication and authorization.
- CRUD operations.
- Data persistence.
- Referential integrity.
- Row Level Security.
- Responsive web design.
- Functional and security testing.

---

## 🔑 Key Design Decisions

### Decision 1 — Relational Database Structure

**Decision:**

Use two primary tables, `plans` and `plan_tasks`, connected through a one-to-many relationship.

**Why:**

This structure supports organizing multiple tasks under a single plan while maintaining clear relationships and reducing unnecessary data duplication.

### Decision 2 — Database-Level Security

**Decision:**

Use Supabase Row Level Security rather than depending exclusively on frontend access restrictions.

**Why:**

Frontend restrictions alone cannot reliably protect user data from direct database requests.

RLS policies are intended to ensure that users can access only records they own.

### Decision 3 — Composite Foreign Key

**Decision:**

Connect `plan_tasks(plan_id, user_id)` to `plans(id, user_id)` using a composite foreign key.

**Why:**

This enforces consistency between task ownership and plan ownership at the database level.

It prevents a task from referencing a plan belonging to another user, provided the constraint is implemented correctly.

### Decision 4 — Calculated Progress

**Decision:**

Calculate progress using current task statuses instead of storing a separate progress value.

**Why:**

A dynamically calculated percentage reduces the risk of displaying outdated or inconsistent completion information.

### Decision 5 — Limited MVP Scope

**Decision:**

Build the core planning and task management system before adding AI plan generation or external workflow automation.

**Why:**

A clearly defined MVP allows the essential features, database relationships, and security requirements to be implemented and validated before increasing system complexity.

---

## 🧪 Testing

The following test cases are defined in the project requirements. Their actual results must be documented after execution.

### Test 1 — Progress Calculation

**Input:**

Create a plan containing four tasks and mark one task as `Done`.

**Expected Result:**

The application displays a completion percentage of `25%`.

**Actual Result:**

Pending verification.

**Status:** ⏳ Not yet documented as tested.

---

### Test 2 — Data Persistence

**Input:**

Create a plan, add tasks, refresh the page, then log out and log in again.

**Expected Result:**

The plan, tasks, and saved changes remain available to the authenticated user.

**Actual Result:**

Pending verification.

**Status:** ⏳ Not yet documented as tested.

---

### Test 3 — User Data Isolation

**Input:**

Create two user accounts, User A and User B.

Attempt to access, modify, or delete User B's records using User A's account.

**Expected Result:**

User A cannot read, modify, or delete User B's plans or tasks.

**Actual Result:**

Pending verification.

**Status:** ⏳ Not yet documented as tested.

---

### Test 4 — Invalid Task Status

**Input:**

Attempt to save a task with a status outside the allowed values:

- `To Do`
- `In Progress`
- `Done`

**Expected Result:**

The database rejects invalid status values.

**Actual Result:**

Pending verification.

**Status:** ⏳ Not yet documented as tested.

---

### Test 5 — Plan Deletion

**Input:**

Delete a plan containing multiple tasks.

**Expected Result:**

The plan is deleted, and its associated tasks are automatically removed through `ON DELETE CASCADE`.

**Actual Result:**

Pending verification.

**Status:** ⏳ Not yet documented as tested.

---

## 📸 Evidence

### Final Result

Add a screenshot of the completed TaskBridge application showing its main interface.
<img width="1473" height="667" alt="image" src="https://github.com/user-attachments/assets/e7406607-4d13-446c-aa2d-f5e857651e86" />
<img width="1482" height="662" alt="image" src="https://github.com/user-attachments/assets/f4e36e7e-dd8d-44c2-b641-88ee9c24314f" />
<img width="1466" height="678" alt="image" src="https://github.com/user-attachments/assets/8a493b76-71de-44cf-92a6-a35c1536642c" />




![Final Result](./screenshots/final-result.png)



### Database Evidence

Recommended supporting screenshots:

- Supabase database tables.
- Relationship between `plans` and `plan_tasks`.
- Row Level Security policies.
- Task management interface.
- Progress tracking interface.
- Successful test results.

Screenshots should be added after the relevant functionality has been verified.

---

## ⚠️ Challenges & Failures

### Challenge 1 — Maintaining Data Ownership

**Challenge:**

Designing a database structure that prevents tasks from being associated with plans owned by different users.

**Design Approach:**

Specify a composite foreign key involving both the plan identifier and user identifier, supported by RLS policies.

**Verification:**

The effectiveness of these constraints must be confirmed through database inspection and cross-user access tests.

### Challenge 2 — Accurate Progress Tracking

**Challenge:**

Ensuring progress percentages remain consistent as users modify their task lists.

**Design Approach:**

Calculate progress directly from task statuses rather than storing an independent percentage.

**Verification:**

Test task creation, deletion, completion, and reopening.

### Challenge 3 — Building Beyond the Interface

**Challenge:**

Ensuring the application is not limited to a working visual interface without reliable persistence, integrity, and security.

**Design Approach:**

Define database relationships, authentication requirements, access policies, and test cases as part of the system design.

**Verification:**

Review the actual Lovable and Supabase implementations.

**Note:** These are documented engineering challenges and proposed approaches. Specific implementation failures and confirmed fixes should be added when supported by development or testing evidence.

---

## 📚 What I Learned

### Technical Lesson

This project demonstrates the importance of designing the backend structure alongside the user interface.

Key technical learning areas include:

- Understanding one-to-many database relationships.
- Defining primary and foreign keys.
- Using database constraints to preserve data integrity.
- Separating authentication from authorization.
- Understanding the role of Row Level Security.
- Calculating progress from current system data.
- Designing for persistent storage.

### Problem-Solving Lesson

A useful application begins with a clearly defined problem, not with a list of tools.

Breaking the project into requirements, data structures, user workflows, and testable outcomes helps establish a more systematic development process.

### AI Lesson

AI-assisted development can accelerate application creation, but generated code and interfaces still require human review.

The central lesson is:

**AI can help build a system, but the developer remains responsible for understanding, directing, testing, and improving it.**

This reflects the project's engineering principle:

**Build the System, Not the Screen.**

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- **AI Plan Generation:** Allow users to describe a goal in natural language and receive a suggested plan.
- **AI Output Evaluation:** Evaluate generated plans for clarity, completeness, realism, and alignment with user goals.
- **Automation Integration:** Connect TaskBridge with external workflow automation tools to support additional processes.
- **Progressive Web App (PWA):** Explore a more app-like experience across devices.

These improvements would be considered after validating the usability and reliability of the MVP.

---

## 🔗 Project Links

**Live Demo:** [https://layout-capture-bot.lovable.app]


**Repository:** []

---

## 👩‍💻 Author

**[Deema Redwan]**

[https://www.linkedin.com/in/deema-radwan-a06a19370/] · [https://github.com/deemaredwan-a11y/]

---

**TaskBridge — Turn Unclear Requests into Clear, Actionable Plans.**

*Build the System, Not the Screen.*
