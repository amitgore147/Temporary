To create a Script Action in ServiceNow to notify a caller of an incident by selecting an existing event, follow these steps:

### Step 1: Navigate to "Script Actions"
1. Log into your ServiceNow instance.
2. In the left-hand navigation menu, type **"Script Actions"** in the filter navigator.
3. Click on **Script Actions** under **System Policy**.

### Step 2: Create a New Script Action
1. In the Script Actions list, click **New** to create a new Script Action.
2. Fill in the fields:
   - **Name**: Enter a name for the script action (e.g., **Notify Caller of Incident**).
   - **Event Name**: Use the magnifying glass icon to search for and select an existing event, such as **incident.assigned** or a custom event if it's already created.
   - **Condition**: Add any condition to trigger this action (optional).

### Step 3: Write the Script
1. Scroll down to the **Script** field, where you'll write the JavaScript code to send a notification. A sample script could look like this:

```javascript
(function() {
    // Get the incident record
    var inc = new GlideRecord('incident');
    inc.get(event.instance);
    
    // Prepare notification details
    var callerId = inc.caller_id; // Get the caller ID
    var incidentNumber = inc.number; // Get the incident number
    
    // Prepare a notification message (Modify as per your requirement)
    var message = 'Incident ' + incidentNumber + ' has been updated.';
    
    // Send notification (can be a custom email, SMS, or other channels as configured)
    gs.eventQueue('incident.notify_caller', inc, callerId, message);
})();
```

   Here’s what this script does:
   - It retrieves the **incident** record.
   - Extracts the **caller_id** and **incident number** from the record.
   - Sends a notification message to the caller using the `gs.eventQueue` method.

### Step 4: Save the Script Action
1. After entering your script, click **Submit** to save the script action.
2. Your new Script Action is now created and will trigger based on the selected event.

### Step 5: Test the Script Action
1. To ensure the script is working, you can create or update an incident that meets the event criteria (like incident assignment) and check if the notification is sent to the caller.
  
By following these steps, you can create a custom Script Action to notify callers of an incident in ServiceNow. You can adjust the notification logic based on your specific business requirements.
