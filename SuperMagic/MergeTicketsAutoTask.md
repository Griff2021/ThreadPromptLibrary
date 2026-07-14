Use this skill when a technician needs to merge two AutoTask tickets. AutoTask's API does not support native ticket merging, so this multi-step process simulates a merge manually.

Before starting, confirm with the technician:
- **Source ticket** — the ticket to be merged away and closed.
- **Destination ticket** — the ticket that will survive and carry all history.

---

**Step 1 — Set source ticket status to Complete**
Call `update_ticket` on the source ticket with the "Complete" status_id.
If you do not already have the status_id, call `list_ticket_statuses` first to resolve it.

---

**Step 2 — Set source ticket Queue to match the destination ticket's Queue**
Fetch the destination ticket via `search_tickets` (single internal_ticket_id lookup) to get its board_id.
Call `update_ticket` on the source ticket with that board_id.

---

**Step 3 — Add an internal note to the source ticket**
Call `add_ticket_note` on the source ticket with is_internal=true.
Note text (adapt as needed):
"This ticket has been merged into ticket [DESTINATION_TICKET_ID]. Please refer to that ticket for all further updates."

---

**Step 4 — Add an internal note to the destination ticket**
Call `add_ticket_note` on the destination ticket with is_internal=true.
Note text (adapt as needed):
"Ticket [SOURCE_TICKET_ID] has been merged into this ticket. All history from the source ticket has been carried over below."

---

**Step 5 — Query all content from the source ticket**
Call `search_tickets` with internal_ticket_id = source ticket and include_messages=true.
Review the full detail view for:
- All notes/messages (internal and external)
- Schedule entries
- Any other linked data visible on the ticket

---

**Step 6 — Recreate all content on the destination ticket**
For each note/message retrieved from the source ticket:
- Call `add_ticket_note` on the destination ticket with is_internal=true.
- Preserve the original author name, date context, and internal/external flag where possible.
- Prefix each recreated note with: "[Merged from ticket SOURCE_TICKET_ID — originally posted DATE]"

For any schedule entries found on the source ticket:
- Call `schedule_ticket` on the destination ticket to recreate them with the same member, date, and time block.

---

**Step 7 — Confirm completion**
Summarise what was transferred to the technician:
- Confirm status and queue were updated on the source ticket.
- List how many notes were recreated on the destination ticket.
- List any schedule entries recreated.
- Confirm the merge is complete.