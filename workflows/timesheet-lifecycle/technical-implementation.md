# Technical Implementation: Timesheet & Daily Lifecycle

This document traces the code execution and technical components that power the daily employee journey.

---

## 🔐 1. Authentication Layer
The login process uses a combination of secure credential verification and session initialization.

- **File:** `login.aspx` / `login.aspx.cs`
- **Method:** Calls `UserRepository.LogintoApplication` using a stored procedure (`usp_login`).
- **Session Keys:**
    - `Session["id"]`: Unique Employee ID.
    - `Session["cat"]`: User role (e.g., ADMIN, HOD, APP).
    - `Session["UserName"]`: Display name for the dashboard.

---

## ⏱️ 2. Attendance (Punching)
Records the "Presence" of a user in the system.

- **File:** `Punch_Lines.aspx.cs`
- **Logic:**
    - Fetches the punch line category from `tbl_pl_category`.
    - Captures `REMOTE_ADDR` to track the user's IP address.
    - Executes a simple SQL insert or update via `clsdal.ExecScal`.

---

## 📂 3. Timesheet Logging
The core data entry module for billable and internal hours.

- **File:** `IKFProjects.aspx.cs`
- **Logic:** 
    - Loads active projects for the employee using `SP_GetUserDashboardProjectList`.
    - On submission, the hours and tasks are written to `tbl_time_sheet`.
- **SQL Component:** `vw_proj_list` (View used for optimized project searching).

---

## 📧 4. Automated Daily Mailer
A backend service page that consolidates daily activity and notifies stakeholders.

- **File:** `Employee_dailyMailer.aspx.cs`
- **Execution Flow:**
    1.  Fetch all employees who are active in `tbl_employee`.
    2.  Check for their punch entries for the current date.
    3.  Check for their timesheet entries.
    4.  Generate an HTML table summary.
    5.  Execute `clsdal.sendMailAllPages` using SMTP settings from `Web.config`.

---

## 🛠️ Performance & Scalability
- **Connection Optimization:** Uses the singleton-like `DAL.cs` to manage database connections efficiently across postbacks.
- **Client-Side Scripts:** Uses the `Milonic` JavaScript library for high-speed navigation menus.
