<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This project outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create our Virtual Machines
- Observe ICMP Traffic
- Configuring a Firewall [Network Security Group

<h2>Deployment and Configuration Steps</h2>

<p>

</p>
<p>
Setting Up the Domain Controller in Azure:
1. Create a Resource Group:

  ![Image 1-29-25 at 2 57 PM](https://github.com/user-attachments/assets/9b0a4927-2af6-454e-945e-cfa61a527f2d)

First, I created a Resource Group in Azure to keep all the related resources organized. I gave it a clear name to reflect its purpose (e.g., "DomainControllerLab") and selected a region close to where I’ll be working.

2. Create a Virtual Network and Subnet:

![Image 1-29-25 at 3 02 PM](https://github.com/user-attachments/assets/86934690-2ac7-4d57-aa33-19323fff6868)

Next, I set up a Virtual Network (VNet) along with a Subnet. This network will provide the foundation for all communication between resources in this setup. I configured the VNet with an address range that would allow for future scalability and created a Subnet within it to segment network traffic.

3. Create the Domain Controller VM:

![Image 1-29-25 at 3 41 PM](https://github.com/user-attachments/assets/5b3a915b-25b2-4789-8c94-9ddd69c65d3a)


After setting up the network, I created a Windows Server 2022 Virtual Machine (VM) and named it “DC-1”.

During the setup:

![Image 1-29-25 at 3 51 PM](https://github.com/user-attachments/assets/7fdce00f-c202-462c-ab24-e808a9d2398f)

I selected the previously created Resource Group and assigned it to the Virtual Network and Subnet I just created.
I used the following credentials for the VM:
Username: labuser
Password: Cyberlab123!
I configured the VM size based on my needs, selected an appropriate storage type, and completed the setup.

4. Set the Domain Controller's NIC Private IP Address to Static:

![Image 1-29-25 at 3 56 PM](https://github.com/user-attachments/assets/bbec5643-6eca-4c2f-8417-dfa8d052767d)

Once the VM was up and running, I navigated to the Networking section of the DC-1 VM in the Azure portal. I found the NIC (Network Interface Card) associated with the VM and updated the Private IP address setting from Dynamic to Static. This ensures the Domain Controller will always have the same IP address, which is crucial for its role in the network.

5. Log Into the VM and Disable the Windows Firewall:

![Image 1-29-25 at 3 59 PM](https://github.com/user-attachments/assets/3ad877e8-5e58-4b81-8635-7092754e59ce)

Finally, I logged into the DC-1 VM using Remote Desktop (RDP) with the credentials I set up earlier. Once inside the Windows Server:

![Image 1-29-25 at 4 02 PM](https://github.com/user-attachments/assets/581e78d4-c999-448d-8c37-b83a48c1bc52)

I opened the Windows Firewall settings via the Control Panel or the search bar.
I disabled the Windows Firewall for all profiles (Domain, Private, and Public) to make testing connectivity easier.
Note: Disabling the firewall is only recommended for testing or lab scenarios. In production, I would configure specific firewall rules instead.


</p>
<br />

<p>

</p>
<p>
1. Create the Client VM:

First, I created a Windows 10 Virtual Machine (VM) and named it “Client-1”. During the setup:

I selected the same region and attached it to the same Virtual Network as DC-1 to ensure both VMs are part of the same network and can communicate.
I used the following credentials for the VM:
Username: labuser
Password: Cyberlab123!
I finalized the VM configuration and waited for it to be created successfully.

2. Set the Client VM’s DNS Settings to Point to DC-1:

![Image 1-29-25 at 4 08 PM](https://github.com/user-attachments/assets/8841767a-5ac1-4545-a371-6c0d346f33a8)

Once Client-1 was created, I navigated to the Networking section for the Client-1 VM in the Azure portal. I found the NIC (Network Interface Card) associated with Client-1 and updated the DNS settings:

I set the DNS server address to the Private IP address of DC-1. This ensures that the client will use the Domain Controller for DNS queries.
3. Restart Client-1:

![Image 1-29-25 at 4 12 PM](https://github.com/user-attachments/assets/5bd6d542-6bca-4c08-bbea-b62d622aea81)

After updating the DNS settings, I restarted the Client-1 VM from the Azure Portal to ensure the changes took effect.

4. Log Into Client-1 and Test Connectivity:

After the restart, I logged into Client-1 using Remote Desktop (RDP) with the credentials I set earlier. Once inside the VM, I performed the following steps:

![Image 1-29-25 at 4 18 PM](https://github.com/user-attachments/assets/73efe90d-13e0-4622-8ddc-90eb158f20f6)

Ping DC-1’s Private IP Address:
I opened a command prompt and ran the ping command to check connectivity:
ping <DC-1 Private IP Address>
The ping was successful, indicating that Client-1 could communicate with DC-1 over the network.
Check DNS Settings in PowerShell:
I opened PowerShell and ran the following command to verify the DNS settings:
ipconfig /all

![Image 1-29-25 at 4 20 PM](https://github.com/user-attachments/assets/cb67f722-e1ab-4cfa-9b81-c975e4065405)

In the output, I confirmed that the DNS Server field showed DC-1’s Private IP Address. This confirmed that the DNS settings were correctly configured.
</p>
<br />

<p>

</p>
<p>
1. Logging into DC-1 and Installing Active Directory Domain Services:

After logging into DC-1 as labuser, I opened the Server Manager. From there:

![Image 2-2-25 at 4 49 PM](https://github.com/user-attachments/assets/abb3df18-cd1a-4c4a-9d72-afc7606321a5)

I navigated to Add roles and features and selected Active Directory Domain Services (AD DS) to install it.
Once the installation completed, I began the promotion process to make this server a Domain Controller (DC).

![Image 2-2-25 at 4 47 PM](https://github.com/user-attachments/assets/db5ce9fe-c0f6-4229-8aad-7cc1d1340691)

2. Promoting DC-1 as a Domain Controller:

During the promotion process:

![Image 2-2-25 at 4 54 PM](https://github.com/user-attachments/assets/75c44452-d446-49a2-864c-7aaef4b27bce)

I selected the option to add a new forest and named the forest mydomain.com. I made a note of this domain name for future logins.
I configured the forest functional level and domain functional level, selecting the default options compatible with Windows Server 2022.
I set up the Directory Services Restore Mode (DSRM) password, which is used for recovering the Active Directory database if needed.
Once the configuration was complete, the server automatically restarted.

3. Logging Back into DC-1 as a Domain User:

![Image 2-2-25 at 4 59 PM](https://github.com/user-attachments/assets/659ba50f-76f5-4fb0-84b8-d1dff238e7f0)

After the restart, I logged back into DC-1 using the domain credentials:
mydomain.com\labuser.

This confirmed that the domain setup was successful, and I was now operating within the mydomain.com domain.

Creating a Domain Admin User:

4. Using Active Directory Users and Computers (ADUC):

In ADUC, I began organizing the domain structure:

![Image 2-2-25 at 5 08 PM](https://github.com/user-attachments/assets/ccf8d2a5-9a7a-4476-9e5c-8af56a5607b6)

I created an Organizational Unit (OU) named _EMPLOYEES to store regular employee accounts.
I created another OU named _ADMINS for administrative accounts.

![Image 2-2-25 at 5 10 PM](https://github.com/user-attachments/assets/8839a758-6a91-4c89-b856-506f1315f78b)

5. Adding Jane Doe as a Domain Admin:

Within the _ADMINS OU:

![Image 2-2-25 at 5 13 PM](https://github.com/user-attachments/assets/3728fce4-2065-46fc-b7d3-447eecd61e8f)

I created a new user named Jane Doe with the username:
jane_admin
Password: Cyberlab123!
After creating the account, I added jane_admin to the Domain Admins Security Group. This granted Jane Doe administrative privileges over the domain.

6. Logging Out and Switching to Jane Admin:

![Image 2-2-25 at 5 18 PM](https://github.com/user-attachments/assets/5652f8cf-6ebb-49c0-8d12-a2b30fc8de89)

Once Jane Doe’s account was set up as a Domain Admin, I logged out of mydomain.com\labuser and closed my connection to DC-1. Then, I logged back into DC-1 using:
mydomain.com\jane_admin.
</p>
<br />
<p> 
  
  
  Joining Client-1 to the Domain:
1. Logging into Client-1 as the Local Admin:

I logged into Client-1 using the original local admin credentials:
Username: labuser
Password: Cyberlab123!

2. Joining Client-1 to the Domain:

Once logged in:

![Image 2-2-25 at 5 21 PM](https://github.com/user-attachments/assets/50eb2f36-c5d7-46e7-b3d0-5a1c068fef4b)

I opened the System Properties window by right-clicking This PC and selecting Properties.
I clicked Change settings under the Computer name, domain, and workgroup settings section.
In the System Properties dialog, I clicked the Change button and selected Domain under the "Member of" section.
I entered the domain name mydomain.com and clicked OK.
I was prompted to provide credentials for a user with permission to join devices to the domain. I used the domain admin account:
Username: mydomain.com\jane_admin
Password: Cyberlab123!

![Image 2-2-25 at 5 23 PM](https://github.com/user-attachments/assets/371d3757-0025-4387-9f35-dadc89ce123f)

After the process completed, I saw a welcome message confirming that Client-1 was successfully added to the domain.
The computer then prompted me to restart. I allowed the restart to complete.

![Image 2-2-25 at 5 25 PM](https://github.com/user-attachments/assets/a7185225-d1fa-49d2-950e-32b8a363d7b8)

Verifying Client-1 in Active Directory:

3. Logging into the Domain Controller:

Next, I logged back into DC-1 as mydomain.com\jane_admin.

4. Verifying Client-1 in ADUC:

Using Active Directory Users and Computers (ADUC):

![Image 2-2-25 at 5 28 PM](https://github.com/user-attachments/assets/a4b50236-d007-4cfb-a767-b89cb9d36a5f)

I navigated to the default Computers container.
I verified that Client-1 appeared in the list of domain-joined computers. This confirmed that the machine had successfully joined the domain.
Organizing Client-1 in ADUC:

5. Creating the _CLIENTS OU:

To better organize domain resources, I created a new Organizational Unit (OU):

![Image 2-2-25 at 5 30 PM](https://github.com/user-attachments/assets/43054faa-fed9-4019-8a53-a2e24ead07e7)

In ADUC, I right-clicked the domain name (mydomain.com) and selected New > Organizational Unit.
I named the OU _CLIENTS to store client machine accounts.

6. Moving Client-1 into the _CLIENTS OU:

After creating the OU:

I navigated to the default Computers container in ADUC.
I located Client-1, right-clicked it, and selected Move.
In the dialog that appeared, I selected the _CLIENTS OU and clicked OK.
This action moved Client-1 into the _CLIENTS OU, helping me maintain a clean and organized structure for the domain.
</p>
<br/>
<img src="https://i.imgur.com/Bz03xt8.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
  Setting Up Remote Desktop for Non-Administrative Users on Client-1:
1. Logging into Client-1 as the Domain Admin:

![Image 2-2-25 at 5 32 PM](https://github.com/user-attachments/assets/dc8625be-0f3d-4e99-a72c-813b7bb3b390)

I logged into Client-1 using the domain admin account:
Username: mydomain.com\jane_admin
Password: Cyberlab123!

2. Enabling Remote Desktop for Domain Users:

Once logged in:

![Image 2-2-25 at 5 36 PM](https://github.com/user-attachments/assets/9aeff93d-d10a-4392-bfb4-d0d9eb383568)

I opened System Properties by right-clicking This PC and selecting Properties.
I clicked Remote Desktop on the left-hand menu.
In the Remote Desktop settings, I allowed remote connections to this computer and clicked on Select Users.
I added Domain Users to the list of users allowed to connect remotely. This ensures all standard users in the domain can log in via Remote Desktop.
I applied the changes and closed the settings.
This setup will enable remote desktop access for non-administrative users, but normally this should be done using Group Policy for managing multiple systems at once.

Creating Additional Users in Bulk:

3. Logging into DC-1 and Preparing the Script:

I logged into DC-1 as mydomain.com\jane_admin and opened PowerShell ISE with administrative privileges.

![Image 2-2-25 at 5 40 PM](https://github.com/user-attachments/assets/d9f64c68-d1e2-4341-a9a5-11ef8a37233d)

I created a new file in PowerShell ISE and pasted the contents of a script to create multiple user accounts. The script used commands like New-ADUser to add users to the domain and place them in the _EMPLOYEES Organizational Unit (OU).

![Image 2-2-25 at 5 43 PM (1)](https://github.com/user-attachments/assets/4a6a19a8-29a7-4108-8a2f-a40e8d5ad35a)

4. Running the Script:

After reviewing the script to ensure it was correct, I ran it. As the script executed, I saw messages indicating the successful creation of each user account.

![Image 2-2-25 at 5 47 PM](https://github.com/user-attachments/assets/b2d7e82e-aa71-4908-9675-bd05460cc744)

5. Verifying the Accounts in ADUC:

Once the script finished:

I opened Active Directory Users and Computers (ADUC).
I navigated to the _EMPLOYEES OU and verified that all the new accounts were created and displayed there.

![Image 2-2-25 at 5 49 PM](https://github.com/user-attachments/assets/250f0be6-238c-4d02-b43a-2db7f02120e5)

Testing Access with a New User:

6. Logging into Client-1 with a New User:

![Image 2-2-25 at 5 51 PM](https://github.com/user-attachments/assets/e5303d0c-753f-494a-a0b1-9db59c9fd3b0)

I noted the default password set in the script and attempted to log into Client-1 with one of the new user accounts, for example:
Username: mydomain.com\newuser1
Password: (password defined in the script).

The login was successful, and I was able to access Client-1 as a standard domain user. This confirmed that Remote Desktop access and account creation worked as expected.


</p>
