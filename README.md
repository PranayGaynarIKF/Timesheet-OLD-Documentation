# Project Documentation: Timesheet24

Welcome to the central technical documentation repository for the **Timesheet24** enterprise system. This repository serves as the definitive source of truth for the system's architecture, business workflows, and database schemas.

## Overview
This documentation provides a comprehensive breakdown of the internal legacy Timesheet Management System. It covers everything from technical configurations and code-behind logic to end-to-end business operations. The repository is designed to assist developers, architects, and stakeholders in understanding, maintaining, and scaling the existing .NET infrastructure.

## Purpose
The primary purpose of this documentation is to bridge the knowledge gap between legacy implementation and modern development standards. It aims to:
- Centralize scattered technical knowledge into a structured format.
- Provide a clear roadmap for system maintenance and future refactoring.
- Reduce onboarding time for new engineers joining the project.
- Document complex business logic and "dark" system modules.

## Why This Documentation?
Maintaining this documentation repository provides several strategic benefits:
- **Consistency:** Ensures all team members follow the same technical patterns and business rules.
- **Risk Mitigation:** Documents critical dependencies and legacy logic that are often undocumented.
- **Scalability:** Simplifies the process of identifying modules for modernization or API transition.
- **Searchability:** Offers a searchable, version-controlled environment for technical research using simple markdown.

## Architecture Overview
The system follows a traditional **N-Tier Architecture** pattern to ensure separation of concerns:
- **Presentation Layer (ASPX):** Handles user interactions and input through WebForms.
- **Service Layer (BLL):** Contains the business rules and core logic of the application.
- **Repository Layer (DAL):** Manages data persistence and abstraction from the underlying SQL Server.
- **Database Layer (SQL Server):** Stores all transactional data, stored procedures, and master records.

## Getting Started
To begin exploring the documentation, follow these steps:
1. Start with the [**index.md**](./index.md) file, which acts as the master navigation hub.
2. Review the [**Common Configuration**](./common/config.md) to understand the environment setup.
3. Explore the **Workflows** directory for detailed step-by-step business logic and technical code-paths.

## Documentation Structure
The repository is organized into logical directories for easy discovery:
- **`common/`**: Contains core system settings, `Web.config` analysis, and global variables.
- **`workflows/`**: Organized by module (e.g., LMS, Referrals, MIS) containing functional and database overviews.
- **`index.md`**: The main table of contents and technical entry point.
- **`README.md`**: High-level project introduction and repository guide.

## Key Features
- **Modular Documentation:** Each business unit is documented independently for better maintenance.
- **Workflow-Based Structure:** Focuses on real-world business cycles and code execution paths.
- **Easy Navigation:** Cross-referenced links between logical overviews and technical implementations.
- **Scalable Design:** Specifically built to allow the easy addition of new modules as the system evolves.

---
*Documentation Version: 1.0.0*