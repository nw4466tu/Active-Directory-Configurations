<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Configurations in Azure</h1>
This lab is a follow up to the lab where I installed Active Directory and created a domain controller. I will now be configuring Active Directory and allowing a client to join the domain as well as creating user accounts. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/EFjZrUG.png" height="80%" width="80%" alt="Configuration Steps"/>
<img src="https://i.imgur.com/HSJk3BO.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
Now that Active Directory is installed on the domain controller VM, it is time to create new Organizational Units and Users. With the Active Directory Users and Computers console open, right click on the domain you created (in my case, ernestotest.com) and make a new Organizational Unit (OU). I have created two Organizational Units, _EMPLOYEES and _ADMINS. The reason behind this naming scheme is because a Powershell script will be utilized later. Within the _ADMINS OU, I created a new User called Jane Doe. Jane's account will be given administrative privileges through the use of a Security Group. To grant admin privileges to a User, right click on the user and open their Properties. Click Member Of then Add to apply the appropraite security group. In this case, I added Jane to the Domain Admins security group. From now on, I will be using Jane's account to make any further changes. I will be logging off as labuser and log in as jane_admin.
</p>
<br />

<p>
<img src="https://i.imgur.com/X6UGnsf.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
Before the client can join the domain, it is important to configure the DNS settings first. The DNS server has to pointing to the domain controller's private IP address. On the Azure portal, open the Networking tab and click on Network Interface. In the DNS servers, enter the domain controller's private IP address and save the changes. Restart the client VM in order to ensure the DNS changes are saved. 
</p>
<br />

<p>
<img src="https://i.imgur.com/b1gUew4.png" height="80%" width="80%" alt="Configuration Steps"/>
<img src="https://i.imgur.com/N0Mnfoq.png" height="80%" width="80%" alt="Configuration Steps"/>
<img src="https://i.imgur.com/DkPUJNR.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
It is now time to make the client VM join the domain. In the System menu of the client VM, click on Rename this PC (advanced) and Change. Enter the domain and necessary credentials in order to let the client join the domain. I am logging in as Jane Doe for the purposes of the lab. It is important to note that the login credentials have to be input within the context of the domain path. The client should now be part of the domain. On the domain controller, the client should now appear in Computers in the Active Directory Users and Computers panel.
</p>
<br />

<p>
<img src="https://i.imgur.com/jmR2LXa.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
Before users in the domain can use the client computer, Remote Desktop has to be enabled for non-administrative users. While logged in as the administrator (in my case, Jane), open System Properties. Click on Remote Desktop and Select users that can remotely access this PC. Allow Domain Users access to Remote Desktop. Non-administrative users can now log in to Client-1. Normally a Group Policy can do the same and allows changes to many systems at once. For the purposes of this lab, a Group Policy won't be used to make this change.
</p>
<br />

<p>
<img src="https://i.imgur.com/HrI6BTq.png" height="80%" width="80%" alt="Configuration Steps"/>
<img src="https://i.imgur.com/c7LaN48.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
Creating users can be done manually or through the use of a script. For this lab, I will be using a PowerShell script. The PowerShell script can be found <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1)"> here. </a> On the domain controller, open PowerShell ISE as an administrator (and make sure you are logged in with an admin account on the domain controller). Create a new file and paste the script into ISE console. Run the script and observe the accounts being created. 
</p>
<br />

<p>
<img src="https://i.imgur.com/Xn5tQU2.png" height="80%" width="80%" alt="Configuration Steps"/>
</p>
<p>
After creating the users, Client-1 can now be signed in as one of the new users that were created from the PowerShell script. Pick a name and simply sign in to the client with the context of the domain. In my case, it is ernestotest.com\bon.rovej.
</p>
<br />

<h2>Lessons Learned</h2>

Doing this lab has made me learn how to set up Active Directory and join clients to the domain. I also created users and assigned the necessary permissions. Active Directory is not difficult to learn despite all the menu navigation that takes place. This lab is a segway for me to learn about DNS settings in-depth and file permissions in action. I will go into detail about these topics in other labs.
