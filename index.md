# Documentation - Master Index

This is the central navigation hub for all technical and functional documentation relating to the **Timesheet24** project. This dashboard is designed to provide quick access to system overviews, detailed workflows, and architectural references.

## Overview
The **Timesheet24** system is a comprehensive HRMS and internal tracking solution used for managing employee attendance, leave applications, project time allocation, and performance reporting. These documents exist to preserve institutional knowledge, simplify onboarding, and provide a clear technical roadmap for system maintenance.

## Quick Navigation - Workflows
Detailed step-by-step guides for the system's most critical business processes:
- [**Authentication & Security Flow**](./workflows/system-admin/overview.md)
- [**Leave Management (LMS) Flow**](./workflows/sample-workflow/overview.md)
- [**Timesheet & Punching Flow**](./workflows/sample-workflow/overview.md)
- [**Project & Client Management**](./workflows/project-management/overview.md)
- [**Referral & Recruitment Flow**](./workflows/referral-system/overview.md)

## System Architecture
High-level overview of the technologies and environments powering the application.

### Technology Stack
- **Frontend/UI:** ASP.NET WebForms (Legacy .NET Framework 4.x)
- **Backend Logic:** C# (N-Tier Architecture)
- **Database:** SQL Server (Transact-SQL / Stored Procedures)
- **Reporting:** Crystal Reports (.rpt Engine)

### Server Environment
- **Web Server:** Internal IIS (Internet Information Services)
- **Hosting:** Local Data Center / On-Premise Servers
- **Database Server:** Dedicated SQL Server Instance

## How to Use This Documentation
Developers and stakeholders should use this repository as a technical manual:
- **Baseline Understanding:** Start with the [**Project README**](./README.md) for a high-level site map.
- **Task-Specific Research:** Use the **Workflows** links above to understand specific code execution paths.
- **Technical Reference:** Consult the **Technical Reference** section for database schemas and design decisions.

## Documentation Standards
To maintain a high standard of professional documentation, all files follow these rules:
- **Naming Conventions:** Use lowercase with hyphens for all file and folder names (e.g., `sample-workflow`).
- **Folder Structure:** Follow the hierarchical tree defined in the README (Common/Workflows).
- **File Consistency:** Every workflow must contain an `overview.md` and a `technical-implementation.md`.

## Maintenance
- **Maintenance Owner:** Central Development Team / Project Leads.
- **Update Frequency:** Documentation must be updated concurrently with any significant code changes or new module deployments.
- **Version Control:** All documentation is managed via Git; changes must be submitted via Pull Request for review.
