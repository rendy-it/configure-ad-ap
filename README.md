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
<img width="563" height="432" alt="Step 4" src="https://github.com/user-attachments/assets/20b4b043-42a2-42ce-902d-bc895e6ecef7" />


<p>
  
- Go to the DC-1 VM page. Make note and copy the private IP address.
   
</p>
<br />

<p>
<img width="893" height="506" alt="Step 4a" src="https://github.com/user-attachments/assets/bcab426f-ca02-4c3d-9634-730d911d9b07" />


<p>
  
- Go to the Windows-VM page and select “Network Settings”
- Then click on the “Network Interface / IP configuration”.

 
   
</p>
<br />

<p>
<img width="758" height="449" alt="Step 4a1" src="https://github.com/user-attachments/assets/8e5779c5-9261-47cb-b3b9-b3400325d389" />


<p>
  
- Then select the “DNS servers” tab on the left.
- Then, select “Custom” and paste the DC-1 VM private IP address number. Then select “Save”.

   
</p>
<br />

<p>
<img width="1458" height="403" alt="Step 4a3" src="https://github.com/user-attachments/assets/2efa9048-196d-4a07-8961-90010f6309db" />


<p>
  
- The DNS configuration is now complete. To make sure all changes have been applied. Go back to your Azure VMs page and restart all the VMs.
   
</p>
<br />

<h2> << Step 5: Confirm network connectivity between the DC-1 VM and the Client VM >> </h2>


<p>
<img width="643" height="460" alt="Step 5" src="https://github.com/user-attachments/assets/8fffb1e6-9414-4899-a1aa-0e6799651853" />


<p>
  
- We are going to log in to the Client Windows-VM and attempt to ping DC-1s private IP address.
- Make note of and copy your DC-1 VM’s private IP address.

   
</p>
<br />

<p>
<img width="887" height="726" alt="Step 5a" src="https://github.com/user-attachments/assets/cbb7c728-f04a-4257-8833-5a8a6bc2b760" />


<p>
  
- Log in to the Windows VM using RDP.
- Then you want to open “PowerShell”

   
</p>
<br />

<p>
<img width="761" height="534" alt="Step 5a1" src="https://github.com/user-attachments/assets/074dd991-92d4-4638-99a2-7f900d3eeba8" />


<p>
  
- Then type “ping” and paste the DC-1 private IP number.
- Example: “ping 10.1.0.5” then press enter and it should ping successfully.


   
</p>
<br />

<p>
<img width="699" height="867" alt="Step 5a2" src="https://github.com/user-attachments/assets/50dee3fc-0f73-4839-923d-e7c8fd1ff1d4" />


<p>
  
- Next, type “ipconfig /all” and it should give us results in which will show DC-1’s private IP address.
- The network connectivity attempt between both VMs is now complete.


   
</p>
<br />



<h2> << Conclusion >> </h2>

<p>
  
- Close the Remote Desktop connection.
- Go Back to your Azure resource group page.
- Also, make sure your VMs are on “Stop” status if you are not going to use them right away. This way you will not be charged while they are not in use.
- To conclude, we have successfully implemented the on-premises of Active Directory within Virtual machines on the Azure Cloud infrastructure. 


</p>
<br />

