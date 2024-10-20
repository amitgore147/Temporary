To create a **script action**, an **event**, and a **notification** in ServiceNow to notify a caller of an incident, follow these steps:

### 1. **Create a Script Action**

A Script Action in ServiceNow is typically executed when an event is fired. We'll create a script action that runs when the event is triggered (e.g., incident creation or update).

#### Steps to create a Script Action:
1. Navigate to **System Policy > Events > Script Actions**.
2. Click **New** to create a new Script Action.
3. Fill out the required fields:
   - **Name**: NotifyCallerOfIncident (or a similar name)
   - **Table**: Incident
   - **Event Name**: (We will create the event next)
   - **Condition**: Add any conditions if necessary (e.g., only for Priority 1 incidents).
   - **Script**: Add the script logic to notify the caller.

Example script:
```javascript
(function executeRule(current, previous /*null when async*/) {
    // Get the caller's email
    var callerEmail = current.caller_id.email;
    var incidentNumber = current.number;
    
    // Trigger the notification
    gs.eventQueue('incident.notify.caller', current, callerEmail, incidentNumber);
})(current, previous);
```

4. Save the Script Action.

### 2. **Create an Event**

Events are triggered and can be used to notify users via notifications.

#### Steps to create an Event:
1. Navigate to **System Policy > Events > Registry**.
2. Click **New** to create a new event.
3. Fill out the fields:
   - **Name**: incident.notify.caller
   - **Table**: Incident
   - **Description**: Event to notify the caller when an incident is updated or created.
4. Save the event.

### 3. **Create a Notification**

A Notification in ServiceNow sends messages to users based on conditions or events.

#### Steps to create a Notification:
1. Navigate to **System Notification > Email > Notifications**.
2. Click **New** to create a new notification.
3. Fill out the required fields:
   - **Name**: Notify Caller of Incident
   - **Table**: Incident
   - **When to Send**: Trigger on the **Event** we created (`incident.notify.caller`).
   - **Who will Receive**: 
     - Add **Email (Script)** and use this script:
       ```javascript
       return event.parm1;
       ```
   - **What it will contain**: Write the email template to include relevant details.

Example message:
```
Hello,

An incident has been created/updated. Here are the details:

Incident Number: ${event.parm2}
Short Description: ${current.short_description}
Status: ${current.state}

Please contact the service desk for further details.

Thank you.
```

4. Save the notification.

### 4. **Test the Setup**
1. Create or update an incident in ServiceNow.
2. The event should fire, triggering the script action and sending the notification to the caller’s email.

This completes the setup for notifying the caller when an incident is created or updated.
