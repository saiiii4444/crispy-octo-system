# Employee Onboarding Power App

This project sketches out a **Power Apps** solution for managing the onboarding of new employees.  It includes a sample dataset (`employee_onboarding.csv`) and documentation to guide you in building your own app and accompanying dashboards.

## Overview

Onboarding is more than just paperwork — it’s the process of integrating new hires into your organisation.  A Power App can streamline this process by providing:

* **A central form** for HR or hiring managers to enter new employee details.
* **A checklist** of tasks (e.g., orientation, badge request, equipment provisioning, training sessions) with completion status.
* **Progress tracking** so managers can see at a glance which steps are pending or completed.
* **Notifications or approvals** (via Power Automate) for steps that require actions from other teams.

The sample CSV file gives you realistic data to start building screens and analytics.

## Dataset (`employee_onboarding.csv`)

The file holds 40 example employees with the following columns:

| Column | Description |
|---|---|
| `EmployeeID` | Unique identifier (e.g., E001). |
| `Name` | Full name of the employee. |
| `StartDate` | Date the employee started or will start (YYYY‑MM‑DD). |
| `Position` | Job title. |
| `Department` | Department to which the employee belongs. |
| `Manager` | Name of the manager or supervisor. |
| `Status` | Onboarding phase (`Onboarding`, `In Progress` or `Completed`). |
| `OrientationComplete` | Boolean indicating whether the orientation session is done. |
| `DocumentsSubmitted` | Boolean indicating whether HR documents have been submitted. |
| `TrainingCompletionDate` | Date when mandatory training was completed (blank if not yet finished). |
| `Comments` | Miscellaneous notes (e.g., “Needs ID card”, “Laptop ready”). |

You can import this CSV as an **Excel data source** in Power Apps or load it into **Dataverse**.  Once imported, you can bind controls (e.g., galleries, forms, toggle switches) directly to these fields.

## Suggested app design

The file `app_design.md` (in this folder) outlines a simple canvas app structure:

1. **Home screen:** a gallery listing all employees with colour‑coded status indicators and a button to add a new hire.
2. **Detail screen:** form controls bound to the employee record; toggles for orientation completion and document submission; date picker for training completion; comments field.  Use the **Patch** function to save edits back to the data source.
3. **New hire form:** pre‑populated StartDate with today’s date; drop‑down lists for position and department; manager auto‑lookup; submit button to create a new record.
4. **Analytics screen:** embed a Power BI tile (if you publish a report) showing metrics such as number of employees by status, average days to completion, etc.

## Building the Power App

1. **Prepare the data:** Save `employee_onboarding.csv` to your OneDrive or upload it to SharePoint/Dataverse.  In Power Apps Studio, create a new canvas app and choose *Excel (OneDrive)* or your connector to import the table.  Ensure you check *Use first row as headers* so the column names match the dataset.
2. **Create screens:** Follow the design described above or adapt it to your workflow.  Use galleries to display lists and forms for editing/creating records.  Set the `Items` property of the gallery to the table name (e.g., `Employee_onboarding`).
3. **Add logic:** Use `If` statements and the `Patch` function to update records.  For example, set the colour of a status label based on `ThisItem.Status` (green for `Completed`, orange for `In Progress`, red for `Onboarding`).
4. **Automate steps:** With **Power Automate**, you can trigger emails to managers when documents are submitted or tasks are completed.

## Reporting in Power BI

To monitor onboarding progress, import `employee_onboarding.csv` into Power BI.  Create measures such as:

* **Number of employees by status:** a simple bar or doughnut chart.
* **Average time to completion:** calculate the difference between `StartDate` and `TrainingCompletionDate` for completed employees.
* **Task completion rates:** count how many employees have `OrientationComplete = true` or `DocumentsSubmitted = true`.

These insights will help HR teams identify bottlenecks and improve the onboarding process.