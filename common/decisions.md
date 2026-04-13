# Architectural Decisions & Rationale

This document tracks the core design choices made during the development of the Timesheet24 application and explains why those paths were chosen over alternatives.

---

## 🏗️ Technology Stack: ASP.NET WebForms
**Decision:** Selected as the primary framework for the application.
**Rationale:**
- **Rapid Development:** Allowed for the quick construction of data-heavy enterprise portals using Server Controls.
- **State Management:** Integrated ViewState and Session support simplified the handling of complex multi-step forms.
- **Legacy Compatibility:** At the time of development, it provided the most stable support for Crystal Reports integration.

---

## 🗄️ Database Choice: SQL Server
**Decision:** Relational database over NoSQL/Flat-file storage.
**Rationale:**
- **Data Integrity:** Foreign key constraints are critical for ensuring that hours are never logged against non-existent projects or deleted employees.
- **Complex Reporting:** Many modules (like MIS) require joining 5+ tables, which is significantly faster in a relational engine with indexed views.
- **ACID Transactions:** Ensures that leave balance deductions and request approvals happen atomically.

---

## 🔐 Authentication: Session vs. JWT
**Decision:** Standard ASP.NET Session-based authentication.
**Rationale:**
- **Stateful Nature:** Since the system is an internal portal used for long sessions, server-side memory for user categories and IDs is more efficient than passing large tokens in every request.
- **Simplicity:** Easier to implement IP-based restriction and centralized login monitoring within the existing `UserRepository` architecture.

---

## 📈 Search Optimization: Database Views
**Decision:** Using `vw_proj_list` instead of complex Linq/ADO.NET joins in C#.
**Rationale:**
- **Performance:** SQL Server performs joins and filtering on views significantly faster than the application layer.
- **Code Cleanliness:** Simplifies the code-behind logic in `IKFProjects.aspx.cs` to a single "SELECT * FROM View" call.

---
> [!NOTE]
> These decisions reflect the industry best practices at the time of the initial system architecture. For future modernizations, migrating to a stateless Web API / React architecture is the recommended path.
