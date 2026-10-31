# SRS – Add Timelog (PM3 Timesheet)

| Item | Value |
|---|---|
| Module | Timekeeping > Timesheet |
| URL | https://pm3.formos.com/timesheet |
| Source | Observed on the live screen (31 Oct 2026). Items marked **[TBC]** were not verified because no real timelog was saved. |

---

## 1. Background

Users record daily working time on the Timesheet screen. The screen has a daily timeline (hours 0–23, with a **Total** hours value) and a work item list at the bottom.

A user adds time to the timesheet by picking a work item. There are five ways to pick one:

1. From **Favorites**
2. From **Tasks**
3. From **General Items**
4. From **Company / internal**
5. From **Previous Work Items** (previous day, past week, past month)

## 2. Objective

- Let users add a timelog quickly, with the work item they need.
- Keep the same rule for all five entry points: pick a work item, then add it to the timesheet of the selected date.

## 3. Scope

**In scope**
- Add timelog from: Favorites, Task, General Item, Company / internal, Previous work item.
- Filtering and searching the work item list needed for adding.

**Out of scope**
- Editing or deleting a timelog.
- Finalize, Download as CSV, Expand range.
- Questions and Bug Reports tabs.
- Reports and approval flow.

## 4. Common Rules

| ID | Rule |
|---|---|
| CR-01 | The timelog is added to the date currently selected in the date picker (default: today). |
| CR-02 | The user adds a work item with the **Add to timesheet** button (up-arrow icon) at the start of each row. |
| CR-03 | After a successful add, the timeline and the **Total** hours show the new entry. **[TBC]** |
| CR-04 | Closed projects and closed iterations are hidden by default. The user can include them with the **Closed** checkbox. |
| CR-05 | The list is paged (25 rows per page) and shows "Showing X to Y of Z". |
| CR-06 | A finalized timesheet must not accept new timelogs. **[TBC]** |
| CR-07 | The **Fav** column shows whether a work item is a favorite. |

## 5. Functional Requirements

### FR-01 Add timelog from Favorites

**Entry:** Click the **Favorites** (star) filter button.

| ID | Requirement |
|---|---|
| FR-01.1 | The list shows only the work items the user marked as favorite. |
| FR-01.2 | Columns: Add, Cust, Iter, No, Title, User/Role, Status, Fav. |
| FR-01.3 | The user clicks **Add to timesheet** on a row to add that work item (see CR-01, CR-02). |
| FR-01.4 | If the user has no favorites, the list is empty. **[TBC: empty message]** |

### FR-02 Add timelog from Task

**Entry:** Open the **TASKS** tab and use the **Advanced search** filter (default view).

| ID | Requirement |
|---|---|
| FR-02.1 | The list shows task work items (e.g. PREP-7421). |
| FR-02.2 | Columns: Add, Pri, Cust, Iter, No, Title, User/Role, Status, Est, Worked, Remain, Fav. |
| FR-02.3 | Filters: Customer (+ Inactive), Project (+ Closed), Iteration (+ Closed), Assigned to, Created by, Status. Buttons: **Apply**, **Clear all**. |
| FR-02.4 | Status filter default is 1 selected status (Work). |
| FR-02.5 | The user clicks **Add to timesheet** on a row to add that task (see CR-01, CR-02). |
| FR-02.6 | The user can also filter by tree view (**Filter by treeview**) and show **Overflow items**. **[TBC: detail]** |
| FR-02.7 | The No and Title links open the work item edit page. |

### FR-03 Add timelog from General Item

**Entry:** Open the **GENERAL ITEMS** tab.

| ID | Requirement |
|---|---|
| FR-03.1 | The list shows general work items (e.g. UI/UX, Dev PM, QA/QC Testing). |
| FR-03.2 | Columns: Add, Pri, Cust, Iter, No, Title, Status, Est, Worked, Remain, Fav. |
| FR-03.3 | The same filters as FR-02.3 are available. |
| FR-03.4 | The user clicks **Add to timesheet** on a row to add that item (see CR-01, CR-02). |
| FR-03.5 | The **Add general item** button creates a new general item. **[TBC: out of scope here, only the list is covered]** |

### FR-04 Add timelog from Company / internal

**Entry:** Click the **Company / internal** filter button.

| ID | Requirement |
|---|---|
| FR-04.1 | The list shows company-wide internal items (e.g. Scrum with US, Mail and reports, Meetings, General Office Operation, Training and Support, HR Tasks). |
| FR-04.2 | Columns: Add, Pri, Title, Status, Fav. There is no Customer, Iteration or No column. |
| FR-04.3 | Only the Status filter is available. |
| FR-04.4 | The user clicks **Add to timesheet** on a row to add that item (see CR-01, CR-02). |

### FR-05 Add previous work item

**Entry:** Click **Add work item from previous day** (plus icon) in the timesheet header.

| ID | Requirement |
|---|---|
| FR-05.1 | A **Previous Work Items** dialog opens with three tabs: **PREVIOUS DAY**, **PAST WEEK**, **PAST MONTH**. |
| FR-05.2 | Each tab lists the work items the user logged time on in that period. |
| FR-05.3 | Columns: checkbox, Cust, Iter, No, Work Item. The header checkbox selects all rows. |
| FR-05.4 | The user can filter the list by **Customer**. |
| FR-05.5 | The user selects one or more rows, then clicks **OK** to add them to the timesheet (see CR-01). |
| FR-05.6 | **Cancel** or the close (X) icon closes the dialog and adds nothing. |
| FR-05.7 | If a tab has no work items, the list is empty. **[TBC: empty message]** |
| FR-05.8 | If a selected work item is already on the timesheet, the system handles the duplicate. **[TBC: skip, merge or error]** |

## 6. Open Questions (to confirm with the dev team)

1. After **Add to timesheet**, does the system add the entry directly, or open a form to enter hours, start/end time and notes?
2. What are the default hours for a new entry?
3. Can the same work item be added twice on the same day?
4. Can the user add a timelog to a past or future date? Is there a limit?
5. What happens when the timesheet is finalized (CR-06)?
6. How does a user mark or unmark a favorite (the **Fav** column)?
7. Are there permission rules per role (Lead, Designer, Dev, QA, Admin)?
