1. For each new incoming thread on the ticket:
   a. Search for other open tickets where both the contact and company are the same as the current ticket.
   b. Compare the issues described in these tickets to the current ticket. Only continue if the issues are determined to be the same.
2. If a matching ticket with the same contact, company, and issue is found:
   a. Automatically merge the threads.
   b. After merging, add an internal note to the merged ticket stating: "Tickets were merged because they were for the same issue from the same contact and company."
   c. In the closed (child) thread, add an internal note linking to the parent thread.

Constraints:
- Only merge tickets if both the contact and company match and the issue is the same.
- Always leave an internal note after merging, explaining the action.
- Never merge tickets if the issues are not the same.