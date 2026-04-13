# Workflow: Leave Management System (LMS)

The **Leave Management System (LMS)** is a core module that allows employees to apply for various types of leave, track their remaining balances, and enables managers to approve or reject these requests.

---

## 📈 Functional Flow

### 1. Leave Application (Employee)
An employee logs into the system and navigates to the "Leave Add" page.
- **Select Leave Type:** Privilege Leave (PL), Sick Leave (SL), Casual Leave (CL), or Compensatory Off (CO).
- **Date Selection:** Choose "From Date" and "To Date".
- **Validation:** The system automatically checks if the employee has enough balance before allowing the submission.
- **Submission:** The request is saved as "Pending" in the database.

### 2. Approval Cycle (Manager)
Once a leave request is submitted:
- **Notification:** The Reporting Manager is notified (via dashboard or email).
- **Review:** The manager views the request details and checks the employee's reason and history.
- **Action:** The manager can either **Approve** or **Reject** the request.
- **Impact:** 
    - If **Approved**, the leave is deducted from the master balance.
    - If **Rejected**, the status is updated, and the balance remains unchanged.

### 3. Cancellation (Employee)
Employees have the option to cancel a request if it has not yet been processed by the manager.

---

## 🚦 Leave Statuses
| Status | Description |
| :--- | :--- |
| **Pending** | Default status after submission; awaiting manager action. |
| **Approved** | Processed and granted by the reporting head. |
| **Rejected** | Denied by the reporting head with a comment. |
| **Canceled** | Withdrawn by the employee before approval. |

---

## 🎯 Key Business Rules
- Employees cannot apply for more leave than their currently available balance.
- Backdated leave applications may be restricted based on company policy (e.g., PL must be applied in advance).
- Manager approval is mandatory for all leave types except as defined by exception rules.
