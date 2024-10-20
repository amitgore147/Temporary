To create an event, business rule (BR), and notification in ServiceNow that triggers when the priority, impact, or urgency fields are updated, follow these steps:

### 1. **Create the Event:**
   - Navigate to **System Policy > Events > Event Registry**.
   - Click on **New** to create a new event.
   - Fill in the fields:
     - **Name**: Provide a unique event name, for example, `incident.priority.impact.urgency.changed`.
     - **Table**: Select the table on which the event will occur, e.g., `Incident`.
     - **Description**: Add a meaningful description for reference.
   - Click **Submit**.

### 2. **Create a Business Rule (BR) to Trigger the Event:**
   - Navigate to **System Definition > Business Rules**.
   - Click on **New** to create a new business rule.
   - Fill in the fields:
     - **Name**: Provide a meaningful name, e.g., `Trigger Event on Priority, Impact, or Urgency Update`.
     - **Table**: Select the `Incident` table.
     - **When**: Set this to **before** or **after** based on when you want the event to trigger.
   - Under **Conditions**:
     - Add the condition: `current.priority.changes() || current.impact.changes() || current.urgency.changes()`.
   - Under the **Advanced** tab, add the following script in the **Script** section:
     ```javascript
     if (current.priority.changes() || current.impact.changes() || current.urgency.changes()) {
         gs.eventQueue('incident.priority.impact.urgency.changed', current, gs.getUserID(), gs.getUserName());
     }
     ```
   - Click **Submit**.

### 3. **Create the Notification:**
   - Navigate to **System Notification > Email > Notifications**.
   - Click on **New** to create a new notification.
   - Fill in the fields:
     - **Name**: Provide a meaningful name, e.g., `Notify on Priority, Impact, or Urgency Change`.
     - **Table**: Select `Incident`.
     - **When to send**: Choose **Event is fired**.
     - **Event name**: Select the event you created earlier, e.g., `incident.priority.impact.urgency.changed`.
   - Under **Who will receive**:
     - Choose recipients (e.g., assignment group, users in roles, etc.).
   - Under **What it will contain**:
     - Fill in the subject and message body with relevant information.
     - Use placeholders for dynamic content, e.g., `Priority has been updated to ${priority}`.
   - Click **Submit**.

This setup will trigger the notification whenever the `priority`, `impact`, or `urgency` field on the `Incident` table is updated.
