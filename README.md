<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Creating Users, Group Policy, and Managing Accounts in Azure </h1>
This tutorial will demonstrate how to configure Remote Desktop for non-administrative users, automate user creation using PowerShell, and manage Group Policy settings. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 
- Windows 10 

<h2>Project Walk-Through: </h2>
<br/>
<br/>

<p align="center">
To start, we'll log into Client-1 via Remote Desktop using the Domain Admin account (Jane_admin).
<br/>
  
![image](https://github.com/user-attachments/assets/19c110f9-fcb8-4a53-a215-d63da58de20a)

</p>

<p align="center">
Next, we're going to allow Remote Desktop for non-administrative users. Right-click the START  button> System> You'll see "Remote Desktop" on the right. Click it> "Select users that can access this PC". Then we're going to enter "Domain Users", select check name. Now all users can access the client via Remote Desktop: 
<br/>
  
![image](https://github.com/user-attachments/assets/b2d3c402-5890-4dbc-a6b0-0ce13640f534)

</p>

<p align="center">
Now we'll create multiple users in PowerShell. First, we need to log into the Domain Controller using the admin account (jane_admin).
<br/>
  

![Screenshot 2024-10-04 141402](https://github.com/user-attachments/assets/380e3658-04fa-43c3-ab1d-69e031ef82b8)

</p>

<p align="center">
After logging in, we'll open PowerShell ISE as an admin
<br />

![image](https://github.com/user-attachments/assets/36849256-6490-4303-8ab8-4d1b549f7b12)
![Screenshot 2024-10-04 141850](https://github.com/user-attachments/assets/d94e7ba6-24fc-4ab3-aa1d-a38b6fc825dd)

<p align="center">
By pressing "CTRL + S" we can save this file
<br/>

![Screenshot 2024-10-04 142109](https://github.com/user-attachments/assets/4e05f646-19e2-4980-b488-7978b63b876e)

<p/>
  
<p align="center">
Next, we'll paste our script into the top section of PowerShell ISE and click the "Run" button, which is marked with a green "Play" symbol.
<br/> 

![image](https://github.com/user-attachments/assets/dc4ad2d4-8894-4599-9abc-e241bc18389d)

<p/>

<p align="center">
Once the script is run, it will start creating random users and placing them into the OU (Organizational Unit) we previously created called "_EMPLOYEES." By navigating to Active Directory Users and Computers and selecting the "EMPLOYEES" folder, you’ll see all the users stored there. Pick one of the randomly generated users, then log into Client-1 using their username and the password "Password1
<br/>

![Screenshot 2024-10-04 143218](https://github.com/user-attachments/assets/56308577-7806-45dd-a572-539dcacc2574)

<p/>

<p align="center">
Log out of Jane's account on Client-1: 
<br/>

![Screenshot 2024-10-04 143358](https://github.com/user-attachments/assets/bfd96e80-df73-40d5-97b1-10dc5a83c7ca)

<p/>

<p align="center">
We can now log back into Client-1 using the newly created user from the script. Make sure to specify the login context by adding "mydomain.com\" before entering the username
<br/>

![Screenshot 2024-10-04 143712](https://github.com/user-attachments/assets/b300e9a8-e9b2-456c-92d8-8be2dbf16a56)

<p/>

<p align="center">
Once logged in, if we navigate to C: > Users, we'll see that our new user has a folder, as do the other users. However, we cannot access these folders because we lack the necessary permissions.
<br/>

![Screenshot 2024-10-04 144108](https://github.com/user-attachments/assets/71cf0dd3-16b6-4f9f-8dc7-df828365f352)

<br/>

<p align="center">
Let's log out of this user account. We will set up a group policy for account lockout and try to lock this user out.
<br/> 

![Screenshot 2024-10-04 144108](https://github.com/user-attachments/assets/5583c953-8c73-43ed-b7f4-49a92e4b2c25)

<br/>

<p align="center"> 
Return to the Domain Controller, right-click the "START" button, and select "Run." Type "gpmc.msc" to open the Group Policy Management console.
<br/>

![image](https://github.com/user-attachments/assets/432b4858-dbe4-4d70-818b-699eeab2427a)
![Screenshot 2024-10-04 145946](https://github.com/user-attachments/assets/0ef8f684-c332-4437-b9fe-29b4f7cdf051)

<br/>

<p align="center">
Expand Domain> mydomain.com> Select "Default Domain Policy"
<br/>

![Screenshot 2024-10-04 150413](https://github.com/user-attachments/assets/bfc8b25f-9539-49e4-ae6c-449c8aac7cae)

<br/>

<p align="center">
Select and expand "Policies" under the "Computer Configuration" tab> Windows Settings> Security Settings and Select "Account Lockout Policy"
<br/>

![Screenshot 2024-10-04 150620](https://github.com/user-attachments/assets/ee5581eb-8608-4122-98b2-4448f3b4abc5)

<br/>

<p align="center">
Select "Account Threshold" and set the account to lock after 5 invalid login attempts. You can also choose the duration of the lockout after the failed attempts. Once done, click "Apply."
<br/>

![Screenshot 2024-10-04 151109](https://github.com/user-attachments/assets/7ca43c7f-64e4-452f-a305-22a668d380a1)
![Screenshot 2024-10-04 151047](https://github.com/user-attachments/assets/fc87fc8f-f016-48bd-b1e2-4a573a2ca3bf)

<br/>

<p align="center">
To verify the changes, navigate to the Group Policy settings and review the lockout policy.
<br/>

![image](https://github.com/user-attachments/assets/cf33f8c5-67f8-42f8-975c-2345c3329181)

<br/>

<p align="center">
The Group Policy will update automatically, but it may take a few minutes. To speed up the process, we can force the update on Client-1 by signing in as an admin and running "gpupdate /force" in the command line.
<br/>

![Screenshot 2024-10-04 151643](https://github.com/user-attachments/assets/a059a3b3-5086-4e59-9cfa-95202c5f7fea)
![Screenshot 2024-10-04 152328](https://github.com/user-attachments/assets/58f43ed1-4be9-4dce-b9fa-7dce86d423ec)

<br/>

<p align="center">
Now for the moment of truth. Let’s log into Client-1 with one of the users we created through PowerShell and enter the wrong password more than 5 times. After the correct number of failed attempts, a lockout message should appear.
<br/>

![Screenshot 2024-10-04 152821](https://github.com/user-attachments/assets/167cba41-9070-4a13-ae78-5671ce82e2f3)

<br/>

<p align="center">
To unlock the user account, go to Active Directory Users and Computers > mydomain.com > _EMPLOYEES, double-click on the locked user, navigate to the "Account" tab, and check the "Unlock Account" box
<br/>

![image](https://github.com/user-attachments/assets/f38a2b96-6441-4a06-81c7-1e84b3842935)

<br/>

<p align="center">
Now we're going to unlock the account and change the password at the same time. Right-Click the user> Rest Password> and select " Unlock user Account" option. 
<br/>

![Screenshot 2024-10-04 153903](https://github.com/user-attachments/assets/5457df21-bcab-4ef8-b09a-1fe361abcf2f)

<br/>

<p align="center">
Now the user can log into Client-1 as normal
<br/>

![Screenshot 2024-10-04 145145](https://github.com/user-attachments/assets/ac1cb8a7-bd9d-4d3b-90ba-895d3befc8ed)

<br/>

<p align="center">
In order to disable users, navigate over to the DC and in "Active Directory Users and Computers"> mydomain.com> _EMPLOYEES> right-click the user you would like to disable and select "Disable"
<br/>

![image](https://github.com/user-attachments/assets/b33ac9be-3b6b-40e0-8e2f-7336215ff861)
![Screenshot 2024-10-04 154404](https://github.com/user-attachments/assets/33b36c14-7aed-4cb4-beec-75e08ad19829)


<br/>

<p align="center">
Now we're going to attempt to log in to the disabled account. There should be a error message once we enter the password. 
<br/>

![Screenshot 2024-10-04 154507](https://github.com/user-attachments/assets/35ac1513-7664-4365-94ae-7b8d1b7d6975)

<br/>

<p align="center">
Navigate back to the DC and right-click the user thats disabled and select "Enable" 
<br/>

![image](https://github.com/user-attachments/assets/7fe48357-b6c1-479a-a360-0e0878a07541)
![Screenshot 2024-10-04 154711](https://github.com/user-attachments/assets/f14477e5-bce1-4917-a30f-b0761a5ab932)

<br/>

<p align="center">
The disabled user is now re-enabled and has regained access to the account. Next, let’s attempt to view the user’s logs. To do this, search for "Event Viewer," then navigate to Windows Logs > Security. As you can see, we are unable to view the logs.
<br/>

![Screenshot 2024-10-04 155151](https://github.com/user-attachments/assets/2156dd78-8fc6-4e2f-b1d7-eb40831ea95f)

<br/>

<p align="center">
We received the error in Event Viewer because we’re not logged in as an admin. Since we already know Jane's credentials, we can close the window and reopen it, this time as Administrator, and then enter the credentials.
<br/>

![Screenshot 2024-10-04 155418](https://github.com/user-attachments/assets/1731e2d1-17a9-4d0f-bfb7-2e0a2652e7bc)

<br/>

<p align=center">
Now if we navigate back over to the Secuirty Log page we can view the logs: 
<br/>

![Screenshot 2024-10-04 155600](https://github.com/user-attachments/assets/7841dc9e-bda0-4758-91f8-5c78f6929dae)

<br/>

<p align="center">
In these logs, we can observe several key components that make up a log, such as the IP address, failed login attempts, the user who tried to log in, and the date and time of each attempt.
<br/>

![image](https://github.com/user-attachments/assets/c20d3ff0-afae-4fd1-9804-c021754c3f8c)














