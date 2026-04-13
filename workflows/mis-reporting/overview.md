# Workflow: MIS & Reporting

The **MIS (Management Information System)** module is used by managers and administrators to extract high-level data regarding employee performance, attendance, and project progress.

---

## 🏗️ Technical Architecture
The reporting engine in this project relies on **Crystal Reports** (`.rpt` files). 

### Key Components:
*   **`mis.aspx`**: The primary container for report rendering. It uses the `CrystalReportWebFormViewer` control.
*   **`.rpt` Files**: Stored in the root and `/control/Contents/`.
    *   `mis1.rpt`: Standard MIS reporting.
    *   `mis_emp_performance.rpt`: Specific reports focusing on employee productivity and timesheet consistency.

---

## 📊 Standard Reports
1.  **Attendance Reports**: Generated using the `SP_GET_EMPLOYEE_ATTENDANCE` stored procedure. It visualizes daily status over a monthly period.
2.  **Performance Analytics**: Leverages the `usp_mis_GetEmployeeWeeklyData` procedure to compare logged hours against expected organizational hours.
3.  **Project Dashboard**: Tracks project timelines using stored procedures like `SP_GetUserDashboardProjectList`.

---

## 🛠️ Data Handling
The reporting logic typically follows this flow:
1.  **Filters**: Users select a date range or specific employee via a `DropDownList`.
2.  **Execution**: The system calls a `getDataSet` method in `DAL.cs` to fetch the records from SQL Server.
3.  **Binding**: The Resulting `DataSet` is bound to the Crystal Reports engine for export (Excel/PDF) or direct web viewing.

---
> [!TIP]
> **Exporting to Excel:** The system uses a utility class `GridViewExportUtil.cs` to convert data grids directly into `.xls` files for offline analysis.
