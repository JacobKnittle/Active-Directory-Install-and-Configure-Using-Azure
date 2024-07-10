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

<h2>Deployment and Configuration Steps</h2>

1.) This tutorial assumes you have worked with Azure and Azure Virtual Machines before.

2.) Start by creating a Domain Controller VM using Windows SErver 2022 and naming it DC-1. A resource group and virtual network (Vnet) will be created automatically.

<p>
  <img width="575" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/e59ccd23-e903-4875-9517-dcc96879781e">

</p>

3.) Next you will want to go to Virtual Machines -> DC-1 -> Networking -> Network Settings -> Network Interface -> IP configurations -> ipconfig1 -> change allocation to static

<p>
  <img width="1279" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/525cdb53-af60-4db2-8dbb-85a76fd7f9ff">

</p>

4.) Now create a Client VM with Windows 10 named Client-1. Make sure it has the same Resource Group and Vnet (check under the network tab) as DC-1.

<p>
  <img width="605" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/4d211d3c-1a67-47f5-9a43-cffb4fbbdc31">
</p>

5.) We are now going to log into client-1s public IP address using remote desktop to ping DC-1s private IP address with a perpetual ping.
