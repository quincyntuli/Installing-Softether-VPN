
# Softether VPN Setup

## A Server Setup

### 01 Download Softether Server

Download the latest RTM version of Softether.  RTM's are preferred over beta versions for stability. At the time of writing these notes the latest version is v4.38-9760-rtm-2021.08.17.

```powershell
IWR -UseBasicParsing https://github.com/SoftEtherVPN/SoftEtherVPN_Stable/releases/download/v4.38-9760-rtm/softether-vpnserver_vpnbridge-v4.38-9760-rtm-2021.08.17-windows-x86_x64-intel.exe -Outfile softether-vpnserver_vpnbridge-v4.38-9760-rtm-2021.08.17-windows-x86_x64-intel.exe
```



### 02 Find Your IP Address


```powershell
Get-NetIPConfiguration | Select-Object -Property IPv4Address
```

In my case that address is 10.0.2.15, it will be different on yours.

### 03 Run Setup Wizard

On the `Select Software Component to Install` window select `SoftEther VPN Server`

On the `End User License Agreement` window scroll down and read the license agreement and select `I agree to the End User License Agreement` and click `Next` if you agree to the terms of use.

On the `Important Notices` window, scroll down to read the notice and click `Next`

On the `Directory to Install on` window click on `Next`.

On the `Ready to Install` window click on `Next`.

On the next window leave the `Start the Softether VPN Server Manager`  box checked and click on `Finished`.

### 04 Server Configuration

At this stage the setup finished and the Server Manager starts.

#### Define Server Settings

On the 'Connection Settings for VPN Server:'' window, select `Edit Setting`.
![[Pasted image 20230117175907.png]]

On the `Edit localhost (This server) window` , replace localhost (This server) with any name you like, in my case i simply replaced it with `vpnserver`

Replace `localhost` with the IP Address of the computer you have found earlier. In my case that address is 10.0.2.15, it will be different on your system.

Type in the password you would like to connect administrator mode. Take note of the password and make a note of it for the future.

Click Ok to complete editing the server settings.

Select the setting and Click on `Connect`.

Leave the password blank and click `OK`.

On the `Change Administrator Password` define the new password and click `OK`

On the `SoftEther VPN Server / Bridge Easy Setup` window check only the `Remote Access VPN Server` box and click `Next`.
![[Pasted image 20230117182329.png]]
Click Yes on the message block that asks `The current settings of this VPN Server. Do you really want to do this` .

#### Create Hub Name

On the Easy Setup - Decide the Virtual Hub Name  window, a name has been suggested as VPN. You can leave it as is and click `OK`. On my setting is chose the name `myVPNhub`. 
![[Pasted image 20230117182514.png]]
On the `Dynamic DNS Function` window click `Exit`. This is an optional step should you have access to a Dynamic DNS service.
![[Pasted image 20230117182752.png]]

On the IPsec / L2TP / EtherIP / L2TPv3 Settings window check the boxes for :
- `Enable L2TP Server Function (L2TP over IPsec)`
- `Enable L2TP Server Function (Raw L2TP with No Encryptions)`
Click `OK`
![[Pasted image 20230117183215.png]]
On the VPN Azure Service Settings window select `Disable VPN Azure` and click `OK`
![[Pasted image 20230117183515.png]]

#### Create User

On the VPN Easy Setup Tasks window, click on `Create Users`.
![[Pasted image 20230117185340.png]]
On the create New User window complete the details for
- User Name (example `vpnuser1` )
- Full Name (example `VPN User 1` )
Click  `Create Certificate`
![[Pasted image 20230117185851.png]]

#### Create Root Certificate

On the Create New Certificate window complete the details for
- Common Name (CN)
- Organization
- Organization Unit
- Country
- State
- Locale
Click `OK`
![[Pasted image 20230117190729.png]]

On the Save Certificate and Private key window 
- select the method Save as X509 Certificate (.CER) and Private Key file (.KEY).
- Set password for Private Key Protection (example `supersecret` )
- Click `OK`
![[Pasted image 20230117191112.png]]
- specify file name for .CER file to be saved and click `Save`
- specify file name for .KEY file to be saved and click `Save`
Click `OK`

#### Select Bridge Device

On the Manage Users window click on `Exit`
![[Pasted image 20230117191509.png]]

Back on the VPN Easy Setup Tasks window
- Select the Ethernet device to establish the bridge connection
Click `Close`
![[Pasted image 20230117192508.png]]

Back on the VPN Server Manager screen, delete the listener port 443 if the vpn server will be hosting secure web pages in the future.
![[Pasted image 20230117193207.png]]

On the Documents folder, take note of the certificate and key files. They will be required for the Client Installation.

<hr>







## B Client Setup

### 01 Download the software


Download the latest RTM version of Softether.  RTM's are preferred over beta versions for stability. At the time of writing these notes the latest version is v4.38-9760-rtm-2021.08.17.

```powershell
wget https://github.com/SoftEtherVPN/SoftEtherVPN_Stable/releases/download/v4.38-9760-rtm/softether-vpnclient-v4.38-9760-rtm-2021.08.17-windows-x86_x64-intel.exe
```



### 02 Find Your IP Address


```powershell
Get-NetIPConfiguration | Select-Object -Property IPv4Address
```

In my case that address is 192.168.1.115, it will be different on yours.


### 03 Run Setup Wizard

On the `Select Software Component to Install` window select `SoftEther VPN Client`

On the `End User License Agreement` window scroll down and read the license agreement and select `I agree to the End User License Agreement` and click `Next` if you agree to the terms of use.

On the `Important Notices` window, scroll down to read the notice and click `Next`

On the `Directory to Install on` window click on `Next`.

On the `Ready to Install` window click on `Next`.

Click `Finish` to complete the installation.


### 04 Client Configuration

On the `SoftEther VPN Client Manager` window click `Add VPN Connection`.

#### Create Virtual Adapter

Click `Yes` to create the Virtual Network Adapter.

On the `Create New Virtual Network Adapter` window click `OK`

#### Define Connection Settings

Define the connection's Setting Name

Define the Host Name or IP Address, this is the IP address of the server we want to connect to. In this example `192.168.1.105`

Select Port Number `5555`

Select Auth Type as `Client Certificate Authentication`

Define the username as defined earlier in the server configuration  `vpnuser1` 

Click `Specify Client Certificate`

Select the **certificate** as generated and saved on the Documents folder of the server. It is expected that the certificate files were copied over to the client computer at this stage. 
Select the **key** as generated and saved on the Documents folder of the server. It is expected that the certificate files were copied over to the client computer at this stage. 

On the Private Key Passphrase prompt, specify the password defined earlier `supersecret`

Click OK

The client has been setup and now you are ready to connect.









