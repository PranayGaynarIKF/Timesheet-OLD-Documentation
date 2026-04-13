# Business Workflow: Daily Employee Lifecycle

This document describes the standard "Day in the Life" workflow of an employee using the **Timesheet24** system. It demonstrates how different modules—from Attendance to Project Tracking—work together to manage organizational productivity.

---

## 🏁 Workflow Overview
The primary goal of the system is to ensure that every work hour is accounted for, presence is verified, and management receives automated updates on daily progress.

### The 5-Step Daily Cycle:
1.  **Authentication** (Accessing the Portal)
2.  **Punch-In** (Registering Attendance)
3.  **Timesheet Logging** (Project & Task Tracking)
4.  **Exceptions & Requests** (Handling Leaves/Late Marks)
5.  **Daily Closure** (Automated Reporting)

---

## 📈 Step-by-Step Breakdown

### 1. System Access (Authentication)
*   **Action:** The employee visits the portal and enters their credentials on the `login.aspx` page.
*   **Behind the Scenes:** The system validates the `UserDTO` against the database. If successful, a Session is created to track the user across the application.

### 2. Marking Presence (Punch-In)
*   **Action:** Upon entering the office or start of work, the employee navigates to the **Punch Lines** section (`Punch_Lines.aspx`).
*   **Function:** This records the exact start time. The system may capture the location or IP address (as configured in `Web.config`) to verify that the employee is working from an authorized zone.

### 3. Project Task Allocation (Timesheet)
*   **Action:** Throughout the day, or at the end of the day, the employee logs their hours on the **Project Tracking** page (`IKFProjects.aspx`).
*   **Details:**
    *   The employee selects a **Project**.
    *   They describe the **Tasks** performed.
    *   They enter the **Duration** (hours/minutes).
*   **Critical Goal:** This ensures that client billing and internal productivity metrics are accurate.

### 4. Handling Deviations (Leaves & Absences)
*   **Scenario:** If an employee cannot complete their full hours or needs time off, they use the **LMS (Leave Management System)** module.
*   **Workflow:**
    *   Employee applies via `Leave-Add.aspx`.
    *   The system checks their `Leave-Balance` (e.g., SL, PL, or CL).
    *   The Manager receives a notification for approval/rejection.

### 5. Automated System Closure (Daily Mailer)
*   **Action:** At a scheduled time (often late evening), the `Employee_dailyMailer.aspx` logic executes.
*   **Outcome:**
    *   It compiles the punch-in times and the logged projects.
    *   It sends an automated summary email (configured via SMTP in `Web.config`) to both the employee and their manager.
    *   This "Daily Digest" identifies any missing timesheets or attendance gaps.

---

## 🎯 Key Business Benefits
*   **Transparency:** Employees know exactly how many hours they have logged.
*   **Accountability:** Managers can see real-time progress on projects without manual follow-ups.
*   **Efficiency:** The automated mailer reduces the need for HR to manually track attendance logs.

---
> [!NOTE]
> This workflow represents the "Happy Path." In case of errors (e.g., forgetting to punch in), the user must follow the manual attendance correction process or consult the `ErrorPage.aspx` for support.
