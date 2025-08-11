# August-5-Client-Scripts-Data-Policies

**Activity**<br>
You've been working at DXC Technology on the ITSM team for a few months. One of your responsibilities is reviewing incoming Asset Recovery Requests. Your manager, recently noticed that some unauthorized users have been trying to submit these requests without proper approval. <br>

She sends you this note:<br>
> “We need to make sure only users with the role dxc_employee can submit Asset Recovery Requests. If they don’t have that role, they should see a clear message and they shouldn’t be able to submit the form at all.”

**Task**:<br>
Use a Client Script to enforce this rule.<br>
The script should:<br>
- Trigger when someone tries to submit the form
- Check if the user has the dxc_employee role
- If not, show a message (pick your location) and prevent submission
---

Right now the Device Manager group has access to the Application Menu and Module. To confirm: <br>
Go to All > DXC Technologies (hover over the edit icon to the right and click it) <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Hover%20Over%20the%20Edit%20Icon%20and%20Click%20It.png?raw=true) <br>
We will remove the role for this specific Application Menu and Module on the bottom. <br>
Now everyone can have access to that specific area. <br>

---
We will create a new role: <br>
Go to All > Users and Groups > Roles <br>
and click **New** <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/New%20Role%20DXC%20Employee.png?raw=true) <br>

Now we will create the Client Script: <br>
Go to System Definition > Client Scripts <br>
Click New <br>
prompt: Make sure that only users with the Dxc employee role can submit asset recovery request.
- Table Asset Recovery Request is the table associated with our Module in our Application
- for Type, onSubmit is the one thatgets triggered when its saved, updated or submitted
- Correction: Name - Block Submission for *non* DXE Employees

  ![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Client%20Script%20New%20Record%20Name%20Table%20UI%20Type%20Type%202.png?raw=true) <br>

**Key variables**: <br>
`g_user` - represents the current user object <br>
`g_form` - represents the form being submitted <br>
In ServiceNow client scripts and UI policies, the current user object is accessed via the g_user object, which is an instance of the GlideUser API. This object provides information about the currently logged-in user, such as their name, roles, and user ID.

**Flow**: <br>

1. When the form is submitted (onSubmit() function is called)
2. It checks if the current user has the "dxe_employee" role
3. If the user does NOT have this role:
    - Blocks the submission
    - Shows an error message: "only dxe employee can submit"
    - Returns false to prevent the form from being processed 
4. If the user does have the role, the function continues (presumably allowing the submission) <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/onSubmit%20Function%20-%20Role%20Permission%20Check%20Script.png?raw=true)

We're going to modify it so that the user we decide to test it with is going to have permissions to access that table. 
 Go to All > Users and Groups > Users <br>
 Open a Users record, in this case Beth Anglin <br>
In Beth Anglin's record, under the Roles tab, we'll click Edit <br>
We're gonna add the role instead of the group, just because of the way that we're gonna go ahead and do the testing
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/Roles%20tab%20inside%20Users%20record.png?raw=true) <br>
We need Beth to access the Asset Recovery table
The role a user should we have in order to access that table is `u_asset_recovery_request_user` <br>
![](https://github.com/CodeWithLuwam/August-5-Client-Scripts-Data-Policies/blob/main/Images/User%20Role%20Edit%20Members.png?raw=true) <br>
  

