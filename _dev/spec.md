# Spec — Transfer Request POC

## Goal
Build a UI/UX POC for an internal NIS employee transfer request process. The real implementation will use MS Forms (manager submission), Power Automate (record creation and notifications), Power Apps (HR review and admin screen), and SharePoint Lists (data store). This POC demonstrates the intended screens, workflow, status model, and user experience before the Power Apps build begins.

## Core Concept
This is a transfer request workflow, not a general form. The experience focuses on manager submission, HR review/approval, email notifications to downstream teams, and admin record follow-up.

## Primary Users
1. **Managers / Submitting Supervisors** — submit transfer requests via MS Forms (no Power Apps license required).
2. **HR Staff** — review requests, add HR-owned information, approve or decline.
3. **Payroll** — receive notification emails; access admin screen for notes and follow-up.
4. **IT** — receive notification emails for Entra updates.
5. **Finance, HR, Payroll** — view historical records in the admin screen.

## Process Flow
1. Manager submits the MS Form from the Manager Portal.
2. Power Automate creates a SharePoint item (status: Submitted) and emails the HR distro.
3. HR opens the request in Power Apps, enters HR-owned fields, and either Approves or Declines.
4. On Approve: Power Automate sends email to Payroll distro and IT distro. Status → Completed.
5. On Decline: Power Automate sends decline email to the submitter only. Status → Declined.
6. Admin users access the Request Hub screen to view history, filter/sort, and add notes.

## Manager Intake Fields
- Employee First Name (required)
- Employee Last Name (required)
- Effective Transfer Date (required)
- Contract or Department (required, single text field)
- Task Order (optional)
- New Job Title (required)
- Transfer Type: Full-Time Transfer or Split Allocation (required)
- New Contract Allocation % (conditional, if Split Allocation — manager enters their contract's share; HQ calculated as remainder)
- NIS Equipment Needed? Yes/No (required)
- Equipment Details: Laptop / Phone / Other (conditional, if Yes)
- Team Lead? Yes/No (required)
- Direct Reports? Yes/No (required)
- Salary Change (optional — HR confirms or overrides)
- Manager Comments (optional)

## HR Review Fields
- Employee ID
- New BUD (Business Unit Director — free text, only if changed)
- Labor Category (free text)
- Salary Change (HR confirms or overrides manager value)
- HR Notes (required when declining)

HR may override any manager-submitted field.

## Status Model
| Status | Set By |
|---|---|
| Submitted | Power Automate (on creation) |
| Completed | Power Automate (after approval + notifications sent) |
| Declined | HR (Decline action) |

HR actions are Approve or Decline only. No intermediate statuses.

## Notification Recipients
- **HR distro** — new request alert (email)
- **Payroll distro** — approval notification (email, hardcoded address)
- **IT distro** — approval notification (email, hardcoded address)
- **Submitter** — decline notification only (email, captured from Forms)

## Admin Screen
- Access: HR, Finance, Payroll
- View and search all records
- Filter by: Status, Follow-Up Complete
- Sort by any column
- Search by employee name or contract/department
- Click row to expand full detail
- Follow-up complete toggle (all admin roles)
- Payroll notes: Payroll role only (honor-system UI for POC)

## Known Downstream Systems
- HR: ADP
- Payroll: Unanet
- IT: Entra
- Admin hub: historical view in Power Apps

## Out of Scope for POC
- Production Power Apps implementation
- Live SharePoint List connection
- Real Power Automate flows or email sending
- Real MS Forms integration
- Waitlist, escalation, or reminder logic
- Jira ticket generation

## POC Build Direction
- Single `index.html` file, plain HTML/CSS/JS, no framework, no build step
- Open `index.html` in a browser to demo
- MS Forms visual style (purple accent, white cards) for manager screens
- Power Apps admin style (sidebar, clean table) for HR and admin screens
- Sample data: 4 requests covering all status values
- Honor-system role selector in admin screen (HR / Finance / Payroll)

## Sol Approval
[x] Approved 2026-05-05
