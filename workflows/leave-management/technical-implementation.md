# Technical Implementation: Leave Management

This document details the code-level flow and architecture of the Leave Management module.

---

## 🏛️ Layered Logic
The LMS module follows the standard N-Tier architecture of the application:

1.  **Presentation Layer:** `Leave-Add.aspx` / `Leave-List.aspx` (UI interaction).
2.  **Service Interface:** `BusinessManager.ILMS` (Contract for leave operations).
3.  **Business Logic:** `LMSModel.cs` (Implements calculation and validation logic).
4.  **Data Access:** `DatabaseManager.LMS` (Wraps the calls to stored procedures).

---

## 💻 Primary Code Entry Points

### 1. Balance Calculation
When a user selects a leave type, the system calls `GetEmployeeLeaveBalance` in the `LMSModel.cs`:

```csharp
public string GetEmployeeLeaveBalance(string EmplopyeeId, string LeaveType)
{
    Dictionary<string, string> ObjMyList = new Dictionary<string, string>();
    ObjMyList.Add("@LB_FK_Employee_Id", EmplopyeeId.ToString());
    ObjMyList.Add("@LB_Fk_LT_Id", LeaveType.ToString());
    LMSMessage = LMS.ExcuteNonQueryProcwithParam(ObjMyList, "UspSelectEmployeeLeaveBalance");
    return LMSMessage ?? "0";
}
```

### 2. Inserting Applied Leave
The core submission logic uses a generic parameter object to pass data to the database:

```csharp
public string InsertAppliedLeaveDetails(object[] Insertdata)
{
    Dictionary<string, string> ObjMylist = new Dictionary<string, string>();
    ObjMylist.Add("@Leavetype", Insertdata[0].ToString());
    ObjMylist.Add("@employeeId", Insertdata[1].ToString());
    // ... Additional parameters (Dates, Reasons, etc.)
    LMSMessage = LMS.ExcuteNonQueryProcwithParam(ObjMylist, "UspInsertAllAppliedLeavesdetails");
    return LMSMessage;
}
```

---

## 📧 Notification Service
After a successful database insertion, the system triggers the custom Mailer service:
- Fetches Reporting Manager details based on `EmployeeId`.
- Reads email templates from `App_Code/BusinessManager/EmailTemplates.cs`.
- Sends an HTML-formatted mail to the manager using `System.Web.Mail`.

---

## 🛡️ Validation Rules (C#)
Before hitting the database, the `LMSModel.cs` performs these server-side checks:
*   `ValidateFromandTodate`: Ensures the start date is before the end date.
*   `CheckCompensatoryOffWorkdatevalidate`: Specifically validates if the selected date is valid for a Comp-Off claim.
*   `ValidateCompensatoryOffFromandTodate`: Checks for date/time overlaps in existing records.

---
> [!NOTE]
> All server-side validations return a string `LMSMessage`. If this message is not empty, the UI displays it as a red error label to the user, preventing the database commit.
