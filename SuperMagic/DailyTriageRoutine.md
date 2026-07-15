==================================================
SKILL: Daily Triage Routine
WHEN TO USE: Every morning to check unassigned threads, review P1s, scan SLA timers, and dispatch work across the team.
==================================================
## Daily Triage Routine

Run this skill each morning to clear the queue, catch fires early, and dispatch the day's work.

### Step 1 — Unassigned Threads
1. Call search_tickets with assigned=false and state="open".
2. List all unassigned threads, grouped by priority (highest first).
3. For each thread, note the client, summary, and how long it has been open.
4. Recommend an assignee based on the thread type or ask the technician who should own each one.
5. Assign threads using update_ticket (assigned_member_id) once the technician confirms.

### Step 2 — P1 / Critical Priority Review
1. Call search_tickets with priority="Priority 1" (or the MSP's equivalent critical label) and state="open".
2. List all open P1 threads with their current status, assignee, and last-update timestamp.
3. Flag any P1 that has had no update in the last 2 hours as STALE — surface these first.
4. For stale P1s, suggest adding an internal note or escalating to a senior tech.

### Step 3 — SLA Timer Scan
1. For each open P1 and P2 thread returned above, fetch the full ticket detail (single internal_ticket_id lookup) to read the SLA timers.
2. Identify threads where the SLA respond or resolve deadline is within 2 hours or already breached.
3. Present a prioritised list: BREACHED → AT RISK (< 2 hrs) → HEALTHY.
4. For any BREACHED or AT RISK thread, recommend an immediate action (assign, escalate, update status).

### Step 4 — Dispatch Summary
1. Compile a morning dispatch summary:
   - Total open threads (all priorities)
   - Unassigned count (before and after Step 1)
   - P1 count and stale P1 count
   - SLA breached count / at-risk count
   - Top 3 threads needing immediate attention (with links)
2. Present the summary in a clean, scannable format.
3. Ask the technician if they want to act on any flagged threads before closing the routine.

### Tips
- Run this skill at the start of each shift, not just mornings.
- If the MSP uses multiple boards, repeat Steps 1–3 per board or ask which board to focus on.
- After dispatch, offer to run the Escalation Risk Detector skill for a deeper sentiment/SLA scan.