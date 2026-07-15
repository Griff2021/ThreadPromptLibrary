==================================================
SKILL: Board & Status Configuration Guide
WHEN TO USE: Verifying a board is correctly configured with the right statuses, priorities, and default settings in the PSA.
==================================================
1. Identify the board to audit — Ask the technician which board they want to verify, or call list_boards to list all active boards and let them pick one.

2. Review statuses — Call list_ticket_statuses for the board. Verify:
   - There is at least one open/active status (e.g. "New", "In Progress", "Waiting on Client")
   - There is at least one closed status (e.g. "Resolved", "Closed", "Completed")
   - No duplicate or ambiguously named statuses exist
   - A sensible default status is set for new tickets landing on this board

3. Review priorities — Call list_ticket_priorities. Verify:
   - All expected priority tiers are present (e.g. Low, Medium, High, Critical)
   - A default priority is configured so tickets aren't created without one
   - Priority names are consistent with SLA agreements in use

4. Check default settings — Confirm with the technician:
   - Is the correct default board set for inbound tickets (email, Thread Messenger, phone)?
   - Are SLA timers attached to the right priority/status combinations?
   - Are auto-assignment rules or round-robin settings configured if needed?

5. Spot-check live tickets — Call search_tickets with board_id and state="open" (limit 5–10). Verify tickets are landing with the expected status and priority — flag any with missing/null values.

6. Flag issues found — Summarise any gaps (missing statuses, no default priority, tickets with null status, etc.) and recommend corrective actions. If the technician wants to fix a ticket's status or priority, use update_ticket.

7. Document findings — Offer to add an internal note to a relevant thread or save a summary for the technician's records.