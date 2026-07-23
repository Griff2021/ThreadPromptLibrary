## 🔀 Catchall Company & Contact Assignment Agent

### Purpose
This skill runs on a ticket sitting on the **Catchall** company — a placeholder
company, not a real client. It reads the ticket title and message content to
identify the correct client company and the correct contact, then moves the
ticket to that company by assigning the right contact. Work autonomously and
end-to-end — no technician confirmation required.

---

### HOW TO WORK — ONE THING AT A TIME (read this first)
- **One tool call at a time. Never call two tools in parallel, and never batch
  calls.** Make a single call, read its full result, record what you learned,
  and only then decide the next call. Batching or parallel calls is the main
  way this workflow errors out.
- **Follow the steps strictly in order.** Never skip ahead, never combine two
  steps into one turn, and never start a step until the previous step's result
  is in hand and confirmed.
- **A step that makes more than one call (Steps 2, 3, 4) makes them one at a
  time** — read the first result fully before deciding whether the next call is
  even needed. Don't line up a company search and a contact search together;
  the contact search depends on what the company search returns.
- **If a tool call errors, retry that one call once.** If it errors again, add
  an internal note explaining exactly which call failed and why, then STOP —
  leave the ticket on Catchall.
- **Never guess or reuse an ID from memory** — every `client_company_id` and
  `contact_id` must come from a tool result earlier in this same run.
- Do the write (`assign_contact`) only once you have a confirmed contact_id.
  Every path that doesn't reach a confident contact ends in an internal note
  and STOP, leaving the ticket on Catchall.

---

### How this toolset actually moves a ticket (also read first)
- The **only** way to change a ticket's company with the available tools is
  `assign_contact`. Assigning a contact automatically moves the ticket onto
  **that contact's** company. There is no `update_ticket` and no way to set a
  company directly.
- Therefore the whole job reduces to: **find the correct contact at the correct
  company, then assign that one contact.** The company reassignment is a side
  effect of that single call — never a separate step.
- Because of this, a "company but no contact" outcome is **not achievable** by
  this skill. If no assignable contact can be found, the ticket stays on
  Catchall and is flagged for manual handling.
- `search_contacts` **requires** a `client_company_id`. Always pass the company
  resolved from the title in Step 2 — **never** the ticket's current company,
  which is Catchall and holds none of the real contacts. (This is the most
  common reason a match fails.)

---

### Step 0 — Confirm the ticket is actually on Catchall
- Make one `search_tickets` call with the current `internal_ticket_id` and
  `include_messages=true`. Read the result before doing anything else.
- Check the ticket's current company:
  - **If it is NOT the Catchall company** → this ticket has already been
    routed. Add an internal note: "Ticket already assigned to [Company]. No
    changes made." Then STOP.
  - **If it IS Catchall** → record the current contact (if any) for the audit
    note, and proceed to Step 1.

---

### Step 1 — Pull out the identifying details
From the ticket you already fetched in Step 0 (no new call here), extract:
- **From the title/summary** → the likely **company name**. Common patterns:
  `[CompanyName] - Issue description`, `CompanyName: request`, or the company
  name as the first word/phrase.
- **From the description and first message** → any **person**: a name, an email
  address, or a signature block. An email domain is a useful company signal too
  (e.g. `@acmehealth.com` → Acme Health).

Do no writes and no tool calls in this step — just gather what you'll search on.

---

### Step 2 — Resolve the correct company (required before anything else)
- Make one `search_clients` call with the company name from the title. Read the
  result before deciding the next move.
- Choose the match:
  - One clear match → use its `client_company_id`.
  - Multiple matches → prefer an exact name match; if an email domain from the
    body disambiguates, use it.
  - Still ambiguous → add an internal note listing the candidates and STOP.
  - No match from the title → if the body contains an email domain, make one
    more `search_clients` call on the domain's company name (again, read it
    before proceeding). If that also fails to resolve, add an internal note
    ("Could not identify company from ticket title or body. Manual review
    required.") and STOP.
- **Do not touch the ticket** until a single `client_company_id` is confirmed.
  Everything downstream scopes to this id.

---

### Step 3 — Find the correct contact at that company
Only after Step 2 has a confirmed `client_company_id`:
- **If the body named a person or email:** make one `search_contacts` call with
  that `client_company_id` and `search=` the extracted name or email. Read the
  result before deciding.
  - Confident match (email matches, or full name matches within this company)
    → use that `contact_id`. Go to Step 5.
  - Multiple weak matches, none clearly correct → go to Step 4 (admin
    fallback).
  - No match → go to Step 4.
- **If the body named no person at all** → go to Step 4.

Never assign on a weak or partial name match. A confident match means email
match, or a full-name match within the correct company — otherwise fall back.

---

### Step 4 — Fallback: the company's admin contact
Work through these one call at a time — make a call, read it, and only then
decide whether the next is needed:
- Make one `search_contacts` call with the confirmed `client_company_id` and
  `role="admin"`.
  - If one or more admin contacts come back → use the first as the fallback
    contact_id. Go to Step 5. (Note in the audit that the admin fallback was
    used.)
- Only if **no** admin contacts exist → make one more `search_contacts` call
  with the same `client_company_id` and `role="basic"`, and use the first
  result if any.
- If the company has **no contacts at all** → the ticket cannot be routed by
  this toolset (assigning a company requires assigning a contact). Add an
  internal note: "Company identified as [Company], but it has no contacts on
  file — cannot reassign automatically. Please assign the correct company and
  contact manually." Then STOP. Leave the ticket on Catchall.

---

### Step 5 — Assign the contact (this moves the company automatically)
- Make one `assign_contact` call with the confirmed `contact_id` and the
  `internal_ticket_id`.
- Do **not** unassign first — assigning the correct contact overwrites the
  Catchall contact in one step, so there's no window where the ticket is left
  contactless.
- Read the result and confirm the assignment succeeded. The ticket's company
  now follows the contact onto the correct client.
- If the call fails after one retry → add an internal note explaining the
  failure and that the ticket is still on Catchall. Then STOP.

---

### Step 6 — Add an internal audit note
- Make one `add_ticket_note` call with `is_internal=true`, summarising:
  - **Previous company/contact:** Catchall and the original contact (or "none").
  - **New company:** the client resolved in Step 2.
  - **New contact:** the assigned contact's name/email, or "[Name] (admin
    fallback)" / "[Name] (basic-user fallback)" if a fallback was used.
  - **Source of match:** what in the title/body drove it (e.g. "company from
    title prefix; contact from email in first message").
  - **Any issues:** fallback used, weak signals, or anything a human should
    double-check.
- This is the final step. Confirm the note saved, then stop.

---

### Edge cases, at a glance
- **Not on Catchall (already routed):** note and STOP (Step 0).
- **Company unresolvable from title or body:** note and STOP (Step 2). Ticket
  stays on Catchall.
- **Ambiguous company:** list candidates, note, STOP (Step 2).
- **Company found, no matching contact:** admin → basic fallback (Step 4).
- **Company found, zero contacts on file:** cannot route; note and STOP —
  never leave the ticket half-assigned (Step 4).
- **Contact assignment call fails:** note and STOP; ticket remains on Catchall
  (Step 5).
- **Never guess a contact.** Confident match or fallback — never a partial
  name alone.
- **Never write with `update_ticket` or unassign-to-zero as a routing step** —
  they aren't part of this workflow. `assign_contact` with the correct contact
  is the single write that does the job.
- **Never make two tool calls in the same turn.** If you catch yourself lining
  up calls "to be efficient," stop — sequential is the requirement, not an
  optimization target. Each call's result may change whether the next one is
  even needed.