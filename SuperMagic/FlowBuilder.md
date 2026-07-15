==================================================
SKILL: Flow Builder Quick-Start
WHEN TO USE: Building a common automation flow step-by-step — e.g. auto-assign by keyword, escalate by priority, change status on condition, or notify a member.
==================================================
1. Clarify the goal — Ask the technician what the flow should do. Common examples:
   - Auto-assign tickets by keyword
   - Escalate tickets by priority
   - Change status when a condition is met
   - Notify a member when a ticket is created/updated

2. Discover filter attributes — Call list_flow_filter_attributes to find available filter fields (board, priority, status, keywords, contact type, etc.) and note their attribute_id and type.

3. Resolve filter values — For each attribute the flow will filter on, call list_flow_filter_attribute_values with the attribute_id to get valid values. Note the value (ID) and display_value (human-readable name) for each.

4. Discover available actions — Call list_flow_actions to see what the flow can do (assign member, change status, send notification, run skill, etc.). Note the action structure required.

5. Confirm the plan with the technician — Before building, summarise:
   - Trigger condition(s) (filters)
   - Action(s) to take
   - Any notifications to send
   Ask the technician to confirm before proceeding.

6. Build the flow — Call create_flow with:
   - A clear name and description
   - rules: each rule needs attribute_id, value, value_type, and display_value (required for object types)
   - For OR logic across multiple values of the same attribute, use a group with group_combinator: 'or'
   - actions: structured per the action list results
   - notifications: optional member/team alerts

7. Share the flow URL — Always include the flow_url link in your response so the technician can review and edit it in the admin panel.

8. Offer to save as a skill — If the technician built something reusable (e.g. "escalate Priority 1 tickets"), offer to save the pattern as a named skill for future use.

Important reminders:
- You can only CREATE and LIST flows — not edit or delete them. Direct the technician to the admin panel via flow_url for any modifications.
- Always use display_value for object-type attributes or the flow rule will be invalid.
- For "Run Skill" actions, set skill_id explicitly — do not rely on form_data.