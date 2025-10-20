<p align="center">
<img width="3000" height="1501" alt="what-is-group-policy-in-active-directory" src="https://github.com/user-attachments/assets/59bfeff1-284c-4271-bfa0-b44ca0d56528" />




</p>

<h1>Configuring Account Policies for Active Directory</h1>
This tutorial outlines how to configure Account Policies in Active Directory using Group Policy to improve security and user authentication. It will cover essential settings like User Password and User Account Lockout Policies to help you implement best practices across your network.<br />


<h2>Environments and Technologies Used</h2>

- Windows Virtual Machine on Microsoft Azure
- Remote Desktop Connection
- Microsoft Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022 for a Domain Controller
- Windows 10 Pro </b> (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

-	Step 1: Configure Group Policy to Lockout the account after 5 attempts.
-	Step 2: Log into the DC-1 VM and try to lockout a user account previously created.
-	Step 3: Re-Enable the account after it has been disabled.
-	Step 4: Enabling and Disabling Accounts.
-	Step 5: Observe the logs of the DC-1 and Client-1 VMs to analyze security activity.


<h2>Deployment and Configuration Steps</h2>  

<h2> << Step 1: Configure Group Policy to Lockout the account after 5 attempts. >> </h2>
<p>
<img width="2198" height="764" alt="Step 1_1a" src="https://github.com/user-attachments/assets/d54d4307-f801-4f8d-a02e-a1555613c0cc" />



</p>
<p>
  
- Login to the DC-1 VM using RDP under “mydomain.com\jane_admin”.
- Then in the search bar, type in “gpmc.msc” and open it. This will open the windows Group Policy Management Console.
- Then select the “Default Domain Policy” under the “mydomain.com” tab that is under “Domains” and right click the click on “Edit”.



</p>
<br />
<p>
<img width="1715" height="813" alt="Step 1a1" src="https://github.com/user-attachments/assets/852950ba-b51a-4c66-ac77-b3004f446a63" />


<p>
  
- Next, In the Group Policy Management Editor, expand the following: Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.
- Then select “Account lockout threshold” and input the number of invalid logo attempts. In this case we will use 5 attempts. Click Apply then Ok.
- You can now exit out of everything.


  
</p>
<br />


<h2> << Step 2: Log into the DC-1 VM and try to lockout a user account previously created >> </h2>

<p>
<img width="2105" height="856" alt="Step 2x_2" src="https://github.com/user-attachments/assets/9881f179-ce7e-453d-a6fb-c781dc167448" />


</p>
<p>
  
- Back in the DC-1 VM, open “Active Directory Users and Computers”.
- Go to the “_EMPLOYEES” folder and select a random user. In my case I will select “duf.sat”. With this user we are going to attempt to log into the Client-1 VM 6 times with a bad password to get that user account locked out.



</p>
<br />

<p>
<img width="1533" height="672" alt="Step 2a_2a1" src="https://github.com/user-attachments/assets/79be5272-936c-4aba-b380-252331ac6117" />


</p>
<p>
  
- Now using RDP try to log into the Client-1 VM with the random user “duf.sat” with the username “mydomain.com\duf.sat”.
- Next make sure to only input false passwords. Do this 6 times and the user account should now be locked out.
- You will get a warning message letting you know that the account has been locked out.
- Now if you try to log into that account with the correct password, you will get an error message.


  

</p>
<br />


<h2> << Step 3: Re-Enable the account after it has been disabled and reset the password >> </h2>


<p>
<img width="1896" height="599" alt="Step 3_3a" src="https://github.com/user-attachments/assets/7cb31cdd-e2f5-4414-bd1f-d79665e2a984" />



<p>
  
- To unlock the account and re-enable it, go back to the DC-1 VM.
- Open “Active Directory Users and Computers”. Right click on the “mydomain.com” tab and click on “Find…”.
- Type in the name of the user account in my case is “duf.sat” and click on “Find Now”.


   
</p>
<br />
<p>
<img width="1896" height="717" alt="Step 3a1_3a2" src="https://github.com/user-attachments/assets/80126181-2e02-44d7-b1e8-851cccebfb3d" />



<p>

  
- Then right click on the user “duf.sat”, and select “Reset Password”.
- Input the new password of your choosing, check the “Unlock the user’s account” box and then click on OK.
- Now when you try to log into this account with the new password, it should be unlocked and you will be able to log in.

  
   
</p>
<br />
<p>
<img width="1396" height="717" alt="Step 3a3" src="https://github.com/user-attachments/assets/7f3cfee8-53cc-45dd-b0d8-550ab7d3f115" />


<p>
  
- Just to test it out and be sure. Open up Windows Powershell and type the “whoami” command and it will result in the name of the user account.
   
</p>
<br />


<h2> << Step 4: Enabling and Disabling Accounts >> </h2>


<p>
<img width="1896" height="717" alt="Step 4_4a" src="https://github.com/user-attachments/assets/66cd1fb5-e91c-40f4-b9b9-5fddb6b1304d" />


<p>
  
- Back in the DC-1 VM. Open “Active Directory Users and Computers”.
- We are going to use the same user in Step 3 to do this step. Search for “duf.sat” once again.
- Once you find the user, right click and click on “Disable Account”.

   
</p>
<br />

<p>
<img width="1896" height="717" alt="Step 4a1_4a2" src="https://github.com/user-attachments/assets/1dab3c81-f129-4c9f-ad98-5c866b69d68a" />



<p>
  
- Once disabled, if you look at the icon, you will now see a down arrow signifying that the account has been disabled.
- Then, when you try and log into that account in the Client VM, you will get a message signaling that the account has been disabled.


 
   
</p>
<br />

<p>
<img width="1896" height="717" alt="Step 4b_4b1" src="https://github.com/user-attachments/assets/fd2293f2-d387-4d77-8866-b0c9ccd161d5" />



<p>
  
- To re-enable the user account, go back to “Active Directory Users and Computers” back in the DC-1 VM.
- Once you search and find the user, right click and click on “Enable Account”. Normally this is not as simple in the real world since there is usually a reason why the user account is disabled. But we will keep it simple here for the sake of the project.
- Now if you search for the user again, you will notice that the down arrow is gone on the icon.
- Then, when you try to log back into the user account in the Client VM, you should be able to log in now which means the account has been re-enabled.


   
</p>
<br />

<h2> << Step 5: Observe the logs of the DC-1 and Client-1 VMs to analyze security activity >> </h2>


<p>
<img width="2040" height="1143" alt="Step 5_5a" src="https://github.com/user-attachments/assets/225b7f14-7252-4de3-a5bf-5cf680e44b64" />



<p>
  
- First, we are going to the DC-1 VM and open “Event Viewer” in the search bar.
- Then expand Windows Logs > Security.
- There you will be able to see all the event logs which occurred in the DC-1 VM.


   
</p>
<br />

<p>
<img width="1691" height="1142" alt="Step 5b" src="https://github.com/user-attachments/assets/b6c7d7f4-b6e1-400a-a842-5aca8e3ea4e4" />



<p>
  
- Next, we are going to the Client-1 VM and open “Event Viewer” and follow the same steps as above.
- This time you will see that you are not able to see the security logs. That is because the user, which in my case is “duf.sat”, does not have admin access.


   
</p>
<br />

<p>
<img width="1472" height="751" alt="Step 5b1_5b2" src="https://github.com/user-attachments/assets/17475a0c-f3d7-42c6-80e0-a851ce53af3b" />



<p>
  
- To bypass this, run “Even Viewer” as an administrator. And input the “jane_admin” credentials.


   
</p>
<br />

<p>
<img width="1746" height="1241" alt="Step 5b3" src="https://github.com/user-attachments/assets/b5474b16-a8f6-47ff-aff6-1b1bd4513c8f" />



<p>
  
- Then, now you will be able to see the Security logs of this user account. Logs such as account lockouts and failed login attempts and other types of authentication events. 


   
</p>
<br />



<h2> << Conclusion >> </h2>

<p>
  
- Close the Remote Desktop connection.
- Go Back to your Azure resource group page.
- Also, make sure your VMs are on “Stop” status if you are not going to use them right away. This way you will not be charged while they are not in use.
- To conclude, we have successfully configured Account Policies in Active Directory using Group Policy to improve security and user authentication in Virtual machines on the Azure Cloud infrastructure. 


</p>
<br />

