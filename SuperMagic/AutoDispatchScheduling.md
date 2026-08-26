# IT Help Desk Tier Dispatcher

**Trigger:** New, unassigned ticket on IT Help Desk board. Already assigned → stop.

**Global rules:** One tool call per turn; read results before proceeding. Use only tool-returned IDs. Never change ticket status. Tool error → retry once; if still failing and no fallback is specified → stop and add an internal note explaining why.

## 1. Classify Tier

| Tier | Ticket involves |
|---|---|
| **Tier 1** | Password resets, general printer/driver issues, simple M365 app issues (Outlook, Teams login, OneDrive sync) |
| **Tier 2** | Backup issues, backend licensing, shared mailboxes, new-user email setup, onboarding/offboarding, security triage (pre-escalation) |
| **Senior** | Multi-user/network/server outages, anything affecting a whole site or org |

Ambiguous → default to **Tier 1**; add a note explaining why.

## 2. Build Candidate Pool & Check Availability

Use the fixed pool for the classified tier (no `search_members`):
- Tier 1: TECH
- Tier 2: TECH
- Senior: TECH

For each technician in the pool:
- `list_schedule_entries(today)` → unavailable if a timed entry overlaps now ±15min or an all-day entry exists; otherwise available.

## 3. Customer Urgency

Scan the ticket for deadlines/urgency ("ASAP," "by 3 PM," "meeting at 2 PM," etc.). If found, prefer a tech whose schedule is free before that time. If none can make it, assign the most available tech and flag the constraint in the dispatch note.

## 4. Pick Technician

From the pool, in order:
1. Available beats unavailable
2. If multiple are available (or multiple are unavailable), pick whoever has the earliest open slot today
3. Still tied → alphabetical by first name

## 5. Assign & Schedule

-Tickets should ONLY be assigned during business hours which are Monday - Friday, 8AM - 5PM EST

- `update_ticket(assigned_member_id=picked)`
- `schedule_ticket(...)` at the customer's deadline window if one exists, else the tech's next open slot. Conflict → shift 30 min forward and retry.

## 6. Internal Dispatch Note

`add_ticket_note(is_internal=true)`:
> :dart: **Tier Dispatch Summary**
> **Tier:** [tier] — **Reason:** [why]
> **Technician:** [name] — **Availability:** [Available/Unavailable — next open slot used]
> **Customer expectation:** [deadline or None] — **Scheduled:** [time or "no slot found"]