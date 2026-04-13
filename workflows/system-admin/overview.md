# Workflow: System Administration & Security

This document covers the administrative controls and security protocols that protect the **Timesheet24** portal.

---

## 🔐 Password & Identity Management
Security is managed through a combination of traditional credential checks and modern policy enforcement.

### Key Logic:
1.  **Forced Password Expiry:** The system tracks the number of days since the last password change. Based on the `password_change_day` setting in `Web.config` (e.g., 30 days), users are automatically prompted to update their credentials.
2.  **Forgotten Passwords:** Users can request a reset via `forgot_password.aspx`. The system generates a temporary secure link sent to their office email (`emp_off_email`).
3.  **Password Strength:** Changes are processed via the `usp_change_pwd` stored procedure, which validates the old password before committing the new one.

---

## 🛡️ Security Infrastructure
*   **IP Restrictions:** The `UserRepository` contains logic to detect the user's IP address. For high-security environments, the system can restrict access to specific authorized IP ranges.
*   **HOD (Head of Department) Logic:** A dedicated field `emp_is_hod` in `tbl_employee` grants elevated permissions for viewing department-wide reporting and approving sensitive requests.
*   **Session State:** The application is stateful, using ASP.NET Session objects (`Session["id"]`, `Session["cat"]`) to maintain user identity across postbacks.

---

## 🛠️ Administrative Operations
*   **Role Categorization:** Users are assigned roles (e.g., APP, HOD, ADMIN) which determine which pages appear in the Milonic JS menu.
*   **Error Logging:** Critical failures are captured by `clsdal.writeLog` and stored in the `/Log/error_log.txt` file for administrator review.

---
## 🎯 Technical Components
*   **Pages:** `change_pwd.aspx`, `forgot_password.aspx`, `Reset_Password.aspx`.
*   **Security Manager:** `App_Code/BusinessManager/Encryption.cs` handles URL parameter encoding to prevent tampering.
*   **Service Layer:** `LoginService.cs` encapsulates the core authentication workflow.
