==================================================
SKILL: Escalation Risk Detector
WHEN TO USE: Scan open threads for sentiment signals, SLA proximity, and stalled replies — flags threads likely to escalate before they do, so a senior tech can intervene early.
==================================================
1. Gather scope — Ask the technician if they want to scan all open threads or narrow by client, board, or assigned member. Note any preferences before proceeding.

2. Pull open threads — Use search_tickets with state="open" and any filters from step 1. Fetch up to 100 tickets. If scoping to a client, resolve client_company_id via search_clients first.

3. Flag negative sentiment — From the results, identify any threads with sentiment of negative or very_negative. Mark these as Sentiment Risk.

4. Flag stalled threads — Identify threads with no update in 3+ days using updated_to set to 3 days before today. Mark these as Stalled.

5. Flag SLA proximity — For any thread where the SLA breach time appears close (within 4 hours) based on ticket detail, mark as SLA Risk. Fetch individual ticket detail via search_tickets with a single internal_ticket_id to read SLA timer fields when needed.

6. Score and rank — Assign each flagged thread a risk score:
   - Negative sentiment → +2
   - Very negative sentiment → +3
   - Stalled (3–5 days) → +1
   - Stalled (5+ days) → +2
   - SLA breach within 4 hours → +3
   - Multiple flags → scores stack

7. Present the report — Display a ranked table of at-risk threads, highest score first. For each, show: thread link, client, assignee, risk flags, score, and last updated date. Group into tiers: 🔴 Critical (5+), 🟠 High (3–4), 🟡 Watch (1–2).

8. Recommend actions — For each 🔴 Critical thread, suggest a specific next step (e.g. "Add an update note", "Escalate to senior tech", "Check SLA breach time"). Offer to add an internal note or reassign directly.

9. Offer to save — Ask if the technician wants to run this scan on a schedule or save any flagged threads for follow-up.