# Workflow: Resource & Asset Management

This module manages the internal knowledge base and organizational assets like shared links and holiday calendars.

---

## 🔗 Useful Links Library
The Link Library (`link_list.aspx`, `links_add.aspx`) serves as a central repository for internal and external resources useful to the team.

### Features:
*   **Categorization:** Links are grouped by **Industry Type** (e.g., IT, Finance) and **Link Category** (e.g., Development, Documentation).
*   **Social Interaction (Tweets):** Each link supports a "Tweet" feature (`TweetList.aspx`) where employees can leave comments, tips, or feedback regarding the resource.
*   **Multi-Select Support:** Administrators can assign a single link to multiple categories simultaneously.
*   **Ownership:** Each link tracks which employee added it and from which IP address it was registered.


---

## 📅 Holiday Management
The system tracks the official **Holiday Calendar** using the `tbl_holidays` table.

### Business Use:
*   **Timesheet Validation:** The `Employee_dailyMailer` uses the holiday table to avoid flagging holidays as "unlogged" days.
*   **Payroll & Leaves:** Ensures holidays are correctly accounted for during leave applications and salary calculations.

---

## 🛠️ Technical Details
*   **Tables:** `tbl_link_category`, `tbl_industry_type`, `tbl_holidays`.
*   **Procedure:** `db_ikf_dcr.usp_link_add` handles the insertion and encoding of URLs.
*   **Security:** Uses `clsdal.writeLog` to capture errors during link registration into `/log/error_log.txt`.

---
## 🎯 Value Addition
Centralizing these resources within the Timesheet system ensures that employees have one-click access to frequently used documents and clarity on upcoming organization-wide holidays.
