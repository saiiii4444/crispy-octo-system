# Power App Design: Employee Onboarding

This document outlines a simple canvas app to manage employee onboarding.  Use it as a blueprint when you build your own application in Power Apps.

## Data source

Use the table from `employee_onboarding.csv` as your primary data source.  When importing the Excel file into Power Apps, check *Use first row as headers* so the fields map correctly.

## 1. Home screen

* Display a **gallery** listing all employees.  Each gallery item should show:
  * Employee name
  * Position / Department
  * Status badge (colour coded: red for “Onboarding”, orange for “In Progress”, green for “Completed”).
* At the top, add a **search box** to filter the gallery by name or department (set the gallery’s `Items` property to `Filter(Employee_onboarding, StartsWith(Name, SearchBox.Text))`).
* Add a **“New employee”** button that navigates to the new hire form.

## 2. Detail screen

When a user selects an employee from the home screen, navigate to this screen.  Use a **display form** or **edit form** to show and edit the employee record.  Fields to include:

* `Name`, `StartDate`, `Position`, `Department`, `Manager` (read‑only or editable depending on your process).
* Toggle controls for `OrientationComplete` and `DocumentsSubmitted` (bind to the corresponding Boolean fields).
* A date picker for `TrainingCompletionDate` (only visible when the employee’s status is `Completed`).
* A multi‑line text input bound to `Comments`.

Include **Save** and **Cancel** buttons.  Use the `Patch` function to update the record:

```powerapps
Patch(Employee_onboarding, SelectedEmployee, {
    OrientationComplete: ToggleOrientation.Value,
    DocumentsSubmitted: ToggleDocuments.Value,
    TrainingCompletionDate: DatePickerTraining.SelectedDate,
    Comments: CommentsInput.Text
});
```

## 3. New hire form

Use an **edit form** with default mode `FormMode.New`.  Fields should include:

* Name (Text input)
* Start date (Date picker with default value `Today()`)
* Position (Drop‑down with items from a hard‑coded list or a separate table)
* Department (Drop‑down)
* Manager (Text input or people picker if integrated with Azure AD)
* Initial status (set default to `Onboarding`)
* OrientationComplete and DocumentsSubmitted (default to `false`)

On submit, call `Patch` with `Defaults(Employee_onboarding)` to create the new record, then navigate back to the home screen.

## 4. Analytics screen (optional)

Embed a **Power BI** tile showing onboarding metrics.  Publish a report created from the same dataset (see instructions in the README).  Use the *Power BI tile* control and set its `Workspace` and `Report` properties accordingly.

## Navigation and logic

* Use `Navigate()` to switch between screens.
* Reset form controls after submission by calling `Reset(FormName)`.
* Use `If` statements to conditionally display controls (e.g., show the training completion date picker only when `Status = "Completed"`).

By following this layout you’ll create a user‑friendly application that streamlines onboarding tasks and provides real‑time insight into progress.