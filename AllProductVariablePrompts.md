## This document is intended to explain what data we send to OpenAI for each feature we have from MagicAI and their variable names in code.

## Auto Prioritization

- Contact Company Name `{{company_name}}`
- Ticket Contact Name `{{contact_name}}`
- Contact Type (if present). `{{contact_type}}`
- Ticket Summary `{{summary}}`
- Ticket Chat History `{{ticket_chat}}` or `{{chat}}`
- Ticket first note `{{first_note}}`
- Ticket Created At `{{created_at}}`
- Ticket agreement `{{ticket_agreement}}`

## Auto Categorization

- Ticket Summary `{{ticket_summary}}`
- Ticket First Note `{{first_note}}`
- Contact Type (if present) `{{contact_type}}`
- Contact Company Name `{{company_name}}`
- Ticket Chat History `{{ticket_chat}}`
- Ticket agreement `{{ticket_agreement}}`
- Category Tree (depends on the category level we are sending to AI) `{{type_subtype_item_category_tree}}` or `{{type_subtype_category_tree}}` or `{{subtype_item_category_tree}}` or `{{item_category_tree}}`

## Generate Title

- Ticket Chat History `{{ticket_chat}}`
- Original Ticket summary `{{ticket_summary}}`
- Ticket agreement `{{ticket_agreement}}`
- Current date & time `{{current_datetime}}`
- Custom Instructions `{{custom_instructions}}`
- Contact Company Name (We refer as User's company name) `{{contact_company_name}}`
- Contact Name (We refer as User's Name in the prompt) `{{contact_name}}`

## Generate Recap

- Ticket summary `{{ticket_summary}}`
- Ticket Configuration array `{{ticket_configuration}}`
- Contact Type (if present) `{{contact_type}}`
- Ticket Priority `{{priority}}`
- Ticket Status `{{status}}`
- Ticket Created at `{{created_at}}`
- Ticket System Id `{{system_id}}`
- Ticket Board `{{board}}`
- Ticket Category (type, subtype, item) `{{type}}`, `{{sub_type}}`, `{{item}}`
- Ticket Chat History `{{chat}}`
- Recap Rules (Set by company) `{{recap_rules}}`
- Contact Full Name `{{contact_fullname}}`
- Member Full Name `{{member_fullname}}`
- Ticket agreement `{{ticket_agreement}}`

## Time Entry

- Ticket Assignee Name (Member's name) `{{member}}`
- Ticket Contact Name `{{contact}}`
- Ticket Members Names (Other Members names) `{{collabMembers}}`
- Ticket Chat History `{{chat}}`
- Contact Type (if present) `{{contact_type}}`

## Chat With Magic

- Ticket Summary `{{ticket_summary}}`
- Ticket Assignee Name (Member's name) `{{assignee_name}}`
- Ticket Contact Name `{{contact_name}}`
- Ticket Configuration list `{{ticket_Configuration}}`
- Ticket Chat History `{{ticket_chat}}`
- Contact Type (if present) `{{contact_type}}`
- Ticket agreement `{{ticket_agreement}}`

## Magic Agent

- Contact Name `{{contact_name}}`
- Contact Email `{{contact_email}}`
- Ticket source `{{ticket_source}}`
- Ticket agreement `{{ticket_agreement}}`
- Current Date time `{{current_datetime}}`
- Triage instructions `{{triage_instructions}}` (former Master intent instructions)
- Ticket Priority `{{priority}}`
- Contact company name `{{contact_company_name}}`
- Contact type (if present) `{{contact_type}}`
- Is the request inside or outside working hours `{{working_hours_flag}}` (Boolean Flag)
NOTE: 
- Ticket title `{{ticket_summary}}`

## Reminder Agent

- Contact Name `{{contact_name}}`
- Ticket Chat History `{{ticket_chat}}`
- Ticket title `{{ticket_summary}}`
- Instructions (prompt of nth reminder message) `{{custom_instructions}}`

## Sentiment processing

- Ticket Chat History `{{segment_notes}}` - recent 3 contact & member notes
- Current date & time `{{current_datetime}}`

## Flows / Internal Notes

- Ticket ID`{Ticket ID}`
- Ticket Summary`{Ticket Summary}`
- Full Name`{Full Name}`