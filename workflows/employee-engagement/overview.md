# Workflow: Employee Engagement

This module focuses on the social and cultural aspects of the organization, tracking employee milestones and personal events.

---

## 🎂 Upcoming Birthdays
The system maintains a real-time list of employees celebrating their birthdays within a specific window.

### Logic Flow:
1.  **Date Retrieval:** The system scans the `tbl_employee` table.
2.  **Date Calculation:** It calculates the difference between the employee's Date of Birth (`emp_dob`) and the current date.
3.  **Active Window:** It displays employees whose birthdays fall between **4 days ago** and **8 days from today**.
4.  **UI Feedback:** If no birthdays are scheduled, a "Sorry No Upcoming Birthdays" message is displayed with a friendly icon.

---

## 🆕 New Joinees & Employee Previews
The **New Joinee** portal (`emp_Joinee.aspx`) and **Employee Previews** (`emp_preview.aspx`) introduce recently hired employees and allow others to view staff profiles.

### Key Features:
*   **Profile Images:** Displays the photograph of the employee (stored in `/control/Contents/Emp_image/`).
*   **Introductory Context:** Provides basic profile information, job titles, and contact details to help with internal team integration.


---

## 🛠️ Technical Implementation
*   **Pages:** `emp_birthday.aspx`, `emp_Joinee.aspx`.
*   **Controls:** Uses ASP.NET `DataList` controls for repeating employee cards.
*   **Image Management:** Utility class `ImageInfo.cs` handles image aspect ratios and display logic.

---
## 🎯 Purpose
The goal of this module is to improve internal communication and foster a positive workplace culture by ensuring that milestones like birthdays and new hires are visible to the entire team.
