# StudentFlow — University Study & Task Manager

> A responsive, bilingual web application that helps university students organize academic subjects, manage assignments, track deadlines, and monitor their study progress in one place.

---

## 🎯 Problem

### The Context

University students often manage multiple academic subjects, assignments, projects, and deadlines simultaneously.

As academic responsibilities increase, keeping track of tasks, priorities, and upcoming deadlines becomes more challenging.

Students need a centralized system to organize their academic workload and understand their progress.

### The Problem

Students may struggle with:

- Managing tasks across multiple university subjects.
- Remembering assignment deadlines.
- Identifying high-priority academic responsibilities.
- Tracking completed and unfinished tasks.
- Organizing academic information in one place.
- Understanding their overall study progress.

### Why It Matters

A structured academic task management system helps students:

- Improve time management.
- Organize their academic responsibilities.
- Prioritize important assignments.
- Reduce missed deadlines.
- Monitor study progress.
- Manage tasks across different subjects.

---

## 💡 Solution

**StudentFlow** is a bilingual academic task management web application built using Lovable and Supabase.

It provides students with a centralized platform for organizing subjects, managing tasks, and tracking academic progress.

What does the system do?

1. **Subject Management:** Allows students to create, view, update, and delete university subjects.
2. **Task Management:** Enables students to create academic tasks, assign deadlines, set priorities, update statuses, and filter tasks.
3. **Progress Tracking:** Displays total tasks, completed tasks, remaining tasks, completion percentages, and upcoming deadlines.
4. **Secure Academic Workspace:** Uses Supabase Authentication and database-level access policies to manage user-owned data.

The application supports both **Arabic (RTL)** and **English (LTR)** interfaces.

---

## 👤 Users

**Primary Users:**

- University students.
- Students managing multiple academic subjects.
- Students who need to organize assignments and deadlines.
- Students looking for a simple academic productivity tool.

**User Need:**

Students need an accessible platform to organize academic subjects, track assignments, manage deadlines, prioritize responsibilities, and monitor their progress throughout their studies.

---

## 🧠 How I Designed the Solution

I approached StudentFlow as a complete application rather than a visual prototype.

The design process was based on clearly defined requirements, database relationships, user workflows, security considerations, and measurable acceptance criteria.

**1. Requirements Analysis**

Defined the core application functions, including authentication, subject management, task management, dashboard calculations, and bilingual support.

**2. MVP Definition**

Focused on essential academic organization features instead of adding unnecessary complexity.

**3. Database Design**

Designed two main relational database tables:

- `subjects`
- `tasks`

Each task can optionally be associated with a university subject.

**4. Security Design**

Specified Supabase Authentication and Row Level Security (RLS) to restrict access to user-owned data.

**5. Bilingual User Experience**

Required Arabic and English interfaces with appropriate RTL and LTR layout directions.

**6. Data Integrity**

Specified validation constraints for task titles, priorities, statuses, and relationships between tasks and subjects.

**7. Evaluation and Testing**

Defined acceptance criteria for application functionality, data persistence, language switching, and user data isolation.

---

## ⚙️ How It Works

```text
Student Input
     |
     v
User Authentication
(Supabase Auth)
     |
     v
Student Dashboard
     |
     v
Create / Manage Subjects
     |
     v
Create / Manage Tasks
     |
     v
Set Task Details
(Subject, Due Date, Priority, Status)
     |
     v
Supabase PostgreSQL
(Data Storage + RLS)
     |
     v
Application Logic
(Progress Calculation + Filtering)
     |
     v
Dashboard Updates
     |
     v
Output:
Organized Subjects
Academic Task List
Upcoming Deadlines
Progress Percentage
```

### Progress Calculation

The system calculates progress using:

`Progress = (Completed Tasks / Total Tasks) × 100`

For example:

If a student creates three tasks and marks one as `Done`:

`Progress = (1 / 3) × 100 ≈ 33.3%`

If there are no tasks, progress is `0%`.

Only tasks with the status `Done` count as completed.

---

## 🤖 Role of AI

AI was used during the development process through **Lovable**, an AI-assisted application builder.

AI assisted with:

- Generating the web application's interface and components.
- Translating requirements into application functionality.
- Implementing the subject and task management structure.
- Assisting with Supabase database integration and security configuration.
- Supporting bilingual interface development.
- Generating and checking parts of the application logic.

### What AI Should NOT Do

- Automatically generate study plans in the current MVP.
- Modify academic tasks without user actions.
- Replace authentication or database security.
- Expose private user data.
- Claim that unexecuted tests have passed.
- Generate false academic progress data.

**Important:** AI is used to assist with application development. StudentFlow does not currently include an AI-powered study planning feature.

---

## 🔄 Automation

StudentFlow includes built-in application logic that reduces repetitive manual calculations and organization.

The specified functionality includes:

- Automatically calculating task completion percentages.
- Updating dashboard metrics based on task data.
- Filtering tasks by subject and status.
- Displaying upcoming tasks based on due dates.
- Applying database validation rules.
- Enforcing user-level data access restrictions.
- Preserving tasks when a related subject is deleted by removing the subject association.

The project does not include external workflow automation using Zapier, n8n, or similar platforms.

---

## 🗂️ Data

### Input

**User Information:**

- User authentication credentials.
- Authenticated user identifier.

**Subject Information:**

- Subject name.
- Optional subject code.

**Task Information:**

- Task title.
- Optional description.
- Optional associated subject.
- Optional due date.
- Priority.
- Status.

**Allowed Priorities:**

- `High`
- `Medium`
- `Low`

**Allowed Statuses:**

- `To Do`
- `In Progress`
- `Done`

### Output

StudentFlow provides:

- A personalized academic dashboard.
- An organized list of university subjects.
- Academic tasks and assignments.
- Task priorities and statuses.
- Upcoming tasks and deadlines.
- Completed and remaining task counts.
- Academic progress percentages.
- Arabic and English user interfaces.

### Database Structure

**Table 1: `subjects`**

| Field | Purpose |
|---|---|
| `id` | Unique subject identifier |
| `user_id` | Subject owner |
| `name` | Subject name |
| `code` | Optional subject code |
| `created_at` | Creation timestamp |

**Table 2: `tasks`**

| Field | Purpose |
|---|---|
| `id` | Unique task identifier |
| `user_id` | Task owner |
| `subject_id` | Optional associated subject |
| `title` | Task title |
| `description` | Optional description |
| `due_date` | Optional deadline |
| `priority` | Task priority |
| `status` | Execution status |
| `created_at` | Creation timestamp |
| `updated_at` | Last update timestamp |

### Database Relationship

```text
User
 |
 +-- Subject A
 |      |
 |      +-- Task 1
 |      +-- Task 2
 |
 +-- Subject B
 |      |
 |      +-- Task 3
 |
 +-- Unassigned Tasks
        |
        +-- Task 4
```

Tasks may exist without an associated subject.

Deleting a subject must preserve its associated tasks and set their `subject_id` to `NULL`.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Lovable | AI-assisted full-stack web application development |
| Supabase Auth | User authentication and session management |
| Supabase PostgreSQL | Persistent storage of subjects and tasks |
| Supabase Row Level Security | Database-level access control and user data isolation |

**Technical Concepts:**

- AI-assisted development.
- Relational database design.
- Authentication and authorization.
- CRUD operations.
- Persistent data storage.
- Row Level Security.
- Database constraints.
- Responsive web design.
- Arabic RTL and English LTR support.
- Application testing and validation.

---

## 🔑 Key Design Decisions

### Decision 1 — Bilingual User Interface

**Decision:**

Build StudentFlow with Arabic and English support, including RTL and LTR layout directions.

**Why:**

Students using the application may prefer different languages. Supporting both improves accessibility and usability.

### Decision 2 — Secure Relational Database

**Decision:**

Use Supabase PostgreSQL with separate `subjects` and `tasks` tables and Row Level Security.

**Why:**

This supports structured academic data management while enforcing ownership-based access control.

### Decision 3 — Optional Subject Association

**Decision:**

Allow tasks to exist without an associated subject.

**Why:**

Students may need to manage general academic tasks that are not related to a particular university subject.

### Decision 4 — Preserve Tasks After Subject Deletion

**Decision:**

When a subject is deleted, retain its tasks and remove their subject association.

**Why:**

Deleting an academic subject should not automatically erase potentially important tasks.

### Decision 5 — Dynamic Dashboard Calculations

**Decision:**

Calculate dashboard metrics from stored task data.

**Why:**

This avoids hardcoded statistics and supports accurate progress tracking.

### Decision 6 — Focused MVP

**Decision:**

Exclude advanced features such as automatic AI study plans, notifications, calendar integrations, and collaboration.

**Why:**

This keeps the first version focused on reliable academic task management.

---

## 🧪 Testing

The following results are based on the development report provided by Lovable.

### Test 1 — Application Build

**Input:**

Build the StudentFlow application with authentication pages, dashboard, subject management, task management, settings, and bilingual interface.

**Expected Result:**

The application compiles without errors.

**Actual Result:**

Lovable reported that the application compiled successfully without errors.

**Status:** ✅ Reported as passed by Lovable.

---

### Test 2 — Progress Calculation

**Input:**

Evaluate the progress calculation with zero tasks and with tasks marked as `Done`.

**Expected Result:**

- Zero tasks produce `0%` progress.
- Only `Done` tasks count as completed.

**Actual Result:**

Lovable reported that both progress calculation tests passed.

**Status:** ✅ Reported as passed by Lovable.

---

### Test 3 — Arabic Interface

**Input:**

Switch the application's home page from English to Arabic.

**Expected Result:**

The interface displays Arabic text and changes its layout direction to RTL.

**Actual Result:**

Lovable reported that the home page switched to Arabic and RTL correctly.

**Status:** ✅ Reported as passed by Lovable.

---

### Test 4 — Protected Dashboard

**Input:**

Attempt to open the dashboard without being authenticated.

**Expected Result:**

The unauthenticated user is redirected to the login page.

**Actual Result:**

Lovable reported that signed-out visitors were redirected to the login page.

**Status:** ✅ Reported as passed by Lovable.

---

### Test 5 — Authentication and Data Persistence

**Input:**

Register a user, log in, create subjects and tasks, refresh the page, log out, and log back in.

**Expected Result:**

The user can authenticate, and saved academic data remains available after refreshing and logging in again.

**Actual Result:**

Not verified. Lovable reported that it could not perform an authenticated sign-in test.

**Status:** ⏳ Pending manual testing.

---

### Test 6 — User Data Isolation

**Input:**

Create two user accounts and attempt to access User A's academic records from User B's account.

**Expected Result:**

Users cannot view, update, or delete academic records belonging to other users.

**Actual Result:**

Not verified through an actual two-user test.

**Status:** ⏳ Pending manual testing.

---

### Test 7 — Subject Deletion

**Input:**

Delete a subject containing associated tasks.

**Expected Result:**

The subject is deleted, while its tasks remain available with no associated subject.

**Actual Result:**

Lovable described this database behavior as implemented, but no completed manual deletion test was documented.

**Status:** ⏳ Pending manual testing.

---

## 📸 Evidence

[Final Result](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/tree/main/03-Evaluation%20%26%20Diagnostic%20Thinking/Independent-Project/Screenshots)



### Additional Evidence

Recommended screenshots include:

- Login and registration screens.
- Dashboard.
- My Tasks page.
- My Subjects page.
- Arabic RTL interface.
- English LTR interface.
- Supabase database tables.
- Row Level Security policies.

**Evidence Status:** Screenshots have not yet been included in this documentation.

---

## ⚠️ Challenges & Failures

### Challenge 1 — Limited Authentication Verification

**What didn't work as expected?**

Lovable was unable to sign in to the connected Supabase project during its verification process.

As a result, it could not confirm real user registration, data persistence, or access isolation using two authenticated accounts.

**How did I solve it?**

The implementation report identified the remaining configuration checks and manual tests:

- Verify that the Email authentication provider is enabled.
- Configure the necessary authentication redirect URLs.
- Test user registration and login.
- Verify data persistence.
- Test data isolation using two separate accounts.

**Resolution Status:** Manual verification remains pending.

### Challenge 2 — Validating Database Security

**Challenge:**

Ensuring that users cannot associate tasks with subjects belonging to another account.

**Implementation Approach:**

Lovable reported implementing database-level validation and Row Level Security policies.

**Resolution Status:**

The implementation was reported, but cross-user security behavior has not yet been independently tested.

### Challenge 3 — Deployment and Repository Verification

**Challenge:**

The application development report did not confirm a connected GitHub repository or published deployment.

**Next Steps:**

Verify repository integration and publish the application before adding public links.

**Resolution Status:** Not confirmed in the provided report.

---

## 📚 What I Learned

### Technical Lesson

StudentFlow demonstrates the importance of building applications with both functional interfaces and structured backend systems.

The project covers:

- Supabase authentication.
- Relational database modeling.
- CRUD operations.
- Data integrity constraints.
- Row Level Security.
- Dashboard calculations.
- Responsive user interfaces.
- Multilingual interface requirements.

### Problem-Solving Lesson

Effective application development begins with clear requirements and acceptance criteria.

Defining how tasks, subjects, users, and progress calculations should work makes the implementation more structured and easier to evaluate.

Testing must include actual user interactions rather than relying only on successful code compilation.

### AI Lesson

AI-assisted application builders such as Lovable can accelerate development, but they do not eliminate the need for engineering judgment and verification.

The key lesson is:

**AI-generated functionality should be evaluated against explicit requirements and real test results before being considered complete.**

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- **AI Study Plan Generation:** Generate suggested study plans based on subjects, deadlines, and academic goals.
- **Notifications:** Add deadline reminders through email or push notifications.
- **Calendar Integration:** Connect academic tasks with calendar services.
- **Advanced Analytics:** Provide deeper insights into task completion and academic progress.
- **Study Collaboration:** Explore optional task sharing for student groups.
- **Mobile Experience:** Further improve the experience for smaller screens and consider a dedicated mobile application.

These features are outside the current MVP.

---

## 🔗 Project Links

[Live Demo:](https://academi-hub-arabic.lovable.app)


[Repository:](https://github.com/deemaredwan-a11y/Deema-portfolio-Ai-Automation/edit/main/03-Evaluation%20%26%20Diagnostic%20Thinking)

---

## 👩‍💻 Author

**[Deema Redwan]**

[linkedin](https://www.linkedin.com/in/deema-radwan-a06a19370/)
[github](https://github.com/deemaredwan-a11y)

---

**StudentFlow — University Study & Task Manager**

*Organize Your Subjects. Manage Your Tasks. Track Your Progress.*
