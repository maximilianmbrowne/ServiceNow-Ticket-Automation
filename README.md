# ServiceNow Ticket Automation Lab 🚀
This repository contains the resources for my video tutorial on automating ServiceNow incident creation using Windows PowerShell. This lab demonstrates how to leverage the ServiceNow REST API to improve efficiency in IT Service Management (ITSM).

📺 Project Overview
The goal of this project is to automate the manual process of logging common IT issues (like account lockouts) by sending data directly from a local workstation to a ServiceNow instance.

🛠️ The PowerShell Script
The script below connects to a ServiceNow Personal Developer Instance (PDI) and creates a High-Impact, High-Urgency ticket.

PowerShell

# Define the API endpoint for the Incident Table
$uri = "https://dev285513.service-now.com/api/now/table/incident"

# Authentication Credentials
$username = "admin"
$password = "WKk!-B8bBi7x"

# Define Ticket Details (Payload)
$body = @{
    short_description = "User locked out of their account"
    description       = "User put in wrong password too many times and is unable to reset their own password"
    category          = "Software"
    subcategory       = "Application"
    impact            = "1"
    urgency           = "1"
} | ConvertTo-Json

# Execute the REST API Request
$Response = Invoke-RestMethod -Uri $uri `
                  -Method Post `
                  -Headers @{Accept = "application/json"; 'Content-Type' = "application/json"} `
                  -Credential (New-Object System.Management.Automation.PSCredential($username, (ConvertTo-SecureString $password -AsPlainText -Force))) `
                  -Body $body

# Display the result (The Task Effective Number)
$Response.result | Format-List -Property *
📝 Lab Steps
1. Write the Script
We begin by drafting the PowerShell script. We use a HashTable to define our incident properties and then use ConvertTo-Json so the ServiceNow API can interpret the data.

2. Execute in PowerShell
Run the script in your terminal. This sends a POST request. If successful, ServiceNow processes the request and returns a JSON response containing the details of the newly created record.

3. Obtain the Task Effective Number
Once the script finishes, look at the output in the terminal. Locate the number property (e.g., INC0010023). This is the unique identifier for your ticket.

4. Verify in ServiceNow
Open your ServiceNow Instance:

Navigate to Incidents > All.

Search for the Task Effective Number you obtained from PowerShell.

Confirm that the Short Description and Category match what was sent in the script.

5. Final Ticket Adjustments
Review the ticket within the ServiceNow UI. In this stage, you can manually assign the ticket to a specific group, add work notes, or resolve the incident once the user is unlocked.