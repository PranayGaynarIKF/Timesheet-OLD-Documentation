# Technical Implementation: Role-Based Access Control

This document explains the technical mechanisms used to enforce security and roles within the **Timesheet24** project.

---

## 🏗️ Role Definition
Roles are NOT hardcoded in code as constants. Instead, they are defined in the database and loaded dynamically into the application during the login phase.

- **Primary Roles:** `Admin`, `HRD`, `Account`, `Employee`.
- **Secondary Flag:** `isHod` (Determines approval authority).

---

## 🔐 Implementation Details

### 1. Data Transfer Object (DTO)
The system uses a `LoginDTO` to pass role information from the repository layer to the UI layer.

```csharp
public class LoginDTO
{
    public int UserId { get; set; }
    public string UserCategory { get; set; } // Stores Admin/HRD/Account/Employee
    public string IsHod { get; set; }        // Stores 'Y' or 'N'
    public string RoleId { get; set; }
}
```

### 2. Session-Based Authorization
The `login.aspx.cs` file initializes the user's role in the global `HttpContext.Current.Session` object.

```csharp
// Inside btn_submit_Click
if (loginDTO.UserId != 0)
{
    Session["id"] = loginDTO.UserId;
    Session["cat"] = loginDTO.UserCategory; // Store Role Name
    Session["hod"] = loginDTO.IsHod;        // Store Manager Status
    
    Response.Redirect("control/user.aspx");
}
```

### 3. Page-Level Access Checks
Since this is a WebForms project, access checks are performed manually in the `Page_Load` event of secured pages or via the Master Page logic.

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    // Authorization Check: Only Admin can access this page
    if (Convert.ToString(Session["cat"]) != "Admin")
    {
        Response.Redirect("../ErrorPage.aspx?msg=Unauthorized", true);
    }
}
```

### 4. Dynamic Menu Filtering
The `Milonic` navigation menu filters links based on the role stored in the Session. If a user is logged in as an **Employee**, the "Admin Settings" menu header is programmatically hidden on the client side.

---

## 🏢 Service Layer Flow
The access check follows this standard internal flow:

1.  **UI**: `login.aspx` collects username/password.
2.  **Service**: `LoginService` calls the repository.
3.  **Repository**: `UserRepository` executes `usp_login`.
4.  **Database**: The Stored Procedure returns the `@cat_name` and `@ishod` output parameters.
5.  **Return**: The role info is bubbled back up to the Session.

---
> [!IMPORTANT]
> **No Global Filters:** Unlike modern MVC projects that use `[Authorize(Roles="Admin")]`, this legacy application requires developer diligence to manually add the Session check at the top of every new `.aspx` file created.
