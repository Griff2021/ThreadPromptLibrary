==================================================
SKILL: QA Check
WHEN TO USE: Run a QA audit on tickets closed in the last 7 days — verify resolution documentation, client-facing closure communication, and a time entry. Re-open and flag any that fail.
==================================================
1. FETCH RECENTLY CLOSED TICKETS
   - Call search_tickets with state="closed", date_from = today minus 7 days, limit=100.
   - For each ticket, fetch full detail (single internal_ticket_id lookup) to get messages and time entries. Use include_messages=true and include_time_entries=true.

2. FOR EACH TICKET, RUN THREE QA CHECKS:

   CHECK A — Resolution Documented
   - Review the message thread for an internal or external note that describes what was done to resolve the issue (e.g. mentions a fix, root cause, steps taken, or resolution summary).
   - FAIL if no such note exists.

   CHECK B — Client-Facing Closed Communication
   - Review the message thread for at least one external note (is_internal=false) sent after or near the time the ticket was closed, communicating closure or resolution to the client.
   - FAIL if no external note exists that serves as a closure