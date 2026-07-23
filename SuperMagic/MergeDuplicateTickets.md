## Merge Duplicate Tickets
 
### Skill Activation
This skill activates in the following ways:
 
**Slash commands:**
- `/dupe` — triggers this skill for duplicate detection and merging.
- `/merge` — triggers this skill for duplicate detection and merging.
- Arguments after the slash command indicate scope. Examples:
  - `/dupe SEA` — scan the SEA board for duplicates.
  - `/merge #12345 #67890` — evaluate two specific tickets as a potential duplicate pair.
  - `/dupe` with no argument — prompt the technician to specify a ticket, board, or set of tickets to evaluate.
 
**Explicit invocation:** The technician directly asks to find, check, or merge duplicate tickets.
 
**Passive trigger recognition:** If the technician's message contains words such as:
'duplicate', 'duplicates', 'duplicated', 'merge', 'merging', 'merged', 'bundle',
'bundled', 'bundling', 'same ticket', 'already open', 'filed twice', 'submitted twice',
or similar variations — evaluate whether this skill is relevant to their request before
responding. If it is relevant, load and follow this skill automatically without waiting
to be explicitly told to do so.
 
If the intent is ambiguous, briefly confirm with the technician before proceeding.
 
---
 
### Triggering the Skill
This skill can be invoked in the following ways:
- **Two (or more) ticket IDs provided:** Evaluate those tickets directly as a potential duplicate group.
- **One ticket ID provided:** Search for likely duplicates of that ticket across all boards.
- **Board(s) specified:** Scan the entire board (or multiple boards) for duplicate ticket groups. When scanning, search for qualifying tickets and evaluate them in batches for duplicate signals. Report all suspected duplicate groups before taking any action.
 
---
 
### Step 0 — Sync & Verify (MANDATORY — Run Before Anything Else)
 
Before fetching, evaluating, or acting on ANY ticket, run the **Sync & Verify Ticket** skill on all candidate tickets involved in this workflow.
 
- If specific ticket IDs were provided, run Sync & Verify on each of those tickets.
- If a board scan was requested, run Sync & Verify on each ticket retrieved from the board before evaluating it for duplicates.
- Do NOT proceed to Step 1 until the technician has confirmed they have clicked the 🔄 Sync icon for all relevant tickets AND the re-fetch has been completed per the Sync & Verify Ticket skill instructions.
- Any tickets flagged by Sync & Verify as already-merged (parent_ticket_id set) or already-closed should be treated as ineligible per the Ticket Eligibility rules below — do not evaluate them further.
 
---
 
### Ticket Eligibility
Before evaluating any ticket as a duplicate candidate, check its eligibility:
- **Open tickets:** Always eligible, UNLESS the ticket already has a parent ticket (i.e. it has already been merged or bundled). Tickets with a parent ticket have already been handled — skip them entirely and do not flag them.
- **Closed tickets:** Only eligible if the ticket has been in a closed status for LESS than 7 days from the current date/time, AND it does not already have a parent ticket. If a closed ticket has been closed for 7 or more days, exclude it from duplicate evaluation entirely. If it would have been a relevant match, note it as excluded and why.
- **"No Work" status:** If a ticket's current status in Thread is **"No Work"**, exclude it from duplicate evaluation entirely. It has already been resolved or merged — do not flag it, reference it, or act on it.
- **Do not reopen or modify any ineligible closed ticket under any circumstances.**
 
---
 
### Step 1 — Fetch & Evaluate
 
For each ticket (or group), retrieve:
- Ticket title / summary
- Full message thread (client-facing notes)
- Contact name
- Assigned technician
- Logged time entries
- Board
- Created date
- Current status
- Parent ticket (if any — used to determine eligibility above)
 
**Duplicate signals to check:**
1. **'Re:' or 'FW:' in the title** — strong signal; likely caused by a client replying to or forwarding an existing ticket.
2. **Similar ticket title** — titles may not match exactly (triage agent may have rewritten them), but look for strong semantic similarity.
3. **Significant content overlap** — client-facing note text is largely duplicated, often caused by email reply chains appending prior notes.
 
---
 
### Step 2 — Present Findings
 
Duplicates may exist as chains of 3 or more tickets (e.g. a client replies multiple times, each reply creating a new ticket). Group all related tickets together — do not limit analysis to pairs only.
 
**Formatting rules for findings:**
- Present all findings in plain, readable prose and bullet points.
- Do NOT use tables — technicians may not be able to scroll to see all columns.
- Each duplicate group should be clearly separated and easy to read at a glance.
 
For each suspected duplicate group, present in this format:
 
---
**Duplicate Group [#]**
 
- **Ticket A:** [ID + title + link] — Created: [date] | Status: [status] | Assigned: [technician]
- **Ticket B:** [ID + title + link] — Created: [date] | Status: [status] | Assigned: [technician]
- **Ticket C:** [ID + title + link] *(if applicable)* — Created: [date] | Status: [status] | Assigned: [technician]
 
**Why these are duplicates:**
- [Bullet 1 — concise reason]
- [Bullet 2 — concise reason]
- [Bullet 3 — if applicable]
 
---
 
> ⚠️ **FLAG — Possible Separate Issue:** If any ticket in the group appears to be raising a *different, unrelated request* (even if created via a reply to the original), call this out clearly with a short, concise explanation. Do not include that ticket in the merge plan without explicit technician acknowledgement. The technician may choose to exclude it from the merge or confirm it should still be merged.
 
> ⚠️ **FLAG — Excluded Closed Ticket:** If a ticket was excluded because it has been closed for 7 or more days, note it here so the technician is aware.
 
> ⚠️ **FLAG — Excluded Already-Merged Ticket:** Do NOT flag or report tickets that already have a parent ticket. These have already been merged or bundled and require no action.
 
If no duplicates are found, report that clearly and stop.
 
---
 
### Step 3 — Propose Merge Plan
 
Before taking any action, present the full proposed merge plan for technician review. Use plain bullets — no tables.
 
- **Parent ticket:** The oldest ticket (by created date), unless the technician specifies otherwise.
- **Child ticket(s):** All newer tickets in the group — there may be more than one.
- **Board:** Parent ticket's board. If any tickets in the group are on different boards, flag this explicitly and confirm with the technician. Default to the parent ticket's board if no preference is given.
- **Contact:** Parent ticket's contact becomes the primary contact. If contacts differ across any tickets in the group, call this out explicitly in the plan.
- **Ticket owner / assigned technician:**
  - If multiple tickets have logged time entries AND the assigned technicians are different, ask the technician to confirm who should become the ticket owner before proceeding.
  - If the same technician is on all tickets, no action needed.
- **Post-merge status changes:**
  - Parent ticket → set to **'Update Required'**
  - All child tickets → set to **'No Work'**
 
---
 
### Step 4 — Await Explicit Confirmation
 
**Do NOT make any changes until the technician explicitly confirms.**
 
Present the full merge plan clearly and ask: *"Shall I proceed with this merge?"*
 
If the technician modifies any part of the plan (parent/child swap, board, contact, owner, exclusions), update the plan accordingly and confirm once more before acting.
 
If multiple duplicate groups were found (e.g. during a board scan), handle each group's confirmation one at a time — do not batch execute across groups without individual confirmation for each.
 
---
 
### Step 5 — Execute Merge
 
For each confirmed group, in order:
1. If any child tickets are on a different board than the parent, update those child tickets' board to match the parent first.
2. Merge all child tickets into the parent ticket.
3. Set the **parent ticket** status to **'Update Required'**.
4. Set all **child tickets** status to **'No Work'**.
5. If any contact differs, assign the parent ticket's contact as the primary contact on the merged ticket.
6. If a technician ownership change was confirmed, update the assigned member on the parent ticket.
7. Add an **internal note to the parent ticket** containing:
   - Statement that this ticket was merged with [child ticket ID(s) + link(s)].
   - The bulleted duplicate reasoning from Step 2 (copy exactly — this is the audit trail).
   - Any conflict resolutions applied (contact change, board change, owner change).
   - Confirmation that child ticket(s) were set to No Work and parent set to Update Required.
 
---
 
### General Rules
- Always run the **Sync & Verify Ticket** skill before any ticket evaluation or action (Step 0 above).
- Never take any write action (merge, status change, note, reassignment) without explicit technician confirmation.
- When scanning a board or multiple boards, report ALL suspected duplicate groups first, then handle merge confirmations one group at a time.
- Always link tickets using the format: [ticket_id](/conversations/{internal_ticket_id})
- Keep all reasoning concise and bulleted — no long-form explanations.
- Never use tables in responses — use plain prose and bullet points only.
- If a status name (e.g. 'No Work', 'Update Required') does not exist on a given board, flag this to the technician and ask how to proceed before continuing.
- Never modify, reopen, or reference ineligible closed tickets (closed 7+ days) as merge candidates.
- Never flag or act on tickets that already have a parent ticket — they have already been merged or bundled and require no action.