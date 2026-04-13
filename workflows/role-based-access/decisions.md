# Architectural Decisions: RBAC

This document outlines the rationale behind the Role-Based Access Control design for the Timesheet24 application.

---

## 🏗️ Decision: Role-Based vs. Permission-Based
**Choice:** Role-Based Access Control (RBAC).
**Rationale:**
- **Simplicity:** For an internal tool with only four primary departments (Admin, HRD, Account, Employee), permission-level granularity (e.g., `CanEditTimesheet`, `CanViewSalaries`) was deemed overly complex.
- **Maintenance:** Managing 4 static groups is faster and less error-prone for the IT team than managing hundreds of individual permission flags per user.

## 🔑 Decision: Session-Based vs. Token-Based (JWT)
**Choice:** Standard ASP.NET Session.
**Rationale:**
- **Legacy Compatibility:** The `WebForms` framework and `SAP Crystal Reports` integration rely heavily on the persistent, stateful nature of Server Sessions.
- **Security Control:** Since the application is used exclusively within a controlled office network, the server-side storage of user categories is more secure against client-side tampering than tokens.
- **IP-Restriction Integration:** Session initialization allows the system to easily lock a user's session to their registered IP address recorded during the login handshake.

## 🗄️ Decision: Stored Procedure Logic
**Choice:** Centralizing role retrieval in `usp_login`.
**Rationale:**
- **Security:** By making the Stored Procedure return the `@cat_name` as an output parameter, we ensure that the role logic cannot be bypassed by SQL injection attacks on the UI layer.
- **Atomic Operation:** Authentication and role fetching happen in a single database round-trip, reducing latency during the initial login phase.

## 🛡️ Security Considerations
- **Session Hijacking:** The application relies on the underlying IIS security settings to prevent Session Hijacking. 
- **Character Matching:** Since roles are stored as strings (`cat_name`), the application uses strict string comparison.
- **Fall-through Denial:** The global `ErrorPage.aspx` logic acts as a catch-all. If a session expires or a role is unreadable, access is denied by default (Fail-Safe behavior).

---
> [!NOTE]
> These decisions were made to balance security with developer productivity in a high-traffic legacy environment. For future cloud migrations, transitioning to a Claim-Based identity system is the recommended evolution.
