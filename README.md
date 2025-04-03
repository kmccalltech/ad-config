<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Configuration and Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines then using PowerShell for user creation

<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/BxtQWIF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create two VMs in Azure ensuring that they are in the SAME VIRTUAL NETWORK and SUBNET MASK (see networking tab when creating VM).

Name one dc-1 (Domain Controller) and set the image to Windows Server 2022.

Name second VM client-1 and set the image to Windows 10.
</p>
<br />

<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 11 AM" src="https://i.imgur.com/VTplmed.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 26 AM" src="https://i.imgur.com/WfbPumo.png" />
    </td>
  </tr>
</table>
<p>
Click into dc-1 -> Network Settings -> Network Interface. We will configure dc-1 to have a static IP address.</p>
<p>Navigate to ipconfig1 -> select Static </p>
<br />

<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 11 AM" src="https://i.imgur.com/TLa8W4j.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 26 AM" src="https://i.imgur.com/Od5Hy5a.png" />
    </td>
  </tr>
</table>
Using dc-1's public IP address, remote desktop into dc-1. Navigate to the start menu -> run -> search for wf.msc

This will open up the firewall settings for DC-1. Go to Windows Defender Firewall Properties and turn off the firewall for each tab. Be sure to click apply when finished.
After that go to Client-1's Network Settings -> Network Interface -> DNS settings. Enter in DC-1's private IP address (this can be found in DC-1's network settings or overview tab). Be sure to click save. Then once the settings are saved navigate back to the virtual machines homepage and give Client-1 a restart by checking its box and clicking "Restart"
When Client-1 has restarted, remote desktop into Client-1 using its public IP address.

</p>
<br />

<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 11 AM" src="https://i.imgur.com/P5tIpbT.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 26 AM" src="https://i.imgur.com/GqyIPQT.png" />
    </td>
  </tr>
</table>
<p>Within Client-1 open up PowerShell and ping DC-1 to ensure that both VMs are on the same virtual network and that DC-1's firewall is disabled.

Next, type "ipconfig /all and look to make sure the DNS server of Client-1 is set to DC-1's private IP address.</p>
<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 11 AM" src="https://i.imgur.com/Z5JmO3s.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 26 AM" src="https://i.imgur.com/rWaaiyn.png" />
    </td>
  </tr>
</table>
<p>Log in to DC-1 and in server manager click "Add role and features". From here add a feature to install Active Directory Domain Services. Click through next to install AD DS.

Once AD DS is installed, go to the top right flag in the server manager and promote the server to become a domain controller. Add a new forest named mydomain.com and continue through the setup wizard. (Be sure to uncheck DNS delegation as you progress through the setup

Once the setup is finished, the VM will reboot. You may have to login again and if so that user is now a domain contoller so you have to put "mydomain/username" for the username for it to identify it</p>
<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 11 AM" src="https://i.imgur.com/6HutEWL.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 11 14 26 AM" src="https://i.imgur.com/4YDcel0.png" />
    </td>
  </tr>
</table>
<p>Open Active Directory Users & Computers from within the start menu. By right clicking on mydomain, create a new Organizational Unit (OU).

Create one named _EMPLOYEES and create another named _ADMINS

Once both OUs are created, within _ADMINS right click to create a new user. I named this user Ken Admin and configure his login credentials.

Something like ken_admin and password Cyberlab123 keeping it simple for this project .</p>
<br>
<p>
  <img height="80%"  width="80%" alt="Screenshot 2025-01-10 at 12 24 26 PM" src="https://i.imgur.com/YzYMEEs.png" />
</p>
<p>Once he is created, right click on him to access Properties -> Members of, and add him to "Domain Admins". Be sure to click "Check Names" and apply when finished.</p>
<p>Log out of DC-1 and log back in as “mydomain.com\ken_admin”</p>
 <p>We can use ken_admin as the admin account from now on
</p>
<table>
  <tr>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 12 48 48 PM" src="https://i.imgur.com/1TeD9xb.png" />
    </td>
    <td>
      <img width="1000" alt="Screenshot 2025-01-10 at 12 49 59 PM" src="https://i.imgur.com/Cy1I7lC.png" />
    </td>
  </tr>
</table>
<p>Login to Client-1 as the original local admin</p>
<p>From the start menu -> System -> Rename this PC Advanced -> Change, and enter mydomain.com</p>
<p>This will prompt a login screen where we can login as mydomain.com\ken_admin and join client-1 to the domain controller dc-1</p>
<br>

<p>We can verify the join was successful by going back into DC-1 and Active Directory Users and Computers (ADUS) and finding client-1 within the Computer tab.</p>
<p>Create a new OU named _CLIENTS and drag client-1 into it.</p>
<br>

<p><img height="70%" width="70%" alt="Screenshot 2025-01-10 at 1 57 16 PM" src="https://i.imgur.com/mjJbkNP.png" />
</p>
<p>Log into client-1 as Ken_Admin -> System -> Remote Desktop -> Select users that can remotely access this PC -> Add -> Domain Users -> Check names -> OK</p>
<p>This will allow any user under the Domain Users group in the domain controller to be able to log into client-1. We will create users in the User Creation lab. </p>
<p><img height="70%" width="70%" alt="Screenshot 2025-01-10 at 2 04 29 PM" src="https://i.imgur.com/obDKAV6.png" />
</p>

Within DC-1 as Jane.Admin, open PowerShell ISE as ADMINISTRATOR and paste the following [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

<p>Be sure to edit the password you want the users to have at the top of the script text.</p>


<p>Check within ADUC and under _EMPLOYEES to see all the created users. Select any user and log into client-1 to test access</p>
<p>All the created users should have access because we gave domain users access to remote desktop in client-1</p>

**You're all done! My next project will be going through group policy and managing accounts see you there!
