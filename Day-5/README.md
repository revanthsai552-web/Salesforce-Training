# Placement Management – Application Trigger

## Overview

The `ApplicationTrigger` is an Apex Trigger created for the **Placement Management System** in Salesforce.

This trigger runs on the `Application__c` object **before a new application is inserted**. It automatically validates whether a student is eligible to apply for a particular company based on the company's requirements.

The trigger also prevents duplicate applications and applications submitted after the company's closing date.

---

## Trigger Details

**Trigger Name:** `ApplicationTrigger`

**Object:** `Application__c`

**Event:** `before insert`

```apex
trigger ApplicationTrigger on Application__c (before insert)
```

The trigger executes before an application record is saved to Salesforce.

---

## Objects Used

### Student__c

The trigger retrieves the following student details:

* `Cgpa__c` – Student's CGPA
* `Branch__c` – Student's branch
* `Active_Backlogs__c` – Number of active backlogs
* `Gradutaion_year__c` – Student's graduation year

### Company__c

The trigger retrieves the following company requirements:

* `Minimum_Cgpa__c` – Minimum required CGPA
* `Eligible_Branch__c` – Eligible branch
* `Maximum_Active_Backlogs__c` – Maximum allowed active backlogs
* `Graduation_Year__c` – Required graduation year
* `Closing_Date__c` – Last date to apply

### Application__c

The application record contains:

* `Student__c` – Student lookup
* `Company__c` – Company lookup
* `Status__c` – Application status

---

# What We Have Implemented

## 1. Collect Student and Company IDs

The trigger first collects the Student and Company record IDs from the applications being inserted.

This allows the trigger to query all required records together instead of performing SOQL queries inside the loop.

```apex
Set<Id> studentIds = new Set<Id>();
Set<Id> companyIds = new Set<Id>();
```

This approach makes the trigger **bulk-safe** and helps avoid Salesforce governor-limit issues.

---

## 2. Retrieve Student Information

The trigger queries the required fields from `Student__c` and stores them in a map.

```apex
Map<Id, Student__c> studentMap
```

The map allows the trigger to quickly find the student related to each application.

---

## 3. Retrieve Company Requirements

The trigger queries the required fields from `Company__c` and stores them in another map.

```apex
Map<Id, Company__c> companyMap
```

This allows the student's details to be compared with the company's eligibility requirements.

---

# Eligibility Validations

The trigger performs multiple eligibility checks before allowing an application to be created.

## 4. CGPA Validation

The student's CGPA is compared with the company's minimum required CGPA.

```apex
student.Cgpa__c < company.Minimum_Cgpa__c
```

If the student's CGPA is lower than the company's requirement, the application is rejected.

**Error message:**

> Application rejected: Student CGPA is below the minimum CGPA required by this company.

---

## 5. Branch Validation

The student's branch is compared with the branch accepted by the company.

```apex
student.Branch__c != company.Eligible_Branch__c
```

If the student's branch does not match the company's eligible branch, the application is rejected.

**Error message:**

> Application rejected: Student branch is not eligible for this company.

---

## 6. Active Backlogs Validation

The student's active backlogs are compared with the maximum number of backlogs allowed by the company.

```apex
student.Active_Backlogs__c > company.Maximum_Active_Backlogs__c
```

If the student has more active backlogs than allowed, the application is rejected.

**Error message:**

> Application rejected: Student has more active backlogs than allowed by this company.

---

## 7. Graduation Year Validation

The student's graduation year is compared with the company's required graduation year.

```apex
student.Gradutaion_year__c != company.Graduation_Year__c
```

If the graduation years do not match, the application is rejected.

**Error message:**

> Application rejected: Student graduation year does not match the company requirement.

---

## 8. Closing Date Validation

The trigger checks whether the company's application deadline has already passed.

```apex
Date.today() > company.Closing_Date__c
```

If the current date is after the company's closing date, the application cannot be submitted.

**Error message:**

> Applications for this company are closed.

---

# Duplicate Application Prevention

The trigger also prevents a student from applying to the same company more than once.

Existing applications are queried using:

```apex
SELECT Student__c, Company__c
FROM Application__c
WHERE Student__c IN :studentIds
AND Company__c IN :companyIds
```

A unique combination of Student ID and Company ID is created:

```apex
StudentId-CompanyId
```

If the combination already exists, the trigger prevents the new application.

**Error message:**

> This student has already applied to this company.

---

# Default Application Status

Whenever a valid application is created, the trigger automatically sets the application status to:

```apex
app.Status__c = 'Applied';
```

Therefore, every newly created application starts with the **Applied** status.

---

# Error Handling

The trigger uses:

```apex
app.addError()
```

When an eligibility condition fails, Salesforce prevents the application from being inserted and displays the appropriate error message to the user.

For example:

```apex
app.addError(
    'Application rejected: Student CGPA is below the minimum CGPA required by this company.'
);
```

This ensures that only eligible applications are saved.

---

# Bulkification

The trigger is designed using Salesforce bulkification principles.

Instead of querying Student and Company records separately for every application, it:

1. Collects all Student IDs.
2. Collects all Company IDs.
3. Performs one query for Students.
4. Performs one query for Companies.
5. Performs one query for existing applications.
6. Processes all applications using maps and sets.

This makes the trigger suitable for processing multiple application records at the same time.

---

# Validation Flow

The overall validation process is:

```text
New Application
       |
       ↓
Get Student Details
       |
       ↓
Get Company Requirements
       |
       ↓
Check CGPA
       |
       ↓
Check Branch
       |
       ↓
Check Active Backlogs
       |
       ↓
Check Graduation Year
       |
       ↓
Check Closing Date
       |
       ↓
Check Duplicate Application
       |
       ↓
All Valid?
    /      \
  No        Yes
  |          |
addError   Status = Applied
  |          |
Rejected   Application Created
```

---

# Technologies Used

* Salesforce
* Apex
* Apex Triggers
* SOQL
* Salesforce Objects
* Lookup Relationships
* Sets
* Maps
* Governor Limit–friendly bulk processing

---

# Key Features

* ✅ CGPA eligibility checking
* ✅ Branch eligibility checking
* ✅ Active backlog validation
* ✅ Graduation year validation
* ✅ Company application deadline validation
* ✅ Duplicate application prevention
* ✅ Automatic default application status
* ✅ Error messages using `addError()`
* ✅ Bulkified Apex trigger
* ✅ Student and Company data handled using Maps
* ✅ Multiple applications can be processed in a single transaction

---

# Purpose of the Trigger

The main purpose of this trigger is to **automate the student-company eligibility process** in the Placement Management System.

Instead of manually checking every student's eligibility, Salesforce automatically verifies the student's:

**CGPA + Branch + Active Backlogs + Graduation Year + Application Deadline**

before allowing the application to be created.

This helps ensure that only eligible students can apply for suitable placement opportunities.
