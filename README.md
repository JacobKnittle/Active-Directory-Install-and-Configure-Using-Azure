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

5.) We are now going to log into client-1s public IP address using remote desktop to ping DC-1s private IP address with a perpetual ping. To get the private IP address for DC-1 go to Virtual Machines -> DC-1 -> network settings -> copy the private IP address.

<p>
  
<img width="993" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/eadd4e9c-6664-4d35-8abb-506fb974d10e">
</p>

<p>
  <img width="695" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/0935f385-93ac-403b-a30e-0320d0a29983">

</p>

6.) Return to client-1 VM and open the command prompt from the search bar and type ping -t (private IP address here).

<p>
  <img width="503" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/c1cd505d-a8d3-46a3-8a33-2e9e62b7775d">

</p>

7.) Next take the Public IP address from DC-1 and use it with remote desktop to login. On DC-1 search for Windows Defender Firewall -> Inbound Rules -> sort by selecting the Protocol tab -> Enable Rules for Core Networking Diagnostics. This will allow ICMP requests and replies to go back and forth between Client and DC.

<p>
  <img width="851" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/4cc44681-13d5-4a45-93c8-d54180670f46">

</p>

8.) We are now going to install Active Directory onto DC-1. Open up Server Manager on DC-1 -> Add Roles and features -> go through the install wizard and make sure you select ACtive Directory Domain Services before finishing the installation.

<p>
  <img width="957" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/1e031e15-d8a5-4320-86d4-d20282029c17">

</p>

9.) After it is done installing we are going to do some configuration starting by clicking on the flag at the top of Server Manager -> select Promote this server to a domain controller -> Add a new forest and name the root domain whatever you'd like. Setup a password and go through the installation wizard. Once completed your VM will restart.

<p>
<img width="960" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/80090954-68eb-452e-93f6-a355f4573805">
</p>

10.) When you log back in to your DC it is now set up using that domain name so change your username to whatever your domain name is and \username (Ex: mydomain.com\labuser) and your normal password.

<p>
  <img width="338" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/32bf9f4e-bc3b-4355-966e-948231090918">

</p>

11.) Next search for Active Directory Users and Computers on DC and we are going to create Organizational Unit (OU) which are basically folders. Right-click on your domain.com -> new Organizational Unit and name it _EMPLOYEES and then another one for _ADMINS.

12.) We will now create a admin user for us to use so right click on _ADMINS -> new -> user. Fill out your name and username along with setting up a password. Under the password part deselect the must change password and select the password never expires and finish the installation.

<p>
  <img width="244" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/5fd3f420-e82e-4437-a3ea-09979857a981">

</p>

<p>
  <img width="244" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/42d477c6-79a4-4211-8c43-2cda521ce5af">

</p>

13.) Next to make the account an admin we are going to right click your account in _ADMINS -> properties -> Member Of -> Add -> type domain -> check names -> Domain Admins -> ok.

<p>
  <img width="230" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/da351fcb-3b4f-4e43-b6aa-9b588e71fa9a">

</p>
