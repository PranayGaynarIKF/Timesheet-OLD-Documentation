# Database Operations & Schema

This document provides an overview of the SQL Server database structure used by the **Timesheet24** system. The database is primarily named `db_ikf_dcr`.

---

## 🏗️ Core Database Schema
The database follows a relational model centered around the **Employee** entity. Most transactions (Timesheets, Leaves, Attendance) are mapped back to a specific Employee ID.

### Primary Tables:

| Table Name | Description | Key Columns |
| :--- | :--- | :--- |
| **`tbl_employee`** | Master list of all staff members. | `emp_id`, `emp_name`, `emp_off_email` |
| **`tbl_login`** | Authentication records and user statuses. | `user_id`, `log_sts`, `user_login_name` |
| **`tbl_project`** | List of all internal and client projects. | `proj_id`, `proj_name`, `proj_duration` |
| **`tbl_client`** | Customer/Client information. | `client_id`, `client_name` |
| **`tbl_time_sheet`** | Daily work logs recorded by employees. | `tsh_id`, `tsh_project_id`, `tsh_total_min` |
| **`tbl_pl_category`**| Attendance marking types (Punch Lines).| `plcat_id`, `plcat_punch_line` |
| **`tbl_holidays`** | Company holiday calendar. | `holi_date`, `holi_occassion` |
| **`tbl_leaves`** | Detailed records of leave applications. | `leave_id`, `emp_id`, `leave_type` |
| **`tbl_Page_Details`**| System navigation and menu mappings. | `page_id`, `page_name` |


---

## 🔄 Relationship Mapping
*   **Employee ↔ Timesheet:** One employee can have many timesheet entries (One-to-Many).
*   **Project ↔ Timesheet:** One project can be referenced by many employees in their work logs.
*   **Client ↔ Project:** One client can have multiple projects assigned to them.
*   **Employee ↔ Attendance:** Every "Punch Line" entry is unique to an employee.

---

## 🔍 Sample SQL Queries
These queries represent the typical data retrieval patterns used in the project's code-behind files.

### 1. Fetching Daily Timesheet Summary
Used to calculate how many hours an employee has logged for a specific project.
```sql
SELECT 
    p.proj_name, 
    SUM(t.tsh_total_min) / 60.0 AS TotalHours
FROM db_ikf_dcr.tbl_time_sheet t
JOIN db_ikf_dcr.tbl_project p ON t.tsh_project_id = p.proj_id
WHERE t.tsh_add_id = @EmployeeID -- e.g., 674
GROUP BY p.proj_name;
```

### 2. Validating User Login
Used in the `UserRepository` to authenticate users.
```sql
SELECT emp_id, emp_name, emp_role 
FROM db_ikf_dcr.tbl_employee 
WHERE emp_login_id = @Username AND emp_pwd = @Password 
AND emp_delete_sts = 'N';
```

### 3. Checking Active Projects for Dashboard
Retrieves the most recent projects an employee is currently tracking time against.
```sql
SELECT TOP 5 proj_name, proj_start_date 
FROM db_ikf_dcr.tbl_project 
WHERE proj_current_sts != 'Completed'
ORDER BY proj_start_date DESC;
```

---

## 🛠️ Data Integrity Notes
*   **Soft Deletes:** Most tables use a `*_delete_sts` column (e.g., `emp_delete_sts = 'N'`) instead of deleting records permanently.
*   **Encryption:** Passwords and sensitive query parameters are often encrypted/encoded before being stored or passed in URLs (see `DAL.Encode` method).
*   **Stored Procedures:** Performance-critical queries (like dashboard lists) are moved to Stored Procedures for optimization.

---
> [!TIP]
> **Database Owner:** In connection strings, the database owner is typically referenced as `dbo_ikf_dcr` or `dbo`. Always check the `Web.config` for the active database schema prefix.
