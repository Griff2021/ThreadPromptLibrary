==================================================
SKILL: Duplicate Thread Cleanup
WHEN TO USE: Identify, consolidate, and close duplicate threads — especially those created from repeated phone calls or multi-fire automations.
==================================================
Use this skill when you notice multiple threads from the same contact or phone number with identical or near-identical summaries.

1. Find duplicates: Use search_tickets with the contact name, phone number, or summary keywords (e.g. "New call from +15166047665"). Sort oldest-first (sort=asc) to identify the original/parent thread — this is the one to keep.

2. Audit before closing: Scan each duplicate for meaningful notes, time entries (include_time_entries=true), or status differences. If any duplicate has useful content, copy it to the parent thread as an internal note before closing.

3. Document on the parent: Add an internal note to the parent thread listing all duplicate thread IDs being consolidated. Example: "Closing duplicates: #9765, #9764, #9763 — all created from the same call with no additional detail."

4. Rename duplicates: Update each duplicate's title to "DUPLICATE - see #[parent_ticket_id]" so it remains traceable in the PSA.

5. Close duplicates: Update each duplicate's status to the board's closed status. Use list_ticket_statuses if you don't already have the closed status_id for that board.

6. Resolve unknown contacts: If the parent thread has an "Unknown" contact, attempt to resolve it using search_contacts with the caller's phone number or name. Assign the correct contact via assign_contact.

7. Flag systemic issues: If 5 or more duplicates exist for the same issue/caller, add an internal note on the parent recommending that an intent or automation flow be reviewed to prevent re-firing in the future.