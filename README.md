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
  
![Image](https://github.com/user-attachments/assets/26237986-20c4-4d9a-8ac1-9556e31dbced)

</p>

<p align="center">
Next, we're going to allow Remote Desktop for non-administrative users. Right-click the START  button> System> You'll see "Remote Desktop" on the right. Click it> "Select users that can access this PC". Then we're going to enter "Domain Users", select check name. Now all users can access the client via Remote Desktop: 
<br/>
  
![Image](https://github.com/user-attachments/assets/9734d4a7-599e-4d3a-8fe1-8777ae69d1a8)

</p>

<p align="center">
Now we'll create multiple users in PowerShell. First, we need to log into the Domain Controller using the admin account (jane_admin).
<br/>
  

![Image](https://github.com/user-attachments/assets/3f804ae7-b47d-4221-84d8-8eedec37b1f5)

</p>

<p align="center">
After logging in, we'll open PowerShell ISE as an admin
<br />

![Image](https://github.com/user-attachments/assets/87cc7863-7213-46f1-b225-cd7df7234b07)
![Image](https://github.com/user-attachments/assets/522f77ce-284d-4f76-b556-0a72e4cc3d64)

<p align="center">
By pressing "CTRL + S" we can save this file
<br/>

![Image](https://github.com/user-attachments/assets/db905a3e-ed35-4c29-9c44-1080e15d16d6)

<p/>
  
<p align="center">
Next, we'll paste our script into the top section of PowerShell ISE and click the "Run" button, which is marked with a green "Play" symbol.
<br/> 

![Image](https://github.com/user-attachments/assets/42089426-bbd3-4d53-8e7b-fd3fb9701ada)

<p/>

<p align="center">
Once the script is run, it will start creating random users and placing them into the OU (Organizational Unit) we previously created called "_EMPLOYEES." By navigating to Active Directory Users and Computers and selecting the "EMPLOYEES" folder, you’ll see all the users stored there. Pick one of the randomly generated users, then log into Client-1 using their username and the password "Password1
<br/>

![Image](https://github.com/user-attachments/assets/7c9213da-3f6a-41eb-8279-524ff66a152d)

<p/>

<p align="center">
Log out of Jane's account on Client-1: 
<br/>

![Image](https://github.com/user-attachments/assets/ba5359ef-ae18-400c-93ee-80d65dcaa212)

<p/>

<p align="center">
We can now log back into Client-1 using the newly created user from the script. Make sure to specify the login context by adding "mydomain.com\" before entering the username
<br/>

![Image](https://github.com/user-attachments/assets/a8728870-7860-4128-a018-bf2d6c4361d1)

<p/>

<p align="center">
Once logged in, if we navigate to C: > Users, we'll see that our new user has a folder, as do the other users. However, we cannot access these folders because we lack the necessary permissions.
<br/>

![Image](https://github.com/user-attachments/assets/200b6008-59fc-4094-adf3-8f9c5ae8bc15)

<br/>

<p align="center">
Let's log out of this user account. We will set up a group policy for account lockout and try to lock this user out.
<br/> 

![Image](https://github.com/user-attachments/assets/e88b7e3c-d4ae-4328-bc9c-da3288c0b505)

<br/>

<p align="center"> 
Return to the Domain Controller, right-click the "START" button, and select "Run." Type "gpmc.msc" to open the Group Policy Management console.
<br/>

![Image](https://github.com/user-attachments/assets/70618ec6-0df9-40e4-af9e-0c8fe69d11f6)
![Image](https://github.com/user-attachments/assets/512dde7e-21ad-4fe4-9284-ffc79d7cd095)

<br/>

<p align="center">
Expand Domain> mydomain.com> Select "Default Domain Policy"
<br/>

![Image](https://github.com/user-attachments/assets/37756cfc-0e4a-40d5-bce7-f7d169dccbe3)

<br/>

<p align="center">
Select and expand "Policies" under the "Computer Configuration" tab> Windows Settings> Security Settings and Select "Account Lockout Policy"
<br/>

![Image](https://github.com/user-attachments/assets/707ada0f-e070-4882-b785-91f287195e64)

<br/>

<p align="center">
Select "Account Threshold" and set the account to lock after 5 invalid login attempts. You can also choose the duration of the lockout after the failed attempts. Once done, click "Apply."
<br/>

![Image](https://github.com/user-attachments/assets/98b9c6ef-2a97-48b0-b841-fd1a86450195)
![Image](https://github.com/user-attachments/assets/ed2c97dc-898f-42b0-b149-14f73f3e7c39)
<br/>

<p align="center">
To verify the changes, navigate to the Group Policy settings and review the lockout policy.
<br/>

![Image](https://github.com/user-attachments/assets/77c38bf6-7bd3-4fe0-9363-31e25700cac6)

<br/>

<p align="center">
The Group Policy will update automatically, but it may take a few minutes. To speed up the process, we can force the update on Client-1 by signing in as an admin and running "gpupdate /force" in the command line.
<br/>

![Image](https://github.com/user-attachments/assets/54f5526e-ddc4-46e5-a4e2-ecf4a9c902a3)
![Image](https://github.com/user-attachments/assets/36844d6d-3613-4656-b03d-7fe9d0f7a1fe)
<br/>

<p align="center">
Now let’s log into Client-1 with one of the users we created through PowerShell and enter the wrong password more than 5 times. After the correct number of failed attempts, a lockout message should appear.
<br/>

![Image](https://github.com/user-attachments/assets/e0331921-27bc-4376-8577-9d79550f4e7e)

<br/>

<p align="center">
To unlock the user account, go to Active Directory Users and Computers > mydomain.com > _EMPLOYEES, double-click on the locked user, navigate to the "Account" tab, and check the "Unlock Account" box
<br/>

![Image](https://github.com/user-attachments/assets/9a1d0ec9-fa5b-4beb-9c3b-4338a36bceca)

<br/>

<p align="center">
Now we're going to unlock the account and change the password at the same time. Right-Click the user> Rest Password> and select " Unlock user Account" option. The user can log into Client-1 as normal. 
<br/>

![Image](https://github.com/user-attachments/assets/a4fbb59f-8292-4d37-a0e4-4a04606ca843)
<br/>

<p align="center">
In order to disable users, navigate over to the DC and in "Active Directory Users and Computers"> mydomain.com> _EMPLOYEES> right-click the user you would like to disable and select "Disable"
<br/>

![Image](https://github.com/user-attachments/assets/005e86dd-319d-49a8-ba16-2a1b56ef2985)
![Image](https://github.com/user-attachments/assets/4d70e7c8-7b07-4cd6-b9bb-90d0044605ac)

<br/>

<p align="center">
Now we're going to attempt to log in to the disabled account. There should be a error message once we enter the password. 
<br/>

![Image](https://github.com/user-attachments/assets/76a35cd5-658f-4d52-bd25-ee5422aa903f)

<br/>

<p align="center">
Navigate back to the DC and right-click the user thats disabled and select "Enable" 
<br/>

![Image](https://github.com/user-attachments/assets/2962b01b-01e7-40d3-8151-2235bfa9072b)
![Image](https://github.com/user-attachments/assets/f89c505c-dfae-435b-809e-66676220b5db)

<br/>

<p align="center">
The disabled user is now re-enabled and has regained access to the account. Next, let’s attempt to view the user’s logs. To do this, search for "Event Viewer," then navigate to Windows Logs > Security. As you can see, we are unable to view the logs.
<br/>

![Image](https://github.com/user-attachments/assets/ed8d0685-62d5-4539-8a98-e2c13b78e760)

<br/>

<p align="center">
We received the error in Event Viewer because we’re not logged in as an admin. Since we already know Jane's credentials, we can close the window and reopen it, this time as Administrator, and then enter the credentials.
<br/>

![Image](https://github.com/user-attachments/assets/bc05c1c1-0dc9-4d63-9e9e-23ecd95372a8)

<br/>

<p align=center">
Now if we navigate back over to the Secuirty Log page we can view the logs: 
<br/>

![Image](https://github.com/user-attachments/assets/3411a6ed-4ace-4a9d-9878-76e38a14d211)

<br/>

<p align="center">
In these logs, we can observe several key components that make up a log, such as the IP address, failed login attempts, the user who tried to log in, and the date and time of each attempt.
<br/>

![Image](https://github.com/user-attachments/assets/c75a58c4-1bef-4d89-abda-f95bb1bc4d7d)













