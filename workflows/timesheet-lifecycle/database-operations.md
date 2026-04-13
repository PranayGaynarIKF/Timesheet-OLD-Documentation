# Database Operations: Timesheet & Daily Lifecycle

This document tracks the SQL Server tables and logic that support the daily operational cycles.

---

## 📊 Lifecycle Tables

| Table Name | Description | Key Columns |
| :--- | :--- | :--- |
| **`tbl_login`** | Stores employee credentials and roles. | `login_id`, `emp_id`, `cat_name` |
| **`tbl_employee`** | Master registry of all staff members. | `emp_pk_id`, `emp_off_email`, `emp_is_active` |
| **`tbl_pl_category`**| Stores the categories for "Punch Lines". | `plcat_id`, `plcat_punch_line` |
| **`tbl_time_sheet`** | Transactional table for all logged hours. | `time_sheet_id`, `emp_id`, `proj_id`, `total_hours` |

---

## ⚙️ Core Stored Procedures

- **`usp_login`**: Authenticates the user and returns the role category and employee status.
- **`SP_GET_EMPLOYEE_ATTENDANCE`**: Consolidates data from punch records and leaves for reporting.
- **`SP_GetUserDashboardProjectList`**: Retrieves a list of active projects assigned to the user for their timesheet view.
- **`usp_get_timesheet_summary`**: Aggregate procedure used by the Daily Mailer to calculate totals for the day.

---

## 🔍 Sample SQL Query: Fetching Daily Productivity
The following query is used to calculate how many hours a user has logged for a specific day:

```sql
SELECT 
    p.proj_name, 
    t.task_description, 
    t.total_hours
FROM db_ikf_dcr.tbl_time_sheet t
JOIN db_ikf_dcr.tbl_project p ON t.proj_id = p.proj_id
WHERE t.emp_id = @id AND t.logged_date = @today;
```

---
## 🎯 Views & Optimizations
- **`vw_proj_list`**: A database view used to join project details with client names, significantly reducing the complexity of the code-behind in `IKFProjects.aspx`.
- **Primary Keys:** Almost all tables use `INT IDENTITY(1,1)` for primary keys to ensure high performance during bulk inserts (common in high-frequency timesheet environments).
