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

14.) Logout and log back into DC using your domain name\admin name (ex: mydomain.com\jane_admin).

15.) Now we are going to connect our Client-1 to DC-1 as our DNS. Start off by going back to azure portal and the DC-1 virtual machine information. Select network settings -> network interface -> Copy Private IP. Then go to client 1 -> network settings -> network interface -> DNS Servers -> Custom -> paste in the Private IP and save it. Lastly, you will want to restart the virtual machine to update the changes.

<p>
  
<img width="666" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/fc9209db-26fe-46d8-8f24-18c3fe0c9977">
</p>

16.) Log back in to Client-1 using your original admin (for me labuser). We are going to join the domain so right click on the windows icon and select system -> Rename this PC -> Change -> Domain and enter your domain -> ok and add your domain with \admin username and your password. Press ok on the popup and the server should restart.

<p>
  <img width="784" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/b94d4a34-2e09-4288-a4d9-e91d33bdf700">
</p>

17.) Log into Client-1 use your domainname\admin username. Next we are going to set up all users having access to the domain. Right click start menu -> system -> remote desktop -> select users that can remotely access this PC -> add -> add in Domain Users -> Check Names -> ok.

18.) Return to DC-1 and open powershell ISE by right-clicking and running as administrator from the search bar. Next copy the script from (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) to create some sample users-> create a new file -> paste the script in it and click run script.

<p>
  <img width="855" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/f3260577-df12-4f65-b7da-9c84ae84cd2f">

</p>

19.) Lastly, you can search Active Directory Users and Computers -> click on your domain.com -> _EMPLOYEES to see all the names created. You can also copy a name and actually log in using your domain name \ name on client-1 and the using the Password Password1 made by the script.

<p>
  <img width="422" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/5983d7b5-3ad8-419d-a852-3bd7dc2c1ac2">

</p>

<p>
  <img width="163" alt="image" src="https://github.com/JacobKnittle/Active-Directory-Install-and-Configure-Using-Azure/assets/124555008/4a104333-e202-444f-ad69-dbb94862d132">


</p>
