API Bug Report – Benefits Dashboard application

This document lists all API defects and observations identified during testing.  
Each issue includes severity, labels, reproduction steps, expected vs actual results, and screenshot placeholders.

Bug 1: PUT Method creates a New Record When non‑existent ID is provided in the body section to Update
**Severity:** High  
**Label:** Functional / Data Integrity  
**Type:** API Behavior Defect  

Description
PUT requests with invalid or non‑existent IDs, creates new employee records instead of returning an error. This violates REST conventions and causes data duplication.

Steps to Reproduce
1. Create an employee via POST.
2. Modify the `id` to a random UUID.
3. Send a PUT request with below body content:
{
  "firstName": "Test151",
  "lastName": "Test151",
  "username": "Test151",
  "id": "039d90e9-5f24-4f8b-9b30-a30f15e307dd",
  "dependants": 32,
  "expiration": "2000-09-04T04:33:20.140Z",
  "salary": 0
}
4. Check the employee list.

Expected Result
- API should return **404 Not Found**.
- No new record should be created.

Actual Result
- API creates a new employee record.

Screenshot
*(screenshots/api_put_invalid_id_creates_new_row.png,screenshots/ui_invalid_id_creates_new_row.png)*

---

Bug 2: Under PUT method, it allows to set the salary field to Zero
**Severity:** High  
**Label:** Functional / Data Integrity  
**Type:** API Behavior Defect  

Description
When I'm updating the record with new id, the API allows creating a new employee with `"salary": 0`, which is unrealistic for payroll systems and may break calculations.

Steps to Reproduce
1. Modify the `id` in the PUT request to a new UUID.
2. Provide below body:
{
  "firstName": "InvalidRecordAPI",
  "lastName": "InvalidRecordAPI",
  "username": "InvalidRecordAPI",
  "id": "6116b128-32d6-ca0b-a611-364f26434f8f",
  "dependants": 32,
  "expiration": "2000-09-04T04:33:20.140Z",
  "salary": 0
3. Send the PUT request.
4. Observe the response.

Expected Result
- API should return **404 Not Found**.
- No new record should be created.
- API should reject salary = 0 with a validation error.

Actual Result
- API creates a new record.
- Salary is 0.

Screenshot
*(screenshots/api_put_new_id_salary_zero.png,screenshots/ui_new_id_salary_zero.png)*

---
Bug 3: PUT Method Allows Salary to be Set More than 52000(26*2000) When a New (Non‑Existent) ID Is Provided
**Severity:** High  
**Label:** Functional / Data Integrity  
**Type:** API Behavior Defect  

Description
When a PUT request is sent with a new (non‑existent) ID, the API creates a new record and allows setting salary to more than **52000** (26*2000).

Steps to Reproduce
1. Modify the `id` in the PUT request to a new UUID.
2. Provide below body :
{
  "firstName": "Test151",
  "lastName": "Test151",
  "username": "Test151",
  "id": "039d90e9-5f24-4f8b-9b30-a30f15e307dd",
  "dependants": 32,
  "expiration": "2000-09-04T04:33:20.140Z",
  "salary": 58000
}
3. Send the PUT request.
4. Observe the response.

Expected Result
- API schema should validate the number.(405 Method Not Allowed)
- No new record should be created.
- Salary should not default to more than 52000.

Actual Result
- API creates a new record.
- Salary becomes 58000.
{
    "partitionKey": "TestUser895",
    "sortKey": "525ae5a8-5d9b-48a7-be4e-1c0e11ff2aaf",
    "username": "TestUser895",
    "id": "525ae5a8-5d9b-48a7-be4e-1c0e11ff2aaf",
    "firstName": "Test151",
    "lastName": "Test151",
    "dependants": 32,
    "expiration": "2000-09-04T04:33:20.14Z",
    "salary": 58000,
    "gross": 2230.7693,
    "benefitsCost": 653.8462,
    "net": 1576.9231
}

Screenshot
(screenshots/api_put_salary_58000.png,screenshots/ui_salary_58000.png)

---
Bug 4: DELETE Allows Repeated Deletion of the Same ID
**Severity:** High  
**Label:** Functional / Data Integrity  
**Type:** API Behavior Defect  

Description
The DELETE endpoint returns success even when deleting an employee that no longer exists. This violates REST standards and misleads clients.

Steps to Reproduce
1. Delete an employee using DELETE.
2. Repeat the DELETE request with the same ID.
3. Observe the response.

Expected Result
- API should return **404 Not Found** for non‑existent IDs.

Actual Result
- API returns success for repeated deletes.

Screenshot
*(screenshots/api_delete_repeated_success.png)*

---

Observation 1: Salary Field Defaults to 52000 Even When a Value Is Provided
**Severity:** Medium  
**Label:** Functional / Business Rule Clarification  
**Type:** Observation  

Description
When creating an employee via POST, the salary value provided in the request body is ignored. The API always assigns a default salary of **52000**.

Steps to Reproduce
1. Send a POST request with `"salary": 90000`.
2. Check the response.

Expected Result
- API should return the salary value provided in the request.

Actual Result
- API returns `"salary": 52000` regardless of input.

Recommendation
Confirm whether salary should be user‑defined or system‑generated.

Screenshot
*(screenshots/api_salary_ignored_post.png)*

---

Observation 2: Employee ID in UI Does Not Match API ID
**Severity:** Low  
**Label:** Data Consistency / Business Rule Clarification  
**Type:** Observation  

Description
The employee ID displayed in the UI does not match the `id` returned by the API. UI appears to use a business identifier, while API uses a UUID.

Expected Result
- UI and API should use consistent identifiers, or clearly differentiate them.

Actual Result
- UI shows a different ID format.

Screenshot
*(screenshots/api_ui_id_mismatch.png)*

---
Observation 3: UI Shows Duplicate Records After API Delete
**Severity:** Medium  
**Label:** Data Sync / UI-API Consistency  
**Type:** Observation  

Description
After deleting an employee via API, the UI displays duplicate records. This suggests a caching or refresh issue between UI and API.

Steps to Reproduce
1. Delete an employee using DELETE API.
2. Refresh the UI.
3. Observe duplicate entries.

Expected Result
- UI should reflect the updated list without duplicates.

Actual Result
- UI shows duplicate rows.

Screenshot
*(screenshots/api_delete_ui_duplicate.png)*

---

End of API Bug Report
