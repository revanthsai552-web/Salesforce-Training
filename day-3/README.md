# Salesforce Training – Day 3

## 1. Which requirements did you solve using Flow?

We used **Record-Triggered Flows** to automate business processes.

### Before-Save Flow
- Automatically populate the **Application Date**.
- Prevent duplicate applications using a **Custom Error**.

### After-Save Flow
- Send an email notification to the **Placement Officer** when a new application is created.
- Automatically create an **Offer Letter** record when the application's **Status** changes to **Accepted**.

---

## 2. Which requirements required Validation Rules?

The following business rules were implemented using **Validation Rules**:

- Student **CGPA** must be greater than or equal to the company's **Minimum CGPA**.
- **Application Date** cannot be later than the company's **Closing Date**.
- Mandatory fields cannot be left blank before saving a record.

---

## 3. Which requirements still needed Apex?

For this project, **Apex was not required**.

All the required functionality was implemented using Salesforce's declarative features, including:

- Record-Triggered Flows
- Validation Rules
- Email Notifications
- Custom Objects

---

## 4. Why did you choose those solutions?

### Record-Triggered Flows
- Automate business processes without writing code.
- Easy to maintain and update.
- Salesforce-recommended solution for process automation.

### Validation Rules
- Enforce business rules and maintain data quality.
- Prevent invalid records from being saved.
- Provide immediate feedback to users.

### Apex
- Not required because all project requirements were achieved using declarative Salesforce features.
- Apex is recommended only when business requirements involve complex logic that cannot be implemented using Flows or Validation Rules.
