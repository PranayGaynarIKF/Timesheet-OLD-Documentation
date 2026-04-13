# Architectural Decisions & Rationale

This document explores the foundational technical choices made during the development of the **Timesheet24** project and provides context for why certain patterns were chosen over modern alternatives.

---

## 🏗️ Architecture: Why N-Tier WebForms?
The application is built using **ASP.NET WebForms** with a layered approach (DAL, BLL, UI).

*   **Choice:** Separation of concerns using `App_Code` libraries.
*   **Reasoning:** At the time of development, WebForms provided a robust, event-driven framework that integrated seamlessly with **IIS** and **Active Directory**. The multi-tier approach ensures that database logic (`DAL.cs`) is centralized, making it easier to maintain than embedding SQL directly in every page.
*   **Outcome:** A stable, high-performance system for internal organizational use where server-side rendering (SSR) is preferred for security and consistency.

---

## 🔐 Security: Session vs. JWT
A common question when reviewing this project is the choice of state management.

*   **Decision:** This system uses **Server-Side Session State** and **Windows/Forms Authentication**.
*   **Why not JWT?** 
    *   **JWT (JSON Web Tokens)** are ideal for stateless, distributed systems (like React/Mobile apps talking to a REST API).
    *   **Sessions** are better suited for this legacy monolithic architecture because the server maintains complete control over the user's state. It simplifies roles and permissions without the overhead of token management and expiration handling required by JWT.

---

## 🗄️ Database: SQL vs. NoSQL
The project relies exclusively on **Microsoft SQL Server**.

*   **Decision:** A strictly **Relational (SQL)** database model.
*   **Reasoning:** 
    *   **Complex Relations:** HRMS data (Employees, Projects, Timesheets, Leaves) is highly interconnected. SQL's "Joins" and "Foreign Keys" ensure data integrity.
    *   **ACID Compliance:** For attendance and payroll-related tracking, transactions must be atomic and consistent—a core strength of SQL Server.
    *   **Reporting:** Tools like **Crystal Reports** (used in this project) are designed to work natively with relational schemas, making it easy to generate complex MIS reports.
    *   **Why not NoSQL?** NoSQL is excellent for unstructured, high-velocity data (like social media feeds), but it lacks the strict relational integrity required for financial and attendance tracking in an enterprise environment.

---

## 🚀 Legacy Considerations
While the system uses patterns that are older (e.g., `DataSet`, `DataTable`, and encoded URL parameters), these choices were made to ensure compatibility with then-current server environments and to maximize developer productivity within the .NET ecosystem of the time.

---
> [!NOTE]
> Future migrations toward a modern stack (like .NET Core) would likely move toward **JWT** for authentication and a **Web API** structure, while retaining the **SQL** foundation for data integrity.
