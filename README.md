# Salesforce-Training
1. Which requirements did you solve using Flow?

We used Record-Triggered Flows for automation.

Before-Save Flow
Automatically populate Application Date.
Prevent duplicate applications using Custom Error.

After-Save Flow
Send an email notification to the Placement Officer when an application is created.
Automatically create an Offer Letter record when the application's Status becomes Accepted.

2. Which requirements required Validation Rules?

The following were implemented using Validation Rules:

Student CGPA must be greater than or equal to the Company's minimum CGPA.
Application Date cannot be after the Company Closing Date.
Mandatory fields cannot be left blank.

3. Which requirements still needed Apex?

For your project, none of the listed requirements require Apex.

Everything can be implemented using:

Record-Triggered Flows
Validation Rules
Email Alerts
Custom Objects

4. Why did you choose those solutions?
Record-Triggered Flows
Best for automating business processes.
No coding required.
Easy to maintain and modify.
Salesforce-recommended solution for automation.
Validation Rules
Best for enforcing data quality.
Prevent invalid records from being saved.
Provide immediate feedback to users.
Apex
Not required because all project requirements can be achieved declaratively.
Apex should be used only when Flow and Validation Rules cannot satisfy the business requirement or when complex custom logic is needed.
