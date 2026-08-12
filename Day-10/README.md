# Day 10 – Student Placement Portal

## Outline

1. Student Selection
2. Student Profile Display
3. Eligibility Checking
4. Eligible Jobs Display
5. Company Details
6. Application Submission
7. Duplicate Application Handling
8. Empty State for No Eligible Jobs
9. Student Profile Update
10. Testing and Deployment

## Work Completed

- Built the Student Placement Portal using LWC.
- Added student selection and student details display.
- Added eligible jobs and company information.
- Added View Details and Apply functionality.
- Added application success and error handling.
- Added duplicate application validation.
- Added Empty State when no eligible jobs are available.
- Created Student Profile LWC for updating student details.
- Tested different eligibility and application scenarios.
- Successfully deployed the components and Apex changes.

## Technologies Used

- Salesforce Lightning Web Components (LWC)
- Apex
- SOQL
- Lightning Data Service
- Salesforce UI Record API

## Application Flow

```text
Start
  ↓
Select Student
  ↓
Display Student Details
  ↓
Check Eligibility
  ↓
Eligible Jobs Available?
  ├── Yes → Display Eligible Jobs
  │          ↓
  │       View Details / Apply
  │          ↓
  │       Application Submitted
  │
  └── No → Display Empty State
             ↓
          Update Profile
             ↓
        Student Profile
             ↓
        Save Profile
