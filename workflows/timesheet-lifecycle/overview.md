# Workflow: Timesheet & Daily Lifecycle

This document describes the standard "Day in the Life" workflow of an employee. It covers the core sequence of events from morning punch-in to evening reporting.

---

## 📈 The Daily Journey

### 1. Authentication (9:00 AM)
- **Action:** The employee logs into the portal using their centralized credentials.
- **Goal:** Establish a secure session and load the user's specific role-based dashboard.
- **Reference:** `login.aspx`

### 2. Marking Presence (Punch-In)
- **Action:** Before starting work, the employee navigates to the **Punch Lines** section.
- **Validation:** The system captures the timestamp and optionally validates the user's IP address to ensure they are at an authorized work location.
- **Target File:** `Punch_Lines.aspx`

### 3. Task Management (Timesheet Entries)
- **Action:** Throughout the day, the employee logs their hours against specific client projects.
- **Details:** 
    - Select a **Project** (e.g., SEO, Development).
    - Provide a **Task Description**.
    - Enter the **Duration** in minutes or hours.
- **Benefit:** Provides real-time visibility into project costs and resource utilization.
- **Target File:** `IKFProjects.aspx`

### 4. Automated Reporting (Closure)
- **Action:** At the end of the day, the automated **Daily Mailer** service runs.
- **Outcome:** The system compiles the employee's punch-in/out times and project logs, sending a "Daily Status Digest" to both the employee and their Reporting Manager.
- **Target File:** `Employee_dailyMailer.aspx`

---

## 🚦 Monitoring & Accuracy
- Managers use the **Attendance Report** to compare punch-in times against logged timesheet hours.
- Discrepancies (e.g., 8 hours punched but 0 hours logged in timesheets) are highlighted in the automated daily email for immediate correction.

---
> [!TIP]
> **Proactive Logging:** Employees are encouraged to log timesheet entries immediately after finishing a task to maintain 100% data accuracy for the Daily Mailer summary.
