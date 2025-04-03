<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
