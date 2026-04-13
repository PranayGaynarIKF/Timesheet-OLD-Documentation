# Workflow: Role-Based Access Control (RBAC)

The **Role-Based Access Control (RBAC)** system manages user permissions based on their assigned organizational functions. This ensures that users can only access the features and data required for their specific role.

---

## 🚦 RBAC Workflow Step-by-Step

### 1. User Authentication
The process begins at the `login.aspx` page. The user enters their credentials, which the system validates against the encrypted records in the database.

### 2. Role Assignment
Once credentials are verified, the system fetches the user's category from the **`tbl_login`** table. Based on this record, one of the following roles is assigned:
- **Admin**: Full system access (All modules and configurations).
- **HRD**: Access to employee management, leave approvals, and recruitment portals.
- **Account**: Access to financial reports, project billing metrics, and cost centers.
- **Employee**: Standard access to personal timesheets, leave applications, and profiles.

### 3. Session Initialization
Instead of generating a JWT token (common in stateless APIs), this system initializes a **Server-Side Session**. The assigned role is stored in `Session["cat"]`, and the user's HOD (Head of Department) status is stored in `Session["hod"]`.

### 4. Access Request
When a user tries to access a page (e.g., `mis.aspx` or `Project_Add.aspx`), the system executes an authorization check.

### 5. Role Verification
The system inspects the active Session variables. If the `Session["cat"]` does not match the required permissions for that specific file or menu item, the system prevents the page from loading.

### 6. Grant or Deny
- **Access Granted:** The page renders the relevant controls and data for the user.
- **Access Denied:** The user is redirected to a custom `ErrorPage.aspx` or back to the dashboard with an "Unauthorized Access" alert.

---
> [!NOTE]
> **HOD Status:** In addition to the four primary roles, an employee may also have the `HOD` flag enabled. This grants them elevated "Department-Level" approval rights even while retaining their base "Employee" or "HRD" role permissions.
