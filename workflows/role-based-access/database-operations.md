# Database Operations: RBAC

This document details the database schema and queries that support the Role-Based Access Control system.

---

## 📊 Database Schema

### 1. The Membership Table (`tbl_login`)
This is the primary table for managing system access. It serves as a combined User-and-Role mapping table.

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `login_id` | `INT (PK)` | Unique identifier for the account. |
| `user_name` | `VARCHAR` | The login username. |
| `password` | `VARCHAR` | The encrypted user password. |
| **`cat_name`** | `VARCHAR` | **Core Role Field** (Admin, HRD, Account, Employee). |
| `is_hod` | `CHAR(1)` | Manager flag ('Y' or 'N'). |
| `emp_id` | `INT (FK)` | Links to the `tbl_employee` master table. |

### 2. Relationships
- **Implicit Mapping:** Unlike modern systems with a many-to-many `UserRoles` table, this legacy schema uses an **Implicit One-to-One Mapping**. An employee is linked to exactly one user account, which has exactly one category (`cat_name`).

---

## ⚙️ Core Stored Procedure: `usp_login`
This procedure is the gatekeeper for the entire application. It performs both authentication and role retrieval.

### Input Parameters:
- `@user_name`: Entered username.
- `@password`: Entered (and previously encrypted) password.
- `@ip_address`: The IP of the user (for IP-restriction checks).

### Output Parameters:
- `@cat_name`: Returns the Role (e.g., 'HRD').
- `@emp_id`: Returns the Internal Employee ID.
- `@ishod`: returns the Manager status flag.

---

## 🔍 Sample SQL Query: Assigning a Role
To update a user's role to **Account**, an administrator would execute a query similar to this:

```sql
UPDATE db_ikf_dcr.tbl_login
SET cat_name = 'Account', is_hod = 'N'
WHERE user_name = 'abhishek_ikf';
```

## 🔍 Sample SQL Query: Fetching Role-Based Counts
Used for administrative dashboards to see user distribution:

```sql
SELECT cat_name, COUNT(*) as UserCount
FROM db_ikf_dcr.tbl_login
GROUP BY cat_name;
```

---
> [!TIP]
> **Data Integrity:** Because roles are stored as plain strings (e.g., 'HRD') instead of foreign keys to a `MasterRoles` table, ensure that update queries use exact casing and spelling to avoid breaking session-based application security.
