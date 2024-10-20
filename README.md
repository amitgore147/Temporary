To create a "Notify Caller of Incident" Script Action in ServiceNow, you'll need to follow these steps. Script Actions are typically used to trigger server-side scripts that respond to certain events, such as when an incident is created or updated. Here's a step-by-step guide:

### Step 1: Log in to ServiceNow
1. Log in to your ServiceNow instance with sufficient permissions (e.g., admin or developer role).

### Step 2: Navigate to Script Actions
1. In the left-hand application navigator, type **"Script Actions"** in the search bar.
2. Click **Script Actions** under the **System Policy** module.

### Step 3: Create a New Script Action
1. Click **New** to create a new Script Action.

### Step 4: Fill in the Script Action Details
1. **Name**: Enter a name for the Script Action, e.g., "Notify Caller of Incident".
2. **Table**: Select the **Incident** table from the dropdown (this is the table that the script will be triggered from).
3. **Condition**: If you want to trigger the notification when a specific condition is met, you can write a condition script here. For example:
   ```javascript
   current.state == 2 && current.caller_id // Notify when incident state changes to "In Progress" and caller exists
   ```

### Step 5: Write the Script
In the **Script** field, write the script that will notify the caller. Below is an example that sends an email to the caller:

```javascript
// Check if the incident has a valid caller
if (current.caller_id) {
    var email = new GlideEmailOutbound(); // Instantiate the email object
    email.setFrom('support@example.com'); // Set the sender's email address
    email.setSubject('Incident Update: ' + current.number); // Set the subject of the email
    email.setBody('Hello ' + current.caller_id.getDisplayValue() + ',\n\n' + 
                  'Your incident (' + current.number + ') has been updated to the following state: ' + 
                  current.getDisplayValue('state') + '.\n\n' + 
                  'Please contact us if you need further assistance.\n\nRegards,\nService Desk');
    email.setTo(current.caller_id.email); // Set the recipient's email address
    email.send(); // Send the email
}
```

### Step 6: Save the Script Action
1. Click **Submit** or **Update** to save the Script Action.

### Step 7: Set Up a Business Rule (Optional)
You can use a **Business Rule** to trigger this Script Action. Follow these steps:

1. Navigate to **Business Rules** in the left-hand application navigator.
2. Click **New** to create a new Business Rule.
3. In the **When to run** section, set:
   - **Table**: Incident
   - **When**: After (to run the rule after the incident is updated)
4. In the **Advanced** section, write the script to trigger the Script Action, like this:
   ```javascript
   gs.eventQueue('incident.notify_caller', current, current.caller_id, current.number);
   ```

5. Save the Business Rule.

### Step 8: Test the Script Action
1. Create or update an incident that meets your conditions (e.g., an incident with a valid caller and a certain state).
2. The caller should receive an email notification as defined in the Script Action.

This process allows you to notify callers automatically whenever an incident is created or updated based on the condition set in your Script Action.
