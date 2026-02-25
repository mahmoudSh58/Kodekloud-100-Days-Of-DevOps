## Day 70 : Configure Jenkins User Access

1- Click on  "Jenkins" on the left menu.

2- Login to Jenkins using username and password in description of this task.

3- Click on "Manage Jenkins" and then click on "Users" in "Security" section.

4- Click on "Create User" and fill the form with the following information:

- Username: `john`
- Password: `B4zNgHA7Ya`
- Full name: `John`

    Click on "Create User" button to create the user.


5- Install "Matrix Authorization Strategy" plugin by following these steps:

- Click on "Manage Jenkins" and then click on "Plugins".
- Click on "Available" tab and search for "Matrix Authorization Strategy" plugin.
- Select the plugin and click on "Install without restart" button to install the plugin.


6- Configure "john" user permission by following these steps:

- Click on "Manage Jenkins" and then click on "Security".
- in "Authorization" section, select "Project-based Matrix Authorization Strategy".
- In "User/group to add" field, enter "john" and click on "Add" button.
- Grant "john" user the following permissions:
  
  - Overall: Read
  - Job: Read
- Click on "Save" button to save the changes.