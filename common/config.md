# System Configuration Overview

This document explains how the **Timesheet24** application is configured. Since this is a legacy ASP.NET WebForms project, most settings are centralized in the `Web.config` file rather than modern JSON files.

---

## 📂 The Configuration Hub: Web.config
In legacy .NET projects, the `Web.config` file acts as the primary brain for the application. It is an XML file that tells the server how to run the app, which database to connect to, and how to handle security.

### Key Sections:
1.  **`<appSettings>`**: Stores simple key-value pairs (like email addresses, file paths, and passwords).
2.  **`<connectionStrings>`**: Stores the database "address" and login credentials.
3.  **`<system.web>`**: Configures the .NET runtime, session timeouts, and authentication modes.

---

## 📄 Web.config vs. appsettings.json
If you are coming from **.NET Core** or **Next.js**, you might be looking for an `appsettings.json` file. Here is how they compare:

| Feature | Legacy (Web.config) | Modern (appsettings.json) |
| :--- | :--- | :--- |
| **Format** | XML (`<tag>value</tag>`) | JSON (`"key": "value"`) |
| **Hierarchy** | Flat or Section-based | Deeply Nested |
| **Security** | Harder to protect via Git | Uses `secrets.json` or Environment Vars |

*Note: This project does not use `appsettings.json`. All changes must be made in `Web.config`.*

---

## 🗄️ Database Configuration
The application connects to the database using the `db_ConnectionString` located in the `<connectionStrings>` section.

**Example Structure:**
```xml
<add name="db_ConnectionString" 
     connectionString="Database=db_ikf_dcr;UID=username;PWD=password;Data Source='192.168.2.100';" />
```
*   **Data Source:** The IP address of the SQL Server.
*   **UID/PWD:** Credentials used by the application to access the data.

---

## 🔐 Authentication & JWT
Currently, the project is configured for **Windows Authentication** or basic Session-based logins.

*   **JWT (JSON Web Tokens):** This project **does not natively use JWT**. In modern apps, JWT is used for secure communication between a frontend (like React) and a backend. In this WebForms app, security is managed via **ASP.NET Session state**.
*   **Environment Variables:** While modern apps use `.env` files, this app reads directly from `Web.config`. To use environment-specific settings, developers typically use "Web.config Transformations" (e.g., `Web.Release.config`).

---

## 📝 Logging Configuration
Logging in this application is traditionally handled through:
1.  **Custom Error Logs:** Managed via the `ErrorLogDTO` and database tables.
2.  **Custom Errors Section:**
    ```xml
    <customErrors mode="Off" />
    ```
    Set `mode="On"` to show friendly error pages to users instead of code stack traces.

---

> [!TIP]
> **Security Warning:** Never commit real passwords (like the SMTP password found in `appSettings`) to public repositories. Always use placeholders and replace them during deployment.
