==================================================
SKILL: Intent Setup Walkthrough
WHEN TO USE: Creating a new intent from scratch — guides through naming, description, variations, replies, and arguments step by step.
==================================================
# Intent Setup Walkthrough

## Step 1 — Check for Duplicates
1. Call list_intents (omit search) to get the full list of existing intents.
2. Scan for any intent with a similar name or purpose to what the user wants to create.
3. If a duplicate or near-duplicate exists, show it with a link and ask the user to confirm they still want to create a new one before proceeding.

## Step 2 — Gather Intent Basics
Ask the user for the following if not already provided:
- Name (max 64 characters, must be unique)
- Description (what this intent does and when it should fire; max 1024 characters)
- Time saved (estimated minutes saved per trigger — default 0 if unsure)
- Active on creation? (default: yes)

## Step 3 — Gather Default Variation Details
Ask the user if they want to configure the default variation now. If yes, collect:
- External reply — the message shown to the end customer after the intent fires
- Internal reply — the technician-only note shown after the intent fires
- Webhook URL (optional) — the endpoint called when the intent is triggered
- Is resolution? — should the external/internal reply ask the customer if their issue is resolved? (true/false for each)

## Step 4 — Gather Arguments (Form Fields)
Ask the user if this intent should collect information from the customer before firing.
If yes, for each argument collect:
- Name — field label shown to the customer
- Description — helper text explaining what to enter
- Type — one of: text, number, date, email, boolean, multiple
- Required? — is this field mandatory?
- Order — display order (1, 2, 3…)
- For multiple type: list the choices (type_data)

Repeat until the user says they have no more arguments to add.

## Step 5 — Confirm Before Creating
Summarise everything collected:
- Intent name, description, time_saved, is_active
- Variation: external reply, internal reply, URL (if any)
- Arguments list (if any)

Ask the user to confirm the details are correct before proceeding.

## Step 6 — Create the Intent
Call create_intent with:
- name, description, time_saved, is_active
- url (if provided), external_reply, internal_reply (if provided)
- arguments array (if any were collected)

## Step 7 — Update Replies (if needed)
If the user specified is_resolution values for the replies, call set_variation_replies with the returned intent_id and variation_id to apply them (create_intent does not accept is_resolution directly).

## Step 8 — Confirm and Link
1. Confirm the intent was created successfully.
2. Show a clickable link: name
3. Ask the user if they want to:
   - Add more variations
   - Adjust visibility scope on the default variation (call update_variation)
   - Set arguments on a specific variation (call set_variation_arguments)
   - Activate or deactivate the intent