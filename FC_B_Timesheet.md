# FC_B_Timesheet – Functional Test Cases (Black box): Timesheet (Sliders and Manual Entry)

**Methods:** Equivalence Partitioning (EP), Boundary Value (BV, part of EP), Decision Table Testing (DT), State Transition Testing (ST). Validation cases (valid / invalid / max length) are in section 4–6.
**Source:** Req_Analysis_Timesheet.md (PM3 Phase 1, 1b, 2, 3, 4). Items marked **[TBC]** depend on an open gap (G-xx) in that file.
**Actual result:** all cases are "Not executed" until run. Fill in the real behavior when executing.

## Common preconditions
| Code | Description |
|---|---|
| PC-1 | Logged in to PM3 as user `dev.user` (role Dev, one role "Dev"), Timekeeping page open in a maximized normal (non-incognito) browser, Sliders mode, selected date = a working day with **no** timesheet, Favorites tab has at least 3 work items: FAV-1 "TM-14 Post and News" (Task), FAV-2 "TM-15 Search Page" (Task), FAV-3 "INT-1 Meetings" (Company Internal). |
| PC-2 | PC-1 and the day has 3 rows, not finalized: R1 FAV-1 0800–0900, R2 FAV-2 0900–1000, R3 FAV-3 1000–1100, each with Role, Description, Remaining filled. |
| PC-3 | PC-2 and the day is finalized. |
| PC-4 | PC-2 but Manual entry mode selected. |
| PC-5 | Logged in as `admin.user` (role Admin). |
| PC-6 | Logged in as `devpm.user` (role Dev PM). Other active users exist: `dev.user`, `qa.user`, `admin.user`. |

Priority: H = High, M = Medium, L = Low. Is Automation: Y/N.

## 1. Date filter (EP)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_001 | Default date is today in mm/dd/yyyy | PC-1 | 1. Open Timekeeping page.<br>2. Read the date at the top right of the Timesheet pane. | – | Date equals today's date in format mm/dd/yyyy (e.g. 11/07/2026) and is displayed as a hyperlink. | Not executed | H | Y |
| FC_B_Timesheet_002 | Back button moves one day back | PC-2 | 1. Click [Back]. | Today = 11/07/2026 | Date becomes 11/06/2026. Grid reloads with rows of that day (empty if none). | Not executed | H | Y |
| FC_B_Timesheet_003 | Forward button moves one day forward | PC-1, date = 11/06/2026 | 1. Click [Forward]. | – | Date becomes 11/07/2026 and grid reloads for that day. | Not executed | H | Y |
| FC_B_Timesheet_004 | Pick a date from calendar | PC-1 | 1. Click the date link.<br>2. Select 10/15/2026 in the calendar. | 10/15/2026 | Calendar closes, date shows 10/15/2026, grid shows timesheet rows of that date. | Not executed | H | Y |
| FC_B_Timesheet_005 | Select a weekend date | PC-1 | 1. Pick Saturday 11/07/2026 – use the next Saturday from the calendar.<br>2. Add FAV-1 with the up-arrow icon. | A Saturday | Date is accepted; row is added to the Saturday timesheet. | Not executed | M | Y |
| FC_B_Timesheet_006 | Select a future date **[TBC G-12]** | PC-1 | 1. Pick a date 7 days in the future.<br>2. Click Add to timesheet on FAV-1. | Today + 7 | Behavior as defined by BA (allowed or blocked with message). Record actual behavior. | Not executed | M | N |

## 2. Add work item to timesheet from Favorites (EP, default times)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_010 | Add first item, no previous timesheet: default start 08:00 | PC-1, user has never logged time | 1. Hover the up-arrow icon of FAV-1 (tooltip).<br>2. Click it. | FAV-1 | Tooltip "Add to timesheet". New row "TM-14 Post and News" appears with start 0800, end 0830 (30 min), hours 0.50, Total 0.50. | Not executed | H | Y |
| FC_B_Timesheet_011 | Add first item, previous timesheet exists: start = first row start of previous timesheet | PC-1, previous timesheet's first row starts 0930 | 1. Click up-arrow on FAV-2. | FAV-2 | New row start = 0930, end = 1000. | Not executed | H | Y |
| FC_B_Timesheet_012 | Add second item: start = end of previous row | PC-2 | 1. Click up-arrow on FAV-1 again from Favorites. | FAV-1 | New row start = 1100, end = 1130. | Not executed | H | Y |
| FC_B_Timesheet_013 | Start after 21:00: default duration 15 minutes | Day has a row ending 2130 (not today) | 1. Click up-arrow on FAV-2. | – | New row start 2130, end 2145. | Not executed | M | Y |
| FC_B_Timesheet_014 | Start before 21:00 on a past day: default duration 30 minutes | Past day, last row ends 1400 | 1. Click up-arrow on FAV-2. | – | New row 1400–1430. | Not executed | M | Y |
| FC_B_Timesheet_015 | Today and start earlier than now: end = current time rounded to 15 min | Today, now = 10:07, last row ends 0900 | 1. Click up-arrow on FAV-2. | now 10:07 | New row start 0900, end 1000 (nearest 15 min to 10:07 is 10:00 – confirm rounding rule). | Not executed | M | N |
| FC_B_Timesheet_016 | Previous end later than 23:30: start becomes 23:30 | Last row ends 2400 | 1. Click up-arrow on FAV-2. | – | New row start = 2330. | Not executed | M | Y |
| FC_B_Timesheet_017 | Add button disabled on finalized day | PC-3 | 1. Hover/click the up-arrow icon on FAV-1. | – | Button is disabled; no row is added; Total unchanged. | Not executed | H | Y |
| FC_B_Timesheet_018 | Add same work item twice (duplicates) **[TBC]** | PC-2 | 1. Click up-arrow on FAV-1 (already on the sheet). | FAV-1 | A second row is added (copy-like behavior) or message – record actual. | Not executed | M | Y |
| FC_B_Timesheet_019 | Customer prefix on title (non Company Internal) | PC-1, FAV-1 belongs to customer Beltone (abbr BEL) | 1. Add FAV-1 and FAV-3. | FAV-1, FAV-3 | Row 1 title "BEL-TM-14  Post and News"; Company Internal row has no prefix, in both Sliders and Manual views. | Not executed | M | Y |
| FC_B_Timesheet_020 | Default sort: by start time then work item number | PC-1 | 1. Add FAV-2, then change its time to 0800–0830 using Manual or slider.<br>2. Add FAV-1 with 0800–0830. | Two rows same start | Both rows start 0800; FAV-1 (TM-14) is listed before FAV-2 (TM-15) after page reload. | Not executed | L | Y |

## 3. Sliders – time selection (EP / BV)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_030 | Drag start handle | PC-2 | 1. Click R1 slider.<br>2. Drag the left circle from 0800 to 0730. | 0730 | Start label updates live to 0730, hours 1.50, Total updates, value saved after focus lost. | Not executed | H | Y |
| FC_B_Timesheet_031 | Drag end handle | PC-2 | 1. Click R3 slider.<br>2. Drag the right circle from 1100 to 1145. | 1145 | End label 1145, hours 1.75, Total increases by 0.75. | Not executed | H | Y |
| FC_B_Timesheet_032 | Move the whole bar | PC-2 | 1. Mouse down on the bar of R1 and drag 1 hour right. | +1h | Start 0900, end 1000, hours stay 1.00. | Not executed | H | Y |
| FC_B_Timesheet_033 | Click left of slider sets new start | PC-2, R2 = 0900–1000 | 1. Click the slider track at 0800 (left of R2 bar). | 0800 | R2 start = 0800, end stays 1000, hours 2.00. | Not executed | M | Y |
| FC_B_Timesheet_034 | Click right of slider sets new end | PC-2, R2 = 0900–1000 | 1. Click the slider track at 1200. | 1200 | R2 end = 1200, start stays 0900, hours 3.00. | Not executed | M | Y |
| FC_B_Timesheet_035 | Slider snaps to 15-minute steps | PC-2 | 1. Drag R1 end handle slowly across 0900–0930 and release at about 0912. | ~0912 | Value snaps to 0900 or 0915, never to a non-15 value; labels show only 00/15/30/45. | Not executed | H | Y |
| FC_B_Timesheet_036 | Hour lines and quarter lines displayed | PC-1 | 1. Look at a slider track. | – | Vertical line at each hour; lighter lines for the 15-min intervals between hours; labels 6 to 23. | Not executed | L | N |
| FC_B_Timesheet_037 | Labels only on selected row | PC-2 | 1. Click R1.<br>2. Click R2. | – | Start/end label visible only on the selected row; R1 label hidden after R2 is selected. | Not executed | M | Y |
| FC_B_Timesheet_038 | Label uses 24-hour format | PC-2 | 1. Select R1 and set start 0300 and end 1745 (use Expand view). | 0300, 1745 | Labels "0300" and "1745" (4 digits, no colon). | Not executed | M | Y |
| FC_B_Timesheet_039 | Hours column: right aligned, 2 decimals, live | PC-2 | 1. Drag R1 end from 0900 to 0945. | – | Hours column shows 1.75 during drag and after release, right-justified, no heading. | Not executed | H | Y |
| FC_B_Timesheet_040 | Total hours equals sum of rows | PC-2 | 1. Read Total at the bottom. | R1+R2+R3 | Total = 3.00. | Not executed | H | Y |
| FC_B_Timesheet_041 | Autosave on losing focus, no re-sort | PC-2 | 1. Move R3 to 0700–0800.<br>2. Click outside the slider.<br>3. Observe grid order.<br>4. Reload page. | 0700–0800 | Value saved without a Save button; row order not changed immediately (R3 still last); after reload/Sort it is first. | Not executed | H | Y |
| FC_B_Timesheet_042 | Start before 05:00: left circle hidden | A row 0400–0600 exists; Expand view is on | 1. Select that row. | 0400–0600 | Left circle not shown; start cannot be changed alone; end handle and whole bar can be dragged. | Not executed | M | Y |
| FC_B_Timesheet_043 | Lower boundary: drag start to the minimum | PC-2 | 1. Drag R1 left handle to the far left. | – | Start stops at the minimum of the track (no lower than allowed min); no error. | Not executed | M | Y |
| FC_B_Timesheet_044 | Upper boundary: drag end to the maximum (24:00) | PC-2 | 1. Drag R3 right handle to the far right. | – | End stops at maximum 2400 and cannot go past it. | Not executed | M | Y |
| FC_B_Timesheet_045 | Select a row by work item name | PC-2 | 1. Click the work item name of R2. | – | Detail pane opens under R2, other rows are pushed down, background shaded. | Not executed | H | Y |
| FC_B_Timesheet_046 | Select a row by slider | PC-2 | 1. Click the slider of R3. | – | Detail pane opens under R3 with Role, Description, Remaining. | Not executed | M | Y |
| FC_B_Timesheet_047 | Expand to 24 hours | PC-2, Sliders mode | 1. Click the Expand toggle left of the first hour.<br>2. Scroll horizontally.<br>3. Click the Collapse toggle. | – | Default collapsed shows 6:00–midnight with no scrollbar. Expanded shows 0–23 with horizontal scrollbar (width unchanged), toggle icon changes to Collapse; collapse hides scrollbar. | Not executed | M | Y |
| FC_B_Timesheet_048 | Bar partly hidden before 6:00 in collapsed view | A row 0430–0700, collapsed view | 1. Select the row.<br>2. Drag the bar right by 1 hour. | – | Bar is displayed without the left circle; end adjustable; moving bar can make the start visible. A row 0400–0530 (fully hidden) is not displayed. | Not executed | L | N |

## 4. Validation – detail pane fields (valid / invalid / max length)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_050 | Role list excludes Admin | Logged in as user with roles Dev, QA, Admin | 1. Select a row.<br>2. Open Role dropdown. | – | Dropdown lists Dev and QA ordered by sort order; "Admin" not listed. | Not executed | H | Y |
| FC_B_Timesheet_051 | Single role: auto selected and disabled | PC-1 (only role Dev) | 1. Add FAV-1.<br>2. Select the row. | – | Role = Dev, dropdown disabled. | Not executed | M | Y |
| FC_B_Timesheet_052 | Multiple roles: default to primary role of the iteration | User with roles Dev, QA; primary role on iteration = QA | 1. Add a task of that iteration.<br>2. Select the row. | – | Role preselected = QA. | Not executed | M | Y |
| FC_B_Timesheet_053 | Role change is autosaved | PC-2, user has 2 roles | 1. Select R1.<br>2. Change Role from Dev to QA.<br>3. Reload page, select R1. | QA | Role = QA after reload without any Save click. | Not executed | H | Y |
| FC_B_Timesheet_054 | Description valid: 1 character | PC-2 | 1. Select R1.<br>2. Type "a" in Description.<br>3. Click outside. | "a" | Saved; no error. | Not executed | M | Y |
| FC_B_Timesheet_055 | Description valid: normal text | PC-2 | 1. Select R1, type text, click outside. | "Fix bug 123 in the news page" | Saved and shown again after reload. | Not executed | H | Y |
| FC_B_Timesheet_056 | Description max length 2000 | PC-2 | 1. Paste 2000 characters.<br>2. Click outside.<br>3. Reload and open R1. | 2000 chars ("a" x 2000) | All 2000 characters accepted and saved. | Not executed | H | Y |
| FC_B_Timesheet_057 | Description over max length 2001 | PC-2 | 1. Paste 2001 characters.<br>2. Click outside. | 2001 chars | Input is limited to 2000 characters (or error shown); 2001st character not saved. | Not executed | H | Y |
| FC_B_Timesheet_058 | Description empty is flagged on Finalize | PC-2, R1 description empty | 1. Click [Finalize]. | empty | Finalize cancelled; yellow round "!" next to R1 name; legend "Required data missing" shown. | Not executed | H | Y |
| FC_B_Timesheet_059 | Description with special characters and script | PC-2 | 1. Enter the text in Description, click outside, reload. | `<script>alert(1)</script> & "quotes" 'x' 日本語` | Text is saved and shown as plain text; no script runs. | Not executed | M | Y |
| FC_B_Timesheet_060 | Description multiline, scroll and resize | PC-2 | 1. Enter 10 lines.<br>2. Drag the textbox resize handle. | 10 lines | Default height shows 3 lines (Sliders); scrollbar appears when text exceeds; height adjustable. | Not executed | L | N |
| FC_B_Timesheet_061 | Remaining valid: 0 on non-"Work" status task | Task status Done/not Work, Show hours ON, row selected | 1. Enter 0, click outside. | 0 | Accepted and saved as 0.00. | Not executed | M | Y |
| FC_B_Timesheet_062 | Remaining valid minimum for status Work: 0.25 | Task status Work, Show hours ON | 1. Enter 0.25, click outside. | 0.25 | Accepted. | Not executed | H | Y |
| FC_B_Timesheet_063 | Remaining invalid: 0 for status Work | Task status Work | 1. Enter 0, click outside. | 0 | Value not accepted / corrected to minimum 0.25 [TBC message]. | Not executed | H | Y |
| FC_B_Timesheet_064 | Remaining valid maximum 9999.75 | Task, Show hours ON | 1. Enter 9999.75, click outside. | 9999.75 | Accepted, shown as 9999.75. | Not executed | H | Y |
| FC_B_Timesheet_065 | Remaining over maximum | Task, Show hours ON | 1. Enter 10000, click outside. | 10000 | Rejected with validation message; value not saved. | Not executed | H | Y |
| FC_B_Timesheet_066 | Remaining negative | Task, Show hours ON | 1. Enter -1, click outside. | -1 | Rejected; value not saved. | Not executed | H | Y |
| FC_B_Timesheet_067 | Remaining non numeric | Task, Show hours ON | 1. Enter "abc", click outside. | abc | Rejected (characters blocked or error message). | Not executed | M | Y |
| FC_B_Timesheet_068 | Remaining max characters (7) | Task, Show hours ON | 1. Type 12345678. | 12345678 | Not more than 7 characters accepted. | Not executed | M | Y |
| FC_B_Timesheet_069 | Remaining rounding to 15 minutes | Task, Show hours ON | 1. Enter 1.10 then click outside.<br>2. Enter 1.20, click outside. | 1.10, 1.20 | Values become 1.00 and 1.25. Two decimals displayed. | Not executed | M | Y |
| FC_B_Timesheet_070 | Remaining default = previous remaining – hours worked | Task with remaining 10.00, new row 1.00 h | 1. Add the task to the timesheet.<br>2. Select the row. | prev 10.00, worked 1.00 | Remaining shows 9.00. | Not executed | H | Y |
| FC_B_Timesheet_071 | Remaining hidden for non-task | PC-2, R3 = Company Internal | 1. Select R3. | – | Remaining field is not shown (also hidden for task with Show hours OFF). | Not executed | M | Y |

## 5. Manual entry mode (EP / BV)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_080 | Default entry mode is Sliders | New user, preferred method Sliders | 1. Open Timekeeping. | – | Radio "Sliders" is selected; sliders displayed. | Not executed | H | Y |
| FC_B_Timesheet_081 | Switch to Manual entry | PC-2 | 1. Select radio "Manual entry". | – | Sliders and hours grid hidden; each row shows Role, Start, End, Hours, Remaining, Description; names left-aligned; Copy button just left of Delete. | Not executed | H | Y |
| FC_B_Timesheet_082 | Selected mode is remembered | PC-2 | 1. Select "Manual entry".<br>2. Reload the page / navigate away and back / log out and log in. | – | Manual entry stays selected **[TBC G-09 for logout]**. | Not executed | H | Y |
| FC_B_Timesheet_083 | Start valid 4 digits | PC-4 | 1. In R1 Start enter 0900.<br>2. Tab out. | 0900 | Accepted, hours updated, autosaved. | Not executed | H | Y |
| FC_B_Timesheet_084 | Start 3 digits gets leading zero | PC-4 | 1. Enter 930 in Start.<br>2. Tab out. | 930 | Value becomes 0930. | Not executed | H | Y |
| FC_B_Timesheet_085 | Start fewer than 3 digits | PC-4 | 1. Enter 09 in Start.<br>2. Tab out. | 09 | Not accepted (min 3 digits): inline error "Time invalid" **[TBC message]**; not saved. | Not executed | M | Y |
| FC_B_Timesheet_086 | Start more than 4 digits | PC-4 | 1. Type 09301 in Start. | 09301 | Fifth digit is not accepted; field holds 0930. | Not executed | M | Y |
| FC_B_Timesheet_087 | Start hour 24 invalid | PC-4 | 1. Enter 2400 in Start.<br>2. Tab out. | 2400 | Inline message "Time invalid"; value not saved. | Not executed | H | Y |
| FC_B_Timesheet_088 | Start hour 23 valid (upper boundary) | PC-4 | 1. Enter 2330 in Start, tab out. | 2330 | Accepted (End must be later or "Precedes start" appears). | Not executed | M | Y |
| FC_B_Timesheet_089 | Start 0000 valid (lower boundary) | PC-4 | 1. Enter 0000 in Start, tab out. | 0000 | Accepted. | Not executed | M | Y |
| FC_B_Timesheet_090 | Non numeric characters | PC-4 | 1. Type "ab:c" in Start. | ab:c | Letters/colon are blocked or "Time invalid" shown. | Not executed | M | Y |
| FC_B_Timesheet_091 | Rounding 1158 → 1200 | PC-4 | 1. Enter 1158 in Start, tab out. | 1158 | Value becomes 1200. | Not executed | H | Y |
| FC_B_Timesheet_092 | Rounding boundary: 1107 → 1100, 1108 → 1115 | PC-4 | 1. Enter 1107 in Start, tab out.<br>2. Enter 1108, tab out. | 1107, 1108 | 1100 and 1115 (closest 15-min interval). | Not executed | M | Y |
| FC_B_Timesheet_093 | Rounding near end of day 2353 **[TBC G-04]** | PC-4 | 1. Enter 2353 in End, tab out. | 2353 | Result as defined by BA (2345, 2400 or error). | Not executed | L | N |
| FC_B_Timesheet_094 | End valid, hours recalculated | PC-4 | 1. R1: Start 0800, End 1015, tab out. | 0800, 1015 | Hours label shows 2.25; Total updated; autosaved. | Not executed | H | Y |
| FC_B_Timesheet_095 | End earlier than Start | PC-4 | 1. R1: Start 1000, End 0900, tab out. | 1000, 0900 | Inline message "Precedes start" under End; not saved. | Not executed | H | Y |
| FC_B_Timesheet_096 | End equal to Start **[TBC G-05]** | PC-4 | 1. R1: Start 1000, End 1000, tab out. | 1000, 1000 | Behavior defined by BA (hours 0.00 accepted or error). | Not executed | M | N |
| FC_B_Timesheet_097 | End hour 24 invalid | PC-4 | 1. Enter 2400 in End. | 2400 | "Time invalid". | Not executed | M | Y |
| FC_B_Timesheet_098 | Invalid time is not autosaved | PC-4 | 1. Enter 2500 in Start, tab out.<br>2. Reload page. | 2500 | Old Start value remains after reload. | Not executed | H | Y |
| FC_B_Timesheet_099 | Autosave on blur of each field in Manual | PC-4 | 1. Change Role, Start, End, Remaining, Description in R1 one by one, clicking outside each time.<br>2. Reload. | any valid | Each value persisted without clicking Save. | Not executed | H | Y |
| FC_B_Timesheet_100 | Auto re-sort in Manual: Sort, Add from Previous, Finalize, date change, add item | PC-4, R3 start changed to 0700 | 1. Click [Sort].<br>2. Change R2 start to 0600, then click [Finalize] (fails validation).<br>3. Change date and come back. | – | After each action rows are sorted by start time; no re-sort happens while only editing a field. | Not executed | M | Y |
| FC_B_Timesheet_101 | Manual description default height 2 lines | PC-4 | 1. Look at the Description box. | – | Shows 2 lines by default; multiline. | Not executed | L | N |
| FC_B_Timesheet_102 | Preferred method "Manual entry" applied on load | `dev.user` profile has Preferred time logging method = Manual entry | 1. Log out and log in.<br>2. Open Timekeeping. | – | Page loads in Manual entry mode. | Not executed | H | Y |
| FC_B_Timesheet_103 | Preferred time logging method field on My Account | PC-1 | 1. Open User menu > My Account.<br>2. Open the dropdown. | – | Dropdown has exactly "Manual entry" and "Sliders" (no blank); editable; save persists. | Not executed | M | Y |

## 6. Copy, Delete, Sort, Download
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_110 | Copy a row | PC-2 | 1. Hover Copy icon on R2 (tooltip).<br>2. Click Copy. | R2 | Tooltip "Add this work item again". New row of FAV-2 with start = end of previous detail, default length; Total increases. | Not executed | H | Y |
| FC_B_Timesheet_111 | Copy when previous end is after 23:30 | Last row ends 2400 | 1. Click Copy. | – | New row start = 2330. | Not executed | L | Y |
| FC_B_Timesheet_112 | Delete a row (not the last one) | PC-2 | 1. Hover trash icon of R2 (tooltip "Delete time log entry").<br>2. Click it. | R2 | R2 removed, R1 and R3 stay, Total = 2.00. | Not executed | H | Y |
| FC_B_Timesheet_113 | Delete the last remaining row | Day has only R1 | 1. Click trash on R1.<br>2. Reload page. | R1 | Row removed, Total 0.00, empty list; the day behaves as no timesheet. | Not executed | H | Y |
| FC_B_Timesheet_114 | Sort re-orders by start, end, work item no | PC-2, R3 changed to 0700 | 1. Hover Sort icon (tooltip "Sort by start time").<br>2. Click it. | – | R3 moves to the first position; ties ordered by end then number. | Not executed | M | Y |
| FC_B_Timesheet_115 | Download CSV | PC-3 or PC-2 as `Dev User` | 1. Hover Download icon (tooltip "Download as csv").<br>2. Click it. | – | File "Timesheet – Dev User – yyyymmdd.csv" is downloaded with columns Work item #, Work item, Description, Duration ordered by start, end, no; last line shows total hours. | Not executed | H | N |
| FC_B_Timesheet_116 | CSV content with comma, quote and long text **[TBC G-15]** | A row with description `a,"b",c` | 1. Download CSV and open in Excel. | `a,"b",c` | Value stays in one cell, correctly escaped. | Not executed | L | N |
| FC_B_Timesheet_117 | Download on a day without rows **[TBC G-15]** | PC-1 | 1. Click Download. | – | Button disabled or CSV only with header/total 0 – record actual behavior. | Not executed | L | N |

## 7. Finalize – Decision Table (DT)
Conditions: C1 at least one row; C2 required data complete (description, role, remaining when visible); C3 no overlapping rows; C4 user is Admin.

| Rule | C1 | C2 | C3 | C4 | Expected outcome |
|---|---|---|---|---|---|
| R1 | N | – | – | N | Prompt "Nothing to finalize", not finalized |
| R2 | N | – | – | Y | **[TBC G-02]** Admin can finalize an empty day |
| R3 | Y | Y | Y | any | Finalized, page read-only |
| R4 | Y | N | Y | any | Not finalized; yellow round "!" on the rows with missing data; legend |
| R5 | Y | Y | N | any | Not finalized; red triangle "!" on both overlapping rows; legend |
| R6 | Y | N | N | any | Not finalized; both icons and both legend lines |

| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_120 | DT R1: finalize with no rows (non admin) | PC-1 | 1. Click [Finalize]. | – | Prompt "Nothing to finalize"; day stays unfinalized. | Not executed | H | Y |
| FC_B_Timesheet_121 | DT R2: Admin finalizes an empty day | PC-5, empty day | 1. Click [Finalize]. | – | **[TBC]** Day is finalized (per Phase 1 admin rule) or prompt – record actual. | Not executed | M | N |
| FC_B_Timesheet_122 | DT R3: valid finalize | PC-2 | 1. Click [Finalize]. | – | No error, page changes to read-only, button caption "Unfinalize". | Not executed | H | Y |
| FC_B_Timesheet_123 | DT R4: missing description/role/remaining | PC-2, R1 description empty, R2 remaining empty (Task) | 1. Click [Finalize]. | – | Finalize cancelled; yellow round "!" left of R1 and R2 names; legend under the grid "Required data missing". | Not executed | H | Y |
| FC_B_Timesheet_124 | DT R5: overlapping rows | PC-2, then set R2 = 0830–0930 (overlaps R1 0800–0900 and R3 stays 1000–1100) | 1. Click [Finalize]. | R1 0800–0900, R2 0830–0930 | Finalize cancelled; red triangle "!" above the sliders of R1 and R2 (not R3); legend "Times overlapping". | Not executed | H | Y |
| FC_B_Timesheet_125 | DT R6: both problems | PC-2 with both situations | 1. Click [Finalize]. | – | Both icon types shown, legend lists both messages. | Not executed | M | Y |
| FC_B_Timesheet_126 | Legend hides after issues are fixed | After FC_B_Timesheet_123 | 1. Fill all missing data.<br>2. Observe legend. | – | Icons disappear and legend is hidden when no issue remains. | Not executed | M | Y |
| FC_B_Timesheet_127 | Touching rows are not overlapping **[TBC G-06]** | PC-2 (0800–0900, 0900–1000) | 1. Click [Finalize]. | – | No overlap icon, finalize succeeds. | Not executed | H | Y |
| FC_B_Timesheet_128 | Validation icons when loading a previous unfinalized day | A past day with a row missing description | 1. Select that past day. | – | Yellow "!" and legend are shown right after loading, without clicking Finalize. | Not executed | M | Y |
| FC_B_Timesheet_129 | Previous day not finalized no longer blocks logging | Yesterday has an unfinalized timesheet, today empty | 1. Add FAV-1 for today. | – | Row is added with no prompt/warning (Phase 3). | Not executed | H | Y |
| FC_B_Timesheet_130 | Required data missing, Remaining hidden → not flagged **[TBC G-19]** | R3 Company Internal with role+description filled, no Remaining field | 1. Click [Finalize]. | – | R3 not flagged. | Not executed | M | Y |

## 8. Finalized state – State Transition (ST)
States: **S1 Unfinalized (editable)**, **S2 Finalized (read-only)**.
Transitions: S1 → S2 (Finalize, valid), S1 → S1 (Finalize, invalid), S2 → S1 (Unfinalize), S2 → S2 (any edit attempt).

| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_140 | S1→S2: UI becomes read-only | PC-2 | 1. Click [Finalize].<br>2. Inspect the page. | – | Visual indicator that the day is finalized; Sort, Delete, Copy hidden; sliders cannot be moved; labels still shown. | Not executed | H | Y |
| FC_B_Timesheet_141 | S2: row can be selected, fields disabled | PC-3 | 1. Click R1 name. | – | Detail pane opens; Role, Description, Remaining are disabled. | Not executed | H | Y |
| FC_B_Timesheet_142 | S2: sliders cannot be moved | PC-3 | 1. Try to drag R1 handle and bar. | – | No change of times. | Not executed | H | Y |
| FC_B_Timesheet_143 | S2: Manual mode fields disabled | PC-3, Manual mode | 1. Try to type in Start/End/Description. | – | Fields are read-only/disabled. | Not executed | M | Y |
| FC_B_Timesheet_144 | S2: Add from Previous is not available | PC-3 | 1. Look for the "Add work item from previous day" icon. | – | Icon hidden/disabled when the sheet is not editable. | Not executed | M | Y |
| FC_B_Timesheet_145 | S2→S1: Unfinalize own day | PC-3 | 1. Click [Unfinalize]. | – | Day returns to editable; Sort/Copy/Delete visible; button caption "Finalize"; data unchanged. | Not executed | H | Y |
| FC_B_Timesheet_146 | S1→S1: invalid finalize leaves state | PC-2 with missing data | 1. Click [Finalize].<br>2. Reload page. | – | Day remains editable (not finalized). | Not executed | H | Y |
| FC_B_Timesheet_147 | S2 for another employee without rights: no Finalize/Unfinalize button | PC-6 sees employee `qa.user`'s finalized day as a user without tier 1 rights **[TBC G-13]** | 1. Select `qa.user`.<br>2. Look at the toolbar. | – | Finalize/Unfinalize button hidden when selected employee ≠ current user and user not tier 1. | Not executed | M | N |
| FC_B_Timesheet_148 | Finalize then re-finalize cycle | PC-2 | 1. Finalize.<br>2. Unfinalize.<br>3. Edit R1 to 0800–0830.<br>4. Finalize again. | – | Each transition works; final state S2 with the edited value. | Not executed | M | Y |
| FC_B_Timesheet_149 | Finalized day cannot receive new time from Favorites | PC-3 | 1. Click the up-arrow in Favorites. | – | Up-arrow disabled (see 017). | Not executed | H | Y |

## 9. Employee selector – Decision Table
| Rule | Role of current user | Expected control |
|---|---|---|
| R1 | Admin | Dropdown, current user first, lists users with highest rank lower than own (and self) |
| R2 | Client PM | Dropdown, lists users with lower rank |
| R3 | Dev PM | Dropdown, lists users with lower rank |
| R4 | Lead / Designer / Dev / QA | Label "Timesheet for: {display_name}" (no dropdown) **[TBC: QA not listed in P3]** |

| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_160 | Dev sees a label, not a dropdown | PC-1 | 1. Look at the Timesheet header. | – | Text "Timesheet for: Dev User"; no dropdown. | Not executed | H | Y |
| FC_B_Timesheet_161 | Dev PM sees dropdown with allowed users | PC-6 | 1. Open the employee dropdown. | – | Current user first; others sorted by display name; only users with lower rank (Dev, QA, Lead...), not Admin. | Not executed | H | Y |
| FC_B_Timesheet_162 | Admin sees dropdown with users | PC-5 | 1. Open the dropdown. | – | All allowed users (lower rank than Admin) are listed. | Not executed | M | Y |
| FC_B_Timesheet_163 | Selecting another employee: grid, highlight, favorites | PC-6 | 1. Select `dev.user`. | dev.user | Grid shows dev.user's entries; selection highlighted yellow; Favorites list shows dev.user's favorites. | Not executed | H | Y |
| FC_B_Timesheet_164 | Time added for the selected employee | PC-6, `dev.user` selected | 1. Click up-arrow on a favorite.<br>2. Switch employee to self. | – | The row is on `dev.user`'s timesheet, not on the Dev PM's. | Not executed | H | Y |
| FC_B_Timesheet_165 | Dev PM can edit, delete and finalize for selected employee | PC-6, `dev.user` selected with rows | 1. Edit a slider.<br>2. Delete a row.<br>3. Finalize. | – | All actions succeed for the selected employee. | Not executed | M | Y |

## 10. Add from Previous (EP / BV)
| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_170 | Open the dialog | PC-1, finalized timesheets exist | 1. Hover the plus icon (tooltip "Add work item from previous day").<br>2. Click it. | – | Dialog "Previous Work Items" with tabs PREVIOUS DAY (default), PAST WEEK, PAST MONTH; grid columns: checkbox, Customer/Org, Iter, No, Work Item (not sortable); OK disabled. | Not executed | H | Y |
| FC_B_Timesheet_171 | Previous Day tab lists the most recent finalized day only | Finalized: 11/04 (A,B), 11/05 (C,D); today 11/06 | 1. Open dialog. | – | Rows C and D only, ordered by customer, iteration, number. | Not executed | H | Y |
| FC_B_Timesheet_172 | Unfinalized days are excluded | Day 11/05 unfinalized with item E | 1. Open dialog, check all tabs. | – | Item E is not listed. | Not executed | H | Y |
| FC_B_Timesheet_173 | Past Week includes last 7 days; 8th day excluded | Finalized items at today-7 (X) and today-8 (Y) | 1. Open PAST WEEK. | – | X listed, Y not listed **[TBC boundary "including today"]**. | Not executed | M | Y |
| FC_B_Timesheet_174 | Past Month includes last 30 days; 31st excluded | Items at today-30 (X) and today-31 (Y) | 1. Open PAST MONTH. | – | X listed, Y not listed. | Not executed | M | Y |
| FC_B_Timesheet_175 | Select one row and OK | PC-1 | 1. Check one row.<br>2. Click [OK]. | 1 row | Dialog closes; new row added on selected date with default times; grid and Total refreshed. | Not executed | H | Y |
| FC_B_Timesheet_176 | Select several rows and OK | PC-1 | 1. Check 3 rows.<br>2. Click [OK]. | 3 rows | 3 rows added, consecutive; each defaults to start + 30 min (15 min after 21:00); first row start follows rule of FC_B_Timesheet_011. | Not executed | H | Y |
| FC_B_Timesheet_177 | Select all / none | PC-1 | 1. Click header checkbox.<br>2. Click it again. | – | All rows checked, then all unchecked; OK enabled only when ≥1 row is checked. | Not executed | M | Y |
| FC_B_Timesheet_178 | Filter by customer | Items of 2 customers | 1. Choose a customer in the filter. | Customer A | Only rows of customer A shown; customer list contains only customers in the grid, sorted by name. | Not executed | M | Y |
| FC_B_Timesheet_179 | Cancel adds nothing | PC-1 | 1. Check a row.<br>2. Click [Cancel]. | – | Dialog closes; no new row. | Not executed | M | Y |
| FC_B_Timesheet_180 | Close (X) adds nothing | PC-1 | 1. Check a row.<br>2. Click X. | – | Dialog closes; no new row. | Not executed | L | Y |
| FC_B_Timesheet_181 | Item already on the timesheet **[TBC]** | PC-2 | 1. Open dialog; check FAV-1 which is on the sheet.<br>2. Click [OK]. | FAV-1 | Behavior per BA (duplicate row added / skipped / message). | Not executed | M | N |
| FC_B_Timesheet_182 | Empty tab | User without history in PAST MONTH | 1. Open PAST MONTH. | – | Empty grid, OK disabled **[TBC message]**. | Not executed | L | Y |
| FC_B_Timesheet_183 | Employee selector respected | PC-6, `dev.user` selected | 1. Open dialog. | – | List contains `dev.user`'s history, not the Dev PM's. | Not executed | M | Y |

## 11. Overflow checkbox – Decision Table
Conditions: work item overflow_type (0 Standard, 1 Overflow, 2 Both); user "Allow Overflow" on the iteration; row added from Overflow list or Standard list.

| Testcase ID | Test case name | Precondition | Steps | Data input | Expected result | Actual result | Priority | Is Automation |
|---|---|---|---|---|---|---|---|---|
| FC_B_Timesheet_190 | Type 0: checkbox hidden, saved as standard | Work item overflow_type = 0 | 1. Add the item.<br>2. Select the row. | type 0 | [Overflow] checkbox hidden; is_overflow saved = 0; no plum highlight. | Not executed | H | Y |
| FC_B_Timesheet_191 | Type 1: checkbox hidden/disabled, saved as overflow | overflow_type = 1 | 1. Add the item from the Overflow section.<br>2. Select the row. | type 1 | Item has plum highlight; overflow saved = 1; checkbox not changeable. | Not executed | H | Y |
| FC_B_Timesheet_192 | Type 2 added from Standard list: unchecked | overflow_type = 2, Allow Overflow checked | 1. Add from Advanced Search.<br>2. Select the row. | type 2 | Checkbox shown, unchecked. | Not executed | H | Y |
| FC_B_Timesheet_193 | Type 2 added from Overflow list: checked | overflow_type = 2 | 1. Add from Overflow section. | type 2 | Checkbox shown, checked; title highlighted plum. | Not executed | H | Y |
| FC_B_Timesheet_194 | Type 2 but user not allowed overflow | overflow_type = 2, assignment "Allow Overflow" unchecked | 1. Add the item.<br>2. Select the row. | – | Checkbox hidden; value saved unchecked. | Not executed | H | Y |
| FC_B_Timesheet_195 | Type 2: toggling the checkbox is saved | overflow_type = 2, allowed | 1. Check the box, click outside.<br>2. Reload. | – | Checkbox stays checked; row highlighted plum. | Not executed | M | Y |
| FC_B_Timesheet_196 | Copy keeps overflow state | Row with overflow checked | 1. Click Copy. | – | New row has the same checked/visible state. | Not executed | M | Y |
| FC_B_Timesheet_197 | Add from Previous: same item as standard and overflow | Item logged both as standard and overflow in history | 1. Open dialog. | – | Two rows for the item: standard first, overflow second with plum highlight and icon. | Not executed | M | Y |
| FC_B_Timesheet_198 | Mismatch: type 0 item with is_overflow = 1 (after Fix Timesheet Data) | Data prepared by DB/Fix page | 1. Open the day, select the row. | – | Checkbox shown checked and disabled; title highlighted plum. | Not executed | L | N |
