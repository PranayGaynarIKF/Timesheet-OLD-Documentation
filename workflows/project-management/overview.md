# Workflow: Project & Client Management

Management of projects and client relations is the core engine that powers the Timesheet system. This module ensures every billable hour is mapped to the correct client.

---

## 🏢 Client Management
The system maintains a registry of clients in the `tbl_client` table.

### Technical Detail:
*   Clients are fetched via the `WebService.asmx (GetClientInfo)` for real-time searching during project setup.
*   Internal logic ensures that only "Active" clients (`client_delete_sts = 'N'`) are available for new projects.

---

## 📂 Project Lifecycle
Projects are the primary units for time tracking. They are managed through the `IKFProjects.aspx` interface.

### Key Logic:
1.  **Creation:** Projects are assigned a `service_type` (e.g., SEO, Development, Design) from `tbl_service`.
2.  **Assignment:** Employees are linked to projects when they log their first timesheet entry.
3.  **Status Tracking:** Projects move through stages such as "In Progress," "Pending," and "Completed."
4.  **Dashboard Integration:** Managers can view the top recent projects and their duration metrics using the `SP_GetUserDashboardProjectList` stored procedure.

---

## 🛠️ Technical Components
*   **Pages:** `IKFProjects.aspx` (List view), `Project_Add.aspx` (Management).
*   **Stored Procedures:**
    *   `db_ikf_dcr.usp_project_list`: Fetches categorized project lists.
    *   `db_ikf_dcr.vw_proj_list`: A SQL View used for optimized searching and sorting.
*   **Export Support:** Users can export project lists directly to Excel using the `btn_dwn_Click` event and the `GridViewExportUtil` utility.

---
## 🎯 Business Goal
The goal of this module is to provide granular visibility into project costs and resource allocation. By categorizing projects by service types, the organization can analyze which departments are most active and profitable.
