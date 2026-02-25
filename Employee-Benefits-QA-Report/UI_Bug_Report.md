UI Bug Report – Paylocity Benefits Dashboard application

This document lists all UI defects and observations identified during testing.

Bug 1: No Error Message Displayed When Invalid Inputs Are Entered in Add Employee Form
**Severity:** Medium  
**Type:** Validation Defect  


Description:
When invalid data is entered in the Add Employee form, the application does not display any validation error messages.

Steps to Reproduce:
1. Log in to the application with valid credentials. 
2. Click on **Add Employee** button. 
3. Enter invalid inputs in the fields FirstName, LastName and Dependants.(e.g., numbers in name fields, negative dependants, invalid characters).
4. Click **Add** button.
5. Observe that no validation message is displayed.

Expected Result:
- Clear validation messages should appear for each invalid field.

Actual Result:
- No error messages are shown.
- User receives no feedback.

Screenshot:
(screenshots/ui_invalid_input_no_error.png)

---

Bug 2: No Error Message When Required Fields Are Left Blank
**Severity:** Medium  
**Type:** Validation Defect  

Description
The Add Employee form allows submission attempts without showing any required-field validation messages.

Steps to Reproduce
1. Log in to the application with valid credentials. 
2. Click on **Add Employee** button. 
3. Leave all fields blank.
4. Click **Add** button.
5. Observe that no validation message is displayed.

Expected Result
- Required-field validation messages should appear.

Actual Result
- No validation messages are displayed.

Screenshot
(screenshots/ui_missing_input_no_error.png)

---

Bug 3: Data Refresh Failure After Session Idle Time.
**Severity:** Medium  
**Type:** Session Handling Defect  

Description:
After the application is idle for more than 5 minutes, you click on Paylocity Benefits Dashboard Button, the page becomes unresponsive and no data is displayed. No timeout or session-expired message is shown.

Steps to Reproduce
1. Log in to the application with valid credentials. 
2. Leave the application idle for 5+ minutes.
3. Click on Paylocity Benefits Dashboard Button
3. Attempt to interact with the page.

Expected Result
- A session timeout or “Please refresh” message should appear.
- Application should remain responsive.
- Automatic LogOut 

Actual Result
- Page becomes unresponsive.
- No data is shown.

Screenshot
(screenshots/ui_idle_unresponsive.png)

---

Bug 4: 405 Error When Invalid Input is given in Username or Password field(Login Page)
**Severity:** High  
**Type:** Validation Defect  

Description :
Submitting the login form with an invalid usernname or password field results in a **405 Method Not Allowed** error instead of a validation message.

Steps to Reproduce:
1. Open the login page.
2. Enter a Invalid username and password.
3. Click **Login**.

Expected Result :
- UI should show Error or Validation Message

Actual Result :
- Application throws a 405 error.

Screenshot:
(screenshots/ui_405_error.png)

---

Bug 5: After Long Inactivity, Add Button Becomes Unresponsive 
**Severity:** Medium  
**Type:** UI Responsiveness Defect  

Description :
After the user is inactive for a long time, the **Add** button becomes unresponsive.  
However, the input fields still accept text.

Steps to Reproduce :
1. Log in to the application with valid credentials. 
2. Leave the application idle for several minutes.
3. Click on **Add Employee** button. 
4. Enter FirstName, LastName and Dependants fields
4. Try clicking **Add**.

Expected Result:
- Add button should remain functional.
- If session expired, a message should appear.

Actual Result:
- Add button does not respond.
- No error or timeout message is shown.

Screenshot:
(screenshots/ui_add_button_unresponsive.png)

---
Bug 6: Delete Functionality Freezes After Deleting Multiple Rows, when performed consecutively
**Severity:**: High
**Type**: UI Responsiveness Defect

Description:  
When the user deletes several employee records in rapid succession, the application becomes unresponsive.
The delete action stops working, and the UI freezes until the page is refreshed.

Steps to Reproduce:
1.Log in to the application with valid credentials.
2.Delete an employee record using the Delete (X) action.
3.Immediately delete additional rows one after another.
4.Observe the UI after 2–3 consecutive deletions.

Expected Result:
1.Each delete action should complete successfully.
2.The UI should remain responsive after multiple deletions.

Actual Result:
1.The UI freezes after several rapid deletions.
2.Delete button becomes unresponsive.
3.No error message or feedback is displayed.
4.User must refresh the page to continue.

Screenshot:  
(screenshots/ui_delete_freeze.png)

---
Observation 1: Dependant Maximum Value Set to 32
**Severity:** Low  
**Type:** Business Rule Clarification Needed  

Description:
The UI allows dependants up to 32, which is unusually high for real-world HR scenarios.

Recommendation :
Confirm intended business rule with product owner.

Screenshot :
(screenshots/ui_dependants_limit.png)

---

Observation 2: A Specific Button to Refresh or Fetch Latest Data
**Severity:** Low  
**Type:** UX Improvement  

Description:
When I click on Paylocity Benefits Dashboard Button, it gets latest data. 
But providing a Specific button to refresh or fetch the latest employee data would improve usability.

Recommendation:
Add a “Refresh Data” or “Reload” button.

Screenshot
(screenshots/ui_missing_refresh_button.png)

---

Observation 3: No Option to Select Multiple Records for Deletion
**Severity:** Medium  
**Type:** Missing Feature / UX Improvement  

Description:
The UI only allows deleting one employee at a time.  
Bulk delete functionality is missing.

Recommendation:
Add checkboxes and a “Delete Selected” option.

Screenshot
(screenshots/ui_missing_multi_delete.png)

---
Observation 4: Missing Sequence Number and DateTimeofCreation Column in Employee List
**Severity:** Low  
**Type**: UX Improvement / Missing Feature

Description:
TThe employee list table does not include a sequence number (index) column. While this does not affect functionality, adding a sequence number would improve readability, make it easier to reference rows during reviews, and enhance overall user experience—especially when dealing with large datasets.

Recommendation:
Introduce a sequence number (S.No) column to help users quickly identify and reference individual records.

Screenshot
(screenshots/ui_missing_index_No.png)

Observation 5: Missing Pagination in Employee List Table
**Severity**: Low
**Type**: UX Improvement / Missing Feature

Description:  
The employee list table does not provide pagination. When the number of employee records grows, the table becomes long and difficult to navigate. Lack of pagination reduces readability, increases scrolling effort, and makes it harder for users to locate or compare records efficiently—especially in environments with large datasets.

Recommendation:  
Introduce pagination controls (e.g., 10/25/50 rows per page) to improve navigation, readability, and overall user experience when managing large employee lists.

Screenshot:  
(screenshots/ui_missing_pagination.png)

Observation 6: Missing Loader/Spinner During Data Fetch
**Severity**: Medium
**Type**: UX Issue / Missing Feedback Indicator

Description:  
When the application is fetching data, no loader or spinner is displayed to indicate that the system is processing the request. This results in a confusing user experience, as users may assume the page is unresponsive, stuck, or that no action has been triggered. Providing a visual loading indicator is essential for clarity, especially when data retrieval takes noticeable time.

Recommendation:  
Introduce a loader or spinner component that appears during data fetch operations. This will provide immediate feedback, reassure users that the system is working, and improve overall usability.

Screenshot  
(screenshots/ui_missing_loader.png)
End of UI Bug Report

