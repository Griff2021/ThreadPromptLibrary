## Merge Duplicate Tickets

### Activation
- **Slash commands:** `/dupe`, `/merge`. Args set scope — `/dupe SEA` (scan board), `/merge #123 #456` (pair), `/dupe` alone (ask what to evaluate).
- **Explicit ask**, or **passive trigger** on words like "duplicate," "merge(d/ing)," "bundle(d/ing)," "same ticket," "already open," "filed/submitted twice" → auto-load. Ambiguous → confirm first.
- **Modes:** 2+ IDs → evaluate as group; 1 ID → search all boards for dupes; board(s) → scan, report all groups before acting.

### Step 0 — Sync & Verify (mandatory, first)
Run **Sync & Verify Ticket** on every candidate ticket before evaluating anything; wait for confirmed 🔄 Sync + re-fetch. Flagged merged/closed tickets follow Eligibility below.

### Ticket Eligibility
- **Open:** eligible unless `parent_ticket_id` set (already merged — skip silently).
- **Closed:** eligible only if closed <7 days and no parent; 7+ days → exclude (note if it would've matched).
- **"No Work" status:** always exclude. Never reopen/modify an ineligible closed ticket.

### Step 1 — Fetch & Evaluate
Retrieve: title, message thread, contact, assigned tech, logged time, board, created date, status, parent ticket.
**Signals:** "Re:"/"FW:" in title (strong); semantically similar title (may be triage-rewritten); significant content overlap (reply-chain dupes).

### Step 2 — Present Findings
Group chains of 3+ tickets, not just pairs. Prose/bullets only — **no tables**. Per group, list each ticket (ID+title+link, created date, status, tech), then "Why these are duplicates" bullets.
Flags: ⚠️ **Possible Separate Issue** — unrelated request in chain, exclude until confirmed. ⚠️ **Excluded Closed** — 7+ days, note why. Never flag already-merged tickets. No duplicates → say so, stop.

### Step 3 — Propose Merge Plan
Bullets, no tables:
- **Parent:** oldest by created date (unless overridden). **Child(ren):** all newer.
- **Board:** parent's; flag+confirm if group spans boards.
- **Contact:** parent's; call out if contacts differ.
- **Owner:** time logged by different techs → ask technician to confirm owner; same tech → no action.
- **Status:** parent → "Update Required"; children → "No Work". Missing status name → flag, ask first.

### Step 4 — Await Confirmation
No changes until explicit confirmation. Ask: *"Shall I proceed with this merge?"* Modified plan → update, reconfirm. Multiple groups → one at a time, never batch.

### Step 5 — Execute (per confirmed group, in order)
1. Move off-board children onto parent's board. 2. Merge children into parent. 3. Set statuses (Step 3). 4–5. Apply confirmed contact/owner changes. 6. Internal note on parent: merged with [child ID(s)+links, `[ticket_id](/conversations/{internal_ticket_id})`]; Step 2's reasoning bullets verbatim (audit trail); conflicts resolved; status-change confirmation.

Throughout: bullets only, never tables; never write without explicit confirmation; never touch ineligible closed or already-parented tickets.