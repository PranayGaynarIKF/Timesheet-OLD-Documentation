# Database Operations: Leave Management

This document tracks the SQL Server tables and stored procedures specifically used by the Leave Management (LMS) module.

---

## 📊 Core Tables

| Table Name | Purpose | Key Columns |
| :--- | :--- | :--- |
| **`tbl_leaves`** | Primary table for all leave transactions. | `LA_Pk_Id`, `LA_FK_Employee_Id`, `LA_LeaveStatus` |
| **`tbl_leave_type`** | Master list of possible leave categories. | `LT_Pk_Id`, `LT_Name` |
| **`tbl_leave_balance`**| Current balance tracking for each employee. | `LB_FK_Employee_Id`, `LB_Fk_LT_Id`, `LB_BalanceCount` |
| **`tbl_holidays`** | Company-wide holiday list for validation. | `holi_date`, `holi_occassion` |

---

## ⚙️ Stored Procedures

### 1. Data Retrieval
- **`UspSelectLeaveTypeDetails`**: Initial call to populate the Leave Type dropdown in the UI.
- **`UspSelectEmployeeLeaveBalance`**: Fetches the remaining days for a specific employee and leave category.
- **`USPSelectAppliedLeaveByEmployeeId`**: Used for the "Leave History" list view.

### 2. Validation & Calculation
- **`usp_getLeaveBalanceDetails`**: Complex logic to calculate balance based on join dates and leave history.
- **`UspCalculateLeaveFromToDateDifference`**: Returns the count of working days between two dates, automatically excluding Sundays and Holidays.

### 3. State Management
- **`UspUpdateAppliedLeaveApproveByLeaveId`**: Called by the manager to approve a request.
- **`UspUpdateAppliedLeaveRejectedByLeaveId`**: Called by the manager to reject a request (requires a comment).
- **`UspUpdateAppliedLeaveCancelationByLeaveId`**: Called by the employee to withdraw a request.

---

## 🔍 Sample SQL Query: Fetching Leave History
This query is used by the system to show an employee their recent applications:

```sql
SELECT 
    l.LA_LeaveFrom, 
    l.LA_LeaveTo, 
    l.LA_LeaveTaken, 
    t.LT_Name, 
    l.LA_LeaveStatus
FROM db_ikf_dcr.tbl_leaves l
JOIN db_ikf_dcr.tbl_leave_type t ON l.LA_Fk_LT_Id = t.LT_Pk_Id
WHERE l.LA_FK_Employee_Id = @emp_id
ORDER BY l.LA_LeaveFrom DESC;
```

---
> [!IMPORTANT]
> **Data Integrity:** The application relies heavily on database triggers or managed procedures to ensure that `tbl_leave_balance` is updated *immediately* upon the approval of a request in `tbl_leaves`.
