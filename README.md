# August-5-Client-Scripts-Data-Policies

## Client Script: Asset Recovery Request Submission Control
### **Activity**<br>
You've been working at DXC Technology on the ITSM team for a few months. One of your responsibilities is reviewing incoming Asset Recovery Requests. Your manager, recently noticed that some unauthorized users have been trying to submit these requests without proper approval. <br>

She sends you this note:<br>
> “We need to make sure only users with the role dxc_employee can submit Asset Recovery Requests. If they don’t have that role, they should see a clear message and they shouldn’t be able to submit the form at all.”

###  **Task**:<br>
Use a Client Script to enforce this rule. The script should:<br>
- Trigger when someone tries to submit the form
- Check if the user has the dxc_employee role
- If not, show a message (pick your location) and prevent submission
---
###  **Current Access Review**
The **Device Manager** group has access to the Application Menu and Module. To review: <br>
1. Navigate to **All > DXC Technologies**
2. Hover over the edit icon to the right and click it <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Hover%20Over%20the%20Edit%20Icon%20and%20Click%20It.png?raw=true) <br>
We will remove the role from the Application Menu and Module, so everyone will have access to this area by default.

---
###  **Create a New Role** <br>
1. Go to **All > Users and Groups > Roles** <br>
2. click **New** <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/New%20Role%20DXC%20Employee.png?raw=true) <br>

---

### **Create the Client Script**: <br>
(Prompt: Make sure that only users with the Dxc employee role can submit asset recovery request.)
1. Navigate to **System Definition > Client Scripts** <br>
2. Click **New** <br>
3. Fill out the form as follows:
    - **Name**: Block Submission for non DXC Employees
    - **Table**: Asset Recovery Request (associated with our module)
    - **Type**: onSubmit (triggers when the form is saved, updated, or submitted)

  ![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Client%20Script%20New%20Record%20Name%20Table%20UI%20Type%20Type%202.png?raw=true) <br>

---

### **Key variables**: <br>
- `g_user` - Represents the current logged-in user (instance of GlideUser API). It provides user info such as roles and user ID. <br>
- `g_form` - Represents the form being submitted <br>

---

### Script Flow: <br>

1. When the form is submitted (`onSubmit()` function is called)
2. Check if the current user has the **"dxe_employee"** role
3. If the user does NOT have this role:
    - Block the submission
    - Show an error message: "only DXC employees can submit this form"
    - Return `false` to prevent the form from processing 
4. If the user does have the role, allow the submission to continue <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/onSubmit%20Function%20-%20Role%20Permission%20Check%20Script.png?raw=true)

---

### Assigning Roles for Testing <br>

To test the script, we'll assign necessary permissions to a test user: <br>
1. Go to **All > Users and Groups > Users** <br>
2. Open the users record for **Beth Anglin** <br>
3. Under the Roles tab, click **Edit** <br> ![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Roles%20tab%20inside%20Users%20record.png?raw=true) <br>
4. Add the role `u_asset_recovery_request_user` (allows access to the Asset Recovery Request table)  - instead of the group, for testing purposes <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/User%20Role%20Edit%20Members.png?raw=true) <br>

---
### Table Permissions Overview <br>

When a table is created, default CRUD (Create, Read, Update, Delete) permissions are created with a specific role.
By assigning Beth this role, she will have full access to the Asset Recovery Request table.

We got rid of the permissions at the application level at the module level. Now we just gave her access to view the table.

Let's go ahead and quickly view the table so we could see those permissions.
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Asset%20Recovery%20Request%20table.png?raw=true) <br>
We click on the u_asset_recovery_request under Acess Control tab and we see the role on the bottom. <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Role%20in%20Asset%20Recovery%20Request.png?raw=true) <br>

So instead of creating a role and applying it to the ACL, we're taking the base acl role that was automatically created and applying it to a particular user. for the testing purposes. Its more efficient and it'll give us all four CRUD permissions.

Reviewing permissions:
1. Navigate to the **Asset Recovery Request** table
2. Click the **Access Control** tab
3. Notice the role applied on the bottom

We are using this default ACL role for testing instead of creating a new one, which simplifies the process.

---

### Impersonate Beth to Test <br>
1. Navigate to **All > DXC Technology Devices > Device Inventory** <br>
2. Verify you can access the Asset Recovery Request table (due to the assigned role)<br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Impersonate%20User%20to%20View%20Asset%20Recovery%20Request%20table.png?raw=true) <br>
3. Create a new Asset Recovery Request and submit the form <br>
4. If the user lacks the dxc_employee role, an error message appears and submission is blocked <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/onSubmit%20Error%20Message.png?raw=true) <br>

---
### Here’s why she still couldn’t submit the form: <br>
- The Client Script you wrote is an additional layer on top of the ACL permissions.
- Even if Beth has the table access role, the Client Script blocks form submission if the user does not have the dxc_employee role.
- So, if Beth’s user record is missing the dxc_employee role, the Client Script will prevent her from submitting—even though she technically has access to the table.




