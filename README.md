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
<img width="372" height="364" alt="Step 1" src="https://github.com/user-attachments/assets/c52606d9-c394-409c-b7a7-fbb0edf07071" /> <img width="53" height="364" alt="Right Arrow for Step 1_1a" src="https://github.com/user-attachments/assets/02fec441-0c57-4cba-89ea-b284105715ba" /> <img width="371" height="364" alt="Step 1a" src="https://github.com/user-attachments/assets/e0427367-3448-4966-b035-795b79085809" />





</p>
<p>
  
- On the Azure home page, hover you mouse cursor on “Virtual Machines”.
- A small window will appear, then click on “Create”.
- Then select “Virtual Machine”. 


</p>
<br />
<p>
<img width="686" height="1007" alt="Step 1b" src="https://github.com/user-attachments/assets/5ef2f882-c6bb-4a32-b62d-4c3eaa6ae5d4" />


<p>
  
- On the next page, select your resource group.
  - Give the VM a name (DC-1).
  - Select a Zone.
  - Select “Windows Server 2022” for the OS image.
  - And select at least 2 vcpus, with 8 or 16 gig memory.


  
</p>
<br />
<p>
<img width="614" height="317" alt="Step 1c" src="https://github.com/user-attachments/assets/daa109ac-5421-4835-b8c9-53b71b6d74d1" />


</p>
<p>
  
- Next, create a Username and Password


</p>
<br />
<p>
<img width="777" height="283" alt="Step 1d" src="https://github.com/user-attachments/assets/624aba38-fc57-4df9-8340-488883b09528" />


</p>
<p>
  
- Next, go to the “Networking” tab.

</p>
<br />
<p>
<img width="777" height="283" alt="Step 1d" src="https://github.com/user-attachments/assets/624aba38-fc57-4df9-8340-488883b09528" />


</p>
<p>
  
- On the next page:
  - Make sure to select the same Virtual Network as the Windows VM that we used from previous projects.
  - Everything else is set by default.


</p>
<br />
<p>
<img width="739" height="331" alt="Step 1f" src="https://github.com/user-attachments/assets/a3b074f2-61f3-42da-8d3b-e8bcf233029b" />


</p>
<p>
  
- Go back to the “Basics” tab.
- Scroll to the bottom and select “Review + Create”.



</p>
<br />
<p>
<img width="431" height="1009" alt="Step 1g" src="https://github.com/user-attachments/assets/883fb488-e5fa-4087-89e0-0c06d81a3b81" />



</p>
<p>
  
- On the next Page:
  - Double Check to make sure everything is to your liking.
  - Then select “Create”.
  - Once completed you can click on “Go to resource”.
  - Then you will be able to see the VM that you just created with all the information.




</p>
<br />


<h2> << Step 2: Use the Windows-VM that we used from previous projects to be the Client >> </h2>

<p>
<img width="771" height="974" alt="Step 0" src="https://github.com/user-attachments/assets/1ed00524-e11e-4efc-9739-65cb5d858437" />


</p>
<p>
  
- For the client VM we are going to use the Windows VM that we created in a previous project referenced [here](https://github.com/rendy-it/azure-vm).
- If you have not created one before just follow the exact steps for the Windows 10 VM created from that project.
- Once you are done, head back to the VM page of your Azure Resource Group and your Windows 10 VM should be there alongside your DC-1 VM.


</p>
<br />

<h2> << Step 3: Set the Domain Controller’s VM’s private IP to static and disable the Windows Firewall >> </h2>


<p>
<img width="366" height="433" alt="Step 2" src="https://github.com/user-attachments/assets/9e0b69b2-6eb9-4e33-af3c-e386ea0d9fd5" /><img width="72" height="433" alt="Right Arrow for Step 2_2a" src="https://github.com/user-attachments/assets/9f868247-3e48-4bff-bba8-b8b7cf678aa9" />
<img width="366" height="433" alt="Step 2a" src="https://github.com/user-attachments/assets/424228bd-2fb3-432b-8d43-022bc99b0c5b" />



<p>
  
- After Creating the VMs, we need to set the DC VM’s NIC’s private ip address to be static.
- Go to the DC-1 VM’s page in Azure and select “Network Settings”.
- Then click on the “Network Interface / IP configuration”.

   
</p>
<br />
<p>
<img width="336" height="594" alt="Step 2a1" src="https://github.com/user-attachments/assets/1858cc82-6cd2-4913-8ce9-b93b0110f9be" /><img width="81" height="594" alt="Right Arrow for Step 2a1_2a1a" src="https://github.com/user-attachments/assets/1063d4fe-7064-4b8d-8f99-cd5470f4ff7b" /><img width="337" height="594" alt="Step 2a1a" src="https://github.com/user-attachments/assets/7c58bb4e-5d6c-4824-a5d6-1c0896529727" />



<p>

  
- Then, click on “ipconfig1” and change the Allocation from “Dynamic” to “Static” and click on “Save”.
- Once you do that, the private IP address of DC-1 should no longer change.
  
   
</p>
<br />
<p>
<img width="1186" height="440" alt="Step 2a2" src="https://github.com/user-attachments/assets/caef7ce6-352a-41ec-ae6d-081fea2c6b2e" />

<p>
  
- Next, log into the DC-1 VM with RDP using the VM’s public IP address.
   
</p>
<br />

<p>
<img width="886" height="761" alt="Step 2a3" src="https://github.com/user-attachments/assets/0a58343a-bd44-4afc-8ed4-3ab856015271" />


<p>
  
- Then once logged in, you want to run “wf.msc” for windows firewall.

   
</p>
<br />

<p>
<img width="836" height="626" alt="Step 2a4" src="https://github.com/user-attachments/assets/1f7d054a-00f5-4574-8cce-013780046f59" />


<p>
  
- Then, select “Windows Defender Firewall Properties”.

   
</p>
<br />

<p>
<img width="1400" height="454" alt="Step 2a5_2a6_2a7" src="https://github.com/user-attachments/assets/bd1f9db5-0e7f-4584-ae39-72c7850f7203" />



<p>
  
- Then for the “Domain” “Private” and “Public” profile tabs make sure that “Firewall State” is off. Then select “Apply”.
- Once that is done, the windows firewall for the DC-1 VM should be off.

   
</p>
<br />

<h2> << Step 4: Set the Windows- VM’s DNS settings to DC-1’s Private IP address >> </h2>


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

