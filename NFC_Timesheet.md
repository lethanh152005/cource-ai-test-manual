# NFC_Timesheet – Non-functional Test Cases: Timesheet (Sliders and Manual Entry)

**Source:** Req_Analysis_Timesheet.md. The requirement documents give **no numeric targets** (gap G-17), so the targets below are assumptions marked **[Assumed]**; confirm with the team.
**Actual result:** "Not executed" until run.

## Common preconditions
| Code | Description |
|---|---|
| PC-1 | Logged in as `dev.user` (role Dev), Timekeeping page, normal maximized browser (Chrome latest unless stated), Sliders mode, test environment with normal data. |
| PC-2 | PC-1 and the day has 3 rows 0800–1100, not finalized. |

| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| NFC_Timesheet_001 | Page load time with a normal day | PC-2 | 1. Open DevTools > Network (disable cache).<br>2. Load the Timekeeping page 5 times.<br>3. Note the time until the grid and Total are shown. | 3 rows | Average load ≤ 3 s **[Assumed]**. | Not executed | M | Y |
| NFC_Timesheet_002 | Load time with many rows | Day with 50 rows | 1. Load the day.<br>2. Note time and UI responsiveness. | 50 rows | Loads ≤ 5 s **[Assumed]**; scrolling smooth; Total correct. | Not executed | M | Y |
| NFC_Timesheet_003 | Slider drag responsiveness | PC-2 | 1. Drag a handle continuously for 10 s.<br>2. Observe labels, hours and Total. | – | Labels, hours and Total update in real time with no visible lag (> 100 ms delay not noticeable) **[Assumed]**. | Not executed | M | N |
| NFC_Timesheet_004 | Autosave latency | PC-2 | 1. Change a value and blur.<br>2. Measure time until the save request returns. | – | Save request completes ≤ 1 s **[Assumed]**; no UI freeze. | Not executed | M | Y |
| NFC_Timesheet_005 | Add to timesheet response time | PC-1 | 1. Click the up-arrow and measure time until the new row appears. | – | ≤ 2 s **[Assumed]**. | Not executed | M | Y |
| NFC_Timesheet_006 | Add from Previous with large history | Past Month with 200 distinct work items | 1. Open dialog, PAST MONTH.<br>2. Select all and click OK. | 200 items | Dialog loads ≤ 3 s; OK adds all rows without error **[Assumed]**; grid renders. | Not executed | L | N |
| NFC_Timesheet_007 | Multi-user load | 50 concurrent users add and edit rows (JMeter/k6) | 1. Run the script for 10 minutes. | 50 users | Error rate < 1 %, 95th percentile response ≤ 2 s **[Assumed]**; no data mix-up between users. | Not executed | M | Y |
| NFC_Timesheet_008 | Usability – tooltips and labels | PC-2 | 1. Hover each icon: up-arrow, trash, copy, sort, download, plus, expand/collapse, overflow. | – | Texts: "Add to timesheet", "Delete time log entry", "Add this work item again", "Sort by start time", "Download as csv", "Add work item from previous day", "Overflow". | Not executed | M | Y |
| NFC_Timesheet_009 | Usability – error messages are clear | PC-2 Manual mode | 1. Trigger "Time invalid", "Precedes start", "Nothing to finalize", legend texts. | – | Messages are in English, next to the field, readable, and disappear when fixed. | Not executed | M | Y |
| NFC_Timesheet_010 | Keyboard accessibility | PC-2, Manual mode | 1. Use Tab/Shift+Tab through Role, Start, End, Remaining, Description, Copy, Delete.<br>2. Use Enter/Space on buttons. | – | Logical order, visible focus, every control usable without mouse. Slider keyboard support (arrow keys) is recorded **[TBC]**. | Not executed | M | N |
| NFC_Timesheet_011 | Accessibility – labels and contrast | PC-2 | 1. Run axe / Lighthouse accessibility audit.<br>2. Check validation icon color contrast and that icons have text alternatives. | – | No critical issues; icons not color-only (legend explains them). | Not executed | L | Y |
| NFC_Timesheet_012 | Browser compatibility | PC-2 | 1. Repeat add, slider drag, manual entry, finalize on Chrome, Edge, Firefox, Safari (latest). | – | Same behavior and layout in all browsers; drag and click-on-track work. | Not executed | H | N |
| NFC_Timesheet_013 | Responsive layout | PC-2 | 1. Resize to 1920x1080, 1366x768, tablet 768x1024. | – | No overlapping controls; slider remains usable at 1366; tablet behavior recorded (horizontal scroll acceptable). | Not executed | L | N |
| NFC_Timesheet_014 | Browser zoom 80% – 150% | PC-2 | 1. Set browser zoom to 80 %, 125 %, 150 %.<br>2. Drag slider and click on track. | – | Click/drag position still maps to the right time (no offset). | Not executed | L | N |
| NFC_Timesheet_015 | Date and number format | PC-2 | 1. Check date, hours and times. | – | Date mm/dd/yyyy; hours 2 decimals; times 4 digits 24h; consistent in grid, dialog and CSV. | Not executed | M | Y |
| NFC_Timesheet_016 | Security – access without login | – | 1. Open the Timekeeping URL and the timesheet API in a new session without login. | – | Redirect to Keycloak login / 401; no data returned. | Not executed | H | Y |
| NFC_Timesheet_017 | Security – horizontal privilege escalation | PC-1 | 1. Replace user id/timesheet id in API calls with another user's id (see FC_G_Timesheet_025). | – | 403/404; no data of other users shown or changed. | Not executed | H | Y |
| NFC_Timesheet_018 | Security – XSS in Description and shown elsewhere | PC-2 | 1. Save `<img src=x onerror=alert(1)>` in Description.<br>2. View it in the grid, the CSV opened in Excel, and reports. | payload | No script executes; CSV does not execute formulas (text starting with `=`, `+`, `-`, `@` is neutralized) **[TBC]**. | Not executed | H | Y |
| NFC_Timesheet_019 | Security – CSRF and tampered requests | PC-2 | 1. Send a save request without the CSRF token / auth header from another origin. | – | Request rejected. | Not executed | M | Y |
| NFC_Timesheet_020 | Reliability – refresh during autosave | PC-2 | 1. Change Description, press F5 immediately before blur.<br>2. Repeat with blur then immediate F5. | – | No corrupted data; the value is either fully saved or not saved; no duplicate rows. | Not executed | M | N |
| NFC_Timesheet_021 | Reliability – double click on add / copy | PC-2 | 1. Double-click quickly on the up-arrow and on Copy. | – | Only one row added per intended click (no accidental duplicates) **[TBC]**. | Not executed | M | N |
| NFC_Timesheet_022 | Reliability – network drop | PC-2 | 1. Go offline, change a slider, go online (see FC_G_Timesheet_041). | – | No crash; clear feedback; data consistent after reload. | Not executed | M | N |
| NFC_Timesheet_023 | Data integrity – totals under many edits | PC-2 | 1. Perform 50 random edits (slider, manual, copy, delete).<br>2. Compare Total with the DB sum. | – | Total always equals the sum of rows; no rounding drift. | Not executed | M | Y |
| NFC_Timesheet_024 | Compatibility – mode switch retains data | PC-2 | 1. Switch Sliders → Manual → Sliders 5 times. | – | Rows, times and descriptions identical; no duplicate saves. | Not executed | M | Y |
| NFC_Timesheet_025 | Session / idle timeout | PC-2 | 1. Stay idle until the session expires.<br>2. Try an edit. | – | User is asked to log in again; no silent failure. | Not executed | L | N |
