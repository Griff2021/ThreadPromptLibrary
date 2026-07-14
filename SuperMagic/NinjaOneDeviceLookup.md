You are performing an automated device lookup for a support thread.
Follow these steps in order:

1. Extract the end user's name, email, and/or device name from 
   the thread.

2. Look up the contact in Thread using the end user's name or email:
   - Check the contact's memory/intelligence for any stored device 
     names, hostnames, or asset tags associated with them
   - If a device name or hostname is found in contact intelligence, 
     use that as your primary search term in NinjaOne

3. If the thread body references a specific device name or hostname 
   directly, use that as your primary search term — it takes 
   priority over contact intelligence.

4. Search NinjaOne for the device:
   - First, resolve the client's NinjaOne organization using the 
     client company name
   - Search for the device using the hostname or device name 
     extracted in steps 2–3
   - If no device name is available, search by the end user's 
     name to match by last logged-in user
   - If the organization-scoped search returns no results, fall 
     back to a global search across all organizations

5. Retrieve the device details including:
   - Device name and model
   - Operating system and version
   - Last seen / online status
   - Any active alerts on the device

6. Add an internal note to the thread with:
   - Device name, OS, and last contact time
   - Any active NinjaOne alerts (list them clearly)
   - The source of the device match (thread body, contact 
     intelligence, or username match) so the tech knows how 
     confident the match is
   - A direct link to the device in NinjaOne for remote access

7. If no device is found after both organization-scoped and global 
   searches, note that in the internal note and suggest the 
   technician verify the device name in contact intelligence or 
   check NinjaOne manually.

8. If active critical alerts are found on the device, recommend 
   escalating the priority to "Priority 2 - Quick Response".