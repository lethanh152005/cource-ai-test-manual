# Requirement Analysis – Timesheet (Sliders and Manual Entry)

**Sources (D:\Testing\PM3-AI\all-iteration):** Phase 1 v1.5 (sliders, finalize, favorites), Phase 1b v1.5 (manual entry, add from previous), Phase 2 v1.4 (preferred logging method), Phase 3 v3.1 (roles/remaining/default times, finalize restriction removed, employee selector by rank, overflow), Phase 4 v1.6 (customer prefix in title, overflow rules, expand 24h, mismatch cases).
Later phases override earlier ones where they conflict (see Gaps).

## 1. Actionable features (what must be built / tested)

| ID | Feature | Source |
|---|---|---|
| T-01 | Date filter (mm/dd/yyyy, calendar link, Back/Forward one day) | P1 |
| T-02 | List of timesheet detail rows, default sort by start time then work item no; work item shown as `{no} {name}`, right-justified (left-justified in Manual) | P1, P1b |
| T-03 | Slider: drag start / end / whole bar, click left sets start, click right sets end, 15-min steps, hour lines + lighter 15-min lines, range 5–24 (labels 6–23) | P1 |
| T-04 | Selected row shows start/end label in 24h format (e.g. 0325, 1745), live update; hidden on unselected/finalized rows | P1 |
| T-05 | Autosave on blur, no auto re-sort after autosave; hours column (end – start, 2 decimals, live); total hours at bottom | P1 |
| T-06 | Start before 05:00: no left circle, start cannot be changed alone, end and whole bar can be moved | P1 |
| T-07 | Detail pane (row click): Role dropdown, Description (max 2000), Remaining (0–9999.99 in P1; 0–9999.75, 15-min rounding in P3) – each required when visible | P1, P3 |
| T-08 | Delete (trash) and Copy ("Add this work item again") per row | P1 |
| T-09 | Toolbar: Sort, Download CSV, Add from Previous, Finalize/Unfinalize | P1, P1b |
| T-10 | Finalize validation: nothing to finalize, required data missing (yellow round !), overlap (red triangle !), legend | P1 |
| T-11 | Finalized = read-only (hide Sort/Delete/Copy, disable fields and sliders, rows still selectable) | P1 |
| T-12 | Employee selector (Admin, Dev PM, Client PM in P1; rank based in P3), yellow highlight, favorites reload | P1, P3 |
| T-13 | Add to timesheet from Favorites (default times) | P1, P3 |
| T-14 | Add from Previous dialog (Previous Day / Past Week / Past Month, org filter, select all, OK/Cancel) | P1b, P3 |
| T-15 | Manual entry mode: radio Sliders/Manual, remembered; Role, Start, End, Hours, Remaining, Description; time rules, rounding, autosave, auto re-sort | P1b |
| T-16 | Preferred time logging method on Edit User / My Account drives mode on page load | P2 |
| T-17 | Role default rules, Remaining default = previous remaining – hours worked, update of task remaining only for most recent detail | P3 |
| T-18 | Default time rules (first row = first row of previous timesheet or 08:00; next = previous end; +30 min, or +15 min after 21:00; today uses current time) | P3 |
| T-19 | Overflow checkbox rules (overflow_type 0/1/2, Allow Overflow, mismatch cases, plum highlight) | P3, P4 |
| T-20 | Customer abbreviation prefix on work item title (not for Company Internal) | P4 |
| T-21 | Expand/Collapse 24h toggle in Sliders view | P4 |
| T-22 | Validation indicators are also run when loading any previous day | P1 |
| T-23 | Finalize restriction on previous day removed for all users; finalized day still blocks new time | P3 |

## 2. Constraints
- Time granularity 15 minutes (slider, manual rounding, remaining).
- Description ≤ 2000 chars; Remaining ≤ 7 chars, 2 decimals; manual time = 3–4 digits.
- Role list from `user_role`, Admin excluded.
- Employee selector shown only for allowed roles; list limited by role rank.
- Page is read-only when `timesheet.is_finalized` is true.

## 3. Acceptance criteria (summary)
1. Any valid add (Favorites / Previous) creates a timesheet (if missing) and a detail row with the default times, and the grid and Total refresh.
2. Moving a slider or entering manual times always results in 15-min values, a correct hours value, a correct Total, and is auto-saved.
3. Finalize succeeds only when there is ≥1 row, no missing required data and no overlap; otherwise it is cancelled with icons and legend.
4. A finalized day cannot be changed (add, edit, copy, delete, sort) until unfinalized by an allowed user.
5. Manual entry validates Start/End ("Time invalid", "Precedes start") and rounds to the closest 15 min.

## 4. Gaps and ambiguities (confirm with BA / Dev)

| # | Area | Gap / ambiguity | Impact on test |
|---|---|---|---|
| G-01 | Unfinalize | P1 says unfinalize sets `time_worked_day.is_finalized` to **true** (typo?) and uses a different table name than `timesheet`. | Expected = false; confirm. |
| G-02 | Finalize with no rows | "Nothing to finalize" prompt for everyone vs "Admins can finalize a day without logging time". | Admin no-row case is TBC. |
| G-03 | Slider range | Min/max 5–24 with labels 6–23 (P1); "no left circle if start < 5:00" (P1) vs hidden before 6:00 (P4); expanded view 0–23 (P4). | Which hour is the true lower bound? |
| G-04 | Manual end value | Hour part allowed 0–23, so 2400 cannot be typed, but slider allows 24:00. Rounding 2353–2359 → 2400? | Boundary result TBC. |
| G-05 | Start = End | Not defined (0 hours). "Precedes start" only when End < Start. | TBC. |
| G-06 | Overlap definition | Touching ranges (0900–1000 and 1000–1100) overlapping or not? | TBC; assumed not overlapping. |
| G-07 | Remaining hours | P1: 0–9999.99, any 2 decimals; P3: max 9999.75, 15-min rounding, min 0.25 for status "Work", shown only for Task with "Show hours" ON. Manual mode "equivalent". | Use P3; confirm. |
| G-08 | Role ordering | `role.priority` (P1) vs `role.sort_order` (P3). | Use sort_order; confirm. |
| G-09 | Mode persistence | P1b: remember in session variable and persist as default; P2: user profile "Preferred time logging method" applied on page load. Does the radio change update the profile? | Order of precedence TBC. |
| G-10 | Default times | "Previous timesheet detail" = previous in list order or previous by time? Copy end time not defined when start = 23:30. | TBC. |
| G-11 | Autosave | No error message / retry for failed autosave; no spec for invalid time in Slider mode; no spec for concurrent edit (two tabs, or admin editing the same user). | Negative tests are assumptions. |
| G-12 | Future / past dates | Can time be added to a future date? Any limit on past dates? | TBC. |
| G-13 | Unfinalize permission | "tier 1" is not defined. Who can unfinalize another employee's day? | TBC. |
| G-14 | Employee selector | P1: Admin, Dev PM, Client PM. P3: rank based, "PM" role name ambiguous; "Lead, Designer, Dev, PM" see a label only. | Use P3 rank rule. |
| G-15 | Download CSV | Filename uses `finalized_date` – what for an unfinalized day? Is the button enabled when there are no rows? CSV escaping of commas/quotes in description. | TBC. |
| G-16 | Delete | No confirmation dialog specified; delete on finalized day hidden. | Assumed no confirmation. |
| G-17 | Performance targets | No numbers for load/autosave time. | NFC uses assumed targets, marked TBC. |
| G-18 | Hours column | Rounding/format for >9.99 hours and total over 24 hours not defined. | TBC. |
| G-19 | Required data validation | Remaining is "required when visible" – confirm yellow "!" applies only when Remaining is visible. | Assumed. |
| G-20 | Rename | P4 renames "Organization" to "Customer" – labels in Add from Previous (Org column / filter) change. | Use "Customer" for P4 build. |
