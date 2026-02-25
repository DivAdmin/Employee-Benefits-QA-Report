API Bug Report – Paylocity Benefits Dashboard Application

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
  }
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
Bug 4: The DELETE endpoint returns success even when deleting an employee that no longer exists or the Same ID
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
------

Bug 5: GET Employee Returns Success for Non‑Existent or Invalid ID
**Severity**: High
**Label**: Functional / Data Integrity
**Type**: API Behavior Defect

Description
When performing a GET request for a specific employee by ID (passed through headers or URL), the API returns a successful response even when the ID does not exist in the system.
A well‑designed REST API should return 404 Not Found when the requested resource is missing.
Returning 200 OK for an invalid or non‑existent ID misleads clients and prevents proper error handling.

Steps to Reproduce
1.Capture a valid GET request for an existing employee.
2.Modify the ID in the request header or URL to a random, non‑existent value.
3.Send the GET request.
4.Observe the response.

Expected Result
1.API should return 404 Not Found with a clear error message such as:
"Employee not found"
2.No employee object should be returned.

Actual Result
1.API returns 200 OK (or another success status).
2.Response body may be empty, defaulted, or inconsistent.
3.No error message is provided.

Screenshot
(screenshots/api_get_invalid_id.png, screenshots/api_get_non_existing_id.png)
---
Bug 6: DELETE Returns “1” and 200 OK for Both Valid and Invalid IDs
**Severity**: High
**Label**: Functional / Data Integrity
**Type**: API Behavior Defect

Description
The DELETE endpoint returns a 200 OK status and a response body of 1 regardless of whether the employee ID exists.
For valid IDs, the employee is deleted but the response is still “1”.
For invalid or non‑existent IDs, the API still returns “1” and “200 OK”, indicating a successful operation even though no deletion occurred.

This behavior violates REST standards and prevents clients from distinguishing between successful and failed delete operations.

Steps to Reproduce
1.Create an employee via POST.
2.Delete the employee using DELETE → observe response.
3.Repeat the DELETE request with the same ID.
4.Delete a completely random or invalid ID.
5.Observe that all responses return “1” and “200 OK”.

Expected Result
1.Valid ID → 200 OK or 204 No Content, with a meaningful message.
2.Invalid or non‑existent ID → 404 Not Found with an error message.
3.Response body should not be a raw integer.

Actual Result
1.API returns 1 and 200 OK for both valid and invalid IDs.
3.2.No error message is provided.
No distinction between successful and failed deletions.

Screenshot
(screenshots/api_delete_invalid_id_returns_1.png,screenshots/api_get_non_existing_id.png)

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

