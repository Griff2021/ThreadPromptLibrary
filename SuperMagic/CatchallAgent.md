## 🔀 Catchall Company & Contact Assignment Agent

### Purpose
This skill runs automatically via a Flow on tickets assigned to the Catchall company. It reads the ticket title and body to identify the correct client company and contact, then re-assigns the ticket accordingly — no technician confirmation required.

---

### Step 1 — Read the ticket in full
- Call `search_tickets` with the current `internal_ticket_id` and `include_messages=true`.
- Extract the following from the ticket:
  - **Title/summary** → used to identify the correct **company name**
  - **Description and first message/note** → used to identify the correct **contact name**
- Note the current contact and company on the ticket for the audit note later.

---

### Step 2 — Identify the correct company from the title
- Parse the ticket **title/summary** for a company name.
  - Common patterns: `[CompanyName] - Issue description`, `CompanyName: request`, or the company name appearing as the first word/phrase.
- Call `search_clients` with the extracted company name to resolve `client_company_id`.
- If multiple clients match, select the closest exact match by name. If still ambiguous, add an internal note explaining the ambiguity and stop — do not reassign.
- If no company can be identified from the title, add an internal note stating this and stop.

---

### Step 3 — Identify the correct contact from the body
- Scan the ticket **description and body/notes** for a person's name, email address, or other identifying details.
- If a name or email is found:
  - Call `search_contacts` with the resolved `client_company_id` and the extracted name/email.
  - If a confident match is found (name + company align, or email matches), use that contact.
  - If multiple contacts match and none is clearly correct, fall back to the primary contact (see Step 4).
  - If no contact is found under that company, fall back to the primary contact (see Step 4).
- If **no name or email is present** in the body, proceed directly to Step 4.

---

### Step 4 — Fallback: find the primary contact
- If no contact was identified in Step 3, call `search_contacts` with the resolved `client_company_id` and `role="admin"` to retrieve admin/primary contacts.
- Select the first result as the primary contact.
- If no contacts exist at all for the company, assign the company only (no contact) and note this in the audit note.

---

### Step 5 — Unassign the current contact
- Call `assign_contact` with `contact_id=0` to clear the existing Catchall contact from the ticket.

---

### Step 6 — Assign the correct contact (and company)
- Call `assign_contact` with the confirmed `contact_id` to set the correct contact on the ticket.
- The ticket's company will update automatically to match the contact's company.
- If no contact was found but a company was identified, call `update_ticket` to set `client_company_id` to the resolved company.

---

### Step 7 — Add an internal audit note
- Call `add_ticket_note` with `is_internal=true` summarising what was done:
  - **Previous company/contact**: the Catchall company and original contact (if any)
  - **New company**: name resolved from the ticket title
  - **New contact**: name and email of the assigned contact, or "Primary contact" if fallback was used, or "None found" if no contact exists
  - **Source of match**: what in the title/body led to this assignment
  - **Any issues**: ambiguity, no match found, or partial assignment

---

### Edge Cases & Guard Rails
- **Company not found in title**: Add an internal note — "Could not identify company from ticket title. Manual review required." — and stop. Do not unassign the current contact.
- **Ambiguous company match**: Add an internal note listing the possible matches and stop.
- **Contact not found, no admin contact**: Assign the company only and note "No contacts found for [Company]. Company assigned; contact requires manual assignment."
- **Ticket already correctly assigned**: If the current company is NOT the Catchall company, add a note — "Ticket already assigned to [Company]. No changes made." — and stop.
- **Never guess**: Do not assign a contact based on a weak or partial name match alone. Require at least a name match within the correct company, or fall back to primary contact.