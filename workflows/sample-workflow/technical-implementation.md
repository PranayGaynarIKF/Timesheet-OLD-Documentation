# Technical Implementation: End-to-End Flow

This document outlines the technical architecture of the **Timesheet24** application, focusing on how data flows from the user interface to the database. 

---

## 🏗️ Architectural Pattern
The application follows a **Decoupled N-Tier Architecture**. While many pages use traditional code-behind logic, newer modules utilize a more structured Service/Repository pattern found in `App_Code`.

### The Standard Flow:
1.  **Entry Point (UI):** ASP.NET WebForms (`.aspx`) or AJAX Web Services (`.asmx`).
2.  **Business Logic:** Manager classes in `App_Code/BusinessManager`.
3.  **Data Access:** Centralized `DAL.cs` or specialized `Repositories`.
4.  **Data Objects:** `DataTransferObjects` (DTOs) for structured data movement.

---

## 🌐 API & Web Service Endpoints
The application exposes several endpoints via `WebService.asmx` and `WebService.cs` to support client-side features like AutoComplete.

### Common Endpoints:
*   **`GetEmployeeList(prefixText)`**: Returns a list of employees for search filters.
*   **`GetClientInfo(prefixText)`**: Retrieves active client names for project allocation.
*   **`GetProjectList(prefixText)`**: (Inferred) Fetches projects associated with a client.

---

## 🔄 Layer-by-Layer Flow Example
Using the **Attendance/Punching** and **Project Logging** flow as a reference:

### 1. Presentation Layer (UI)
In `Punch_Lines.aspx.cs`, the page captures data from the query string or form and interacts with the `DAL`.

```csharp
// Example from Punch_Lines.aspx.cs
public partial class Punch_Lines : System.Web.UI.Page {
    DAL clsdal = new DAL(); // Initialize Data Access Layer
    
    protected void Page_Load(object sender, EventArgs e) {
        if (!IsPostBack) {
            // Decodes ID and fetches punch line category from DB
            string categoryId = clsdal.Decode(Request.QueryString["id"]);
            lbl_desc.Text = clsdal.ExecScal("SELECT plcat_punch_line FROM tbl_pl_category WHERE plcat_id=" + categoryId);
        }
    }
}
```

### 2. Business Logic Layer (BLL)
For complex operations like Leave Management, the UI calls a "Model" or "Manager" class in `App_Code/BusinessManager`.

```csharp
// Conceptual Business logic in LMSModel.cs
public class LMSModel {
    public bool ApplyForLeave(LeaveDTO leaveData) {
        // 1. Validate if user has enough balance
        // 2. Check for overlapping dates
        // 3. If valid, call Repository or DAL to save
        return true;
    }
}
```

### 3. Data Access Layer (DAL)
The `DAL.cs` class is the central utility for all SQL operations. It uses ADO.NET to communicate with SQL Server.

```csharp
// Standard DAL Execution Pattern
public string ExecScal(string sql) {
    using (SqlConnection conn = new SqlConnection(db_connection_string)) {
        SqlCommand cmd = new SqlCommand(sql, conn);
        conn.Open();
        return cmd.ExecuteScalar()?.ToString();
    }
}
```

---

## 🛠️ Key Technical Components

| Component | Responsibility | Location |
| :--- | :--- | :--- |
| **UserDTO** | Represents the logged-in user session data. | `App_Code/DataTransferObjects` |
| **WebService** | Provides AJAX-callable methods for frontend UI updates. | `App_Code/WebService.cs` |
| **Encryption** | Handles encoding/decoding of sensitive IDs in URLs. | `App_Code/BusinessManager/Encryption.cs` |
| **Repositories** | Modern implementation of data access per module. | `App_Code/Repositories/` |

---

## ⚡ Technical Summary
*   **Frontend:** Uses ASP.NET Server Controls (`<asp:Label>`, `<asp:GridView>`).
*   **State Management:** Primarily relies on `Session["UserDTO"]` and `ViewState`.
*   **Database Integration:** Hardcoded SQL strings in older modules; Stored Procedures in newer modules.
*   **AJAX:** Handled via `ScriptManager` and `[WebMethod]` attributes for a smoother user experience.

---
> [!IMPORTANT]
> **Refactoring Note:** When adding new features, it is highly recommended to follow the **Repository** pattern in `App_Code` rather than writing raw SQL in `.aspx.cs` files to improve maintainability and security.
