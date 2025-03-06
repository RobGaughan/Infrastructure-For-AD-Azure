# Preparing infrastructure for Active Directory in Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>


<h1>Preparing infrastructure for Active Directory in Azure</h1>
In this lab we will be creating the infrastructure required to host an on premises deployment of Active Directory using Azure  

We will install nessary Virtual networks and Virtual machines as well as installing Active directory. 

Then we will create a client virtual machine that will act as an end user.  


<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Virtual Machines
- Virtual Networking
- Active Directory 

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter: Azure Edition - x64 Gen2
- Windows 10 Pro, version 22H2 - x64 Gen2

<h2>Azure Terminology</h2>

![0](https://github.com/user-attachments/assets/66ceaafe-ba90-49ab-b6f4-e61ef6482e33)


When working with Azure it is important to understand the terminology used in our lab :

A Tenant is an Organization, It is created when an organization or single entity creates an azure account

Inside The Tenant is the Subscription which is the payment for the services azure provides it is also useful for resource organization as well as placing quotas or limits for certain services.

Inside The Subscription we will create a resource group which functions like a folder in azure as it groups multiple resources together


Inside the Resource group we will create a VNET of 10.0.0.0/16

Inside the Vnet we will create a subnet of 10.0.0.0/24
 
Inside the subnet we will create our VMs Domain-Controller-1 and Client VM 


<h2>High-Level Deployment and Configuration Steps</h2>

1. Create an Azure Virtual Network (VNet)
2. Create a Windows Server VM
3. Create Client VM
4. Configure DNS settings
5. Verify DNS settings on Client-1  


<h2>Deployment and Configuration Steps</h2>

### Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below

  
![image](https://github.com/user-attachments/assets/32ef39ea-8bb9-493d-9e6d-512f20bc51af)

- Click "Create new" and create a new resource group called "AD-LAB" <br>

- Name the Vnet "AD-LAB-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![2](https://github.com/user-attachments/assets/a6a12c6a-eddb-4763-ac38-9800de36d89a)

### Deploy a Windows Server VM and place it in the Vnet
- Navigate to the Virtual machines page and click on create
  
![Pasted image 20250228195539](https://github.com/user-attachments/assets/8d6986e1-686c-4a5f-909c-57191158e23c)

Configure the following in the highlighted areas in the picture below:

| Selection  | Configuration|
| ------------- | ------------- |
| Resource group: | AD-LAB  |
| Virtual machine name:  |  Domain-Controller-1 |
| Region  | ***Select same region as the vnet from the previous step in my case it is East US***|
| Image:  | Windows Server 2022 Datacenter: Azure Edition - x64 Gen2 |
| Size:   | Standard_D2s_v3 - 2 vcpus, 8 GiB memory |
| Username |  LabADMIN |
| Password  | LabPassword123 |

> [!IMPORTANT]
> Make sure the region matches the Vnet's region

![4](https://github.com/user-attachments/assets/51c1e296-2b54-4217-84b2-8cf238a79400)


> [!IMPORTANT]
> Navigate to the "Networking" tab and Verify that AD-LAB-VNET is selected for virtual network

After you verify then click "Review + create" and proceed to create it 

![5](https://github.com/user-attachments/assets/7c5bbef9-1e25-4010-853f-0874bd21fea6)




### Create Client VM

Click on "Create" 
![6](https://github.com/user-attachments/assets/e1d688df-e652-4a89-8dbe-2a963c2e5786)

Configure the following in the highlighted areas in the picture below:

| Selection  | Configuration|
| ------------- | ------------- |
| Resource group: | AD-LAB  |
| Virtual machine name:  |  Client-1 |
| Region  | ***Select same region as the vnet from the previous step in my case it is East US***|
| Image:  | Windows 10 Pro, version 22H2 - x64 Gen2|
| Size:   | Standard_D2s_v3 - 2 vcpus, 8 GiB memory |
| Username |  LabADMIN |
| Password  | LabPassword123 |

Make sure to check the box under "Licensing" Then click "Review + create" and proceed to create it 

![7](https://github.com/user-attachments/assets/a6dea4db-440e-4203-937f-168dbe4ada9e)


### DNS Configuration

In order for Active Directory to work properly and join the domain we must configure Client-1 to use Domain-Controller-1 as its DNS server


#### Description of diagram
**On the left/orange section:** 
- This is what Azure deaults to when we create the client VM and when we created the Domain-Controller-1 VM
- It Defaults to using the Vnet's Virtual DNS server

**On the right/blue section:**
- What we will be configuring our Domain-Controller-1 VM and Client-1 VM
- We will be configuring Client-1 to use Domain-Controller-1 as its DNS server instead

![image](https://github.com/user-attachments/assets/dc6edbc0-9a9f-4a89-bcd1-3c5e3be7d484)


Do do this configuration we will need to do the following steps:
1. Configure a static IP on  Domain-Controller-1
2. Then we must tell Client-1 to use Domain-Controller-1 as its DNS server

#### Setting Domain-Controller-1 Static IP 
Set the VM to have a static IP to do that we must do the following:

Go to Virtual Machines > Domain-Controller-1 > Networking > Network settings > NIC > IPconfig > Private IP address settings > select static and save 

> [!NOTE]
> Take note of the static IP you set we will be using it in the next step  
> For me it was 10.0.0.4 

Refer to the gif below: 

![ezgif-872af4bccc6f3e](https://github.com/user-attachments/assets/8962c61e-0581-4d65-887c-30373dcba181)

#### Setting Client-1 DNS settings:

Go to Virtual Machines > Client-1 > Networking > Network settings > NIC > DNS servers

For "DNS servers" > Select the "custom" box  
Under "DNS Server" > Put 10.0.0.4 which is Domain-Controller-1's IP   
Then click save

Refer to the gif below:   

![cropped-client-1-dns-settings](https://github.com/user-attachments/assets/d0d6d329-9cf9-4cd5-96bd-a7f4700afc86)

Restart VM for DNS settings to take hold:

![10](https://github.com/user-attachments/assets/13d97191-af38-4ea7-a1d4-3724531bf9e6)





### Disable firewall on Domain-Controller-1

For lab purposes we will be disabling the firewall on Domain-Controller-1 for the DNS settings to take hold a bit easier  
***Keep in mind this is for lab purposes only and done for brevity this should not be done on a live enterprise enviroment*** 

#### Connect to Domain-Controller-1
Virtual Machines > Domain-Controller-1 > Networking > Network settings 

From here we want to take note of the public IP address we will be using this to remote into Domain-Controller-1

![18](https://github.com/user-attachments/assets/e7e60b6e-0fe5-4f7f-9fb2-396aa86e7c11)

Using Remote Desktop Connection enter the public IP you found in the last step  
Username: LabADMIN  
Password: LabPassword123  
Then click "connect"

![19](https://github.com/user-attachments/assets/05c136e9-4118-496f-a9ca-247459e5c0fb)

One we are in the VM press: Windows key + R and enter "wf.msc" to bring up firewall settings  

![11](https://github.com/user-attachments/assets/e918b547-4748-4183-bd26-58c98ee4ef41)

Next Click " Windows Defender Firewall Properties"

![12](https://github.com/user-attachments/assets/2c337af3-b0f8-4e01-adf4-73bdd78f52f1)

Turn off highlighted section  
Make sure you turn off Domain Profile, Private profile, and Public profile

![image](https://github.com/user-attachments/assets/1a201109-b571-4b7b-bb4e-0fbd9b0a4402)

### Verifiying Client-1 DNS settings
Connect to Client-1 

Find client-1 public IP address:  
Virtual Machines > Client-1 > Networking > Network settings 

![image](https://github.com/user-attachments/assets/35965722-d05f-4c90-be6c-a737d71cd161)


login using Remote Desktop Connection

![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)


Once inside Client-1 VM open PowerShell and use command "ipconfig /all" to display its DNS server  
Verify That it is Domain-Controller-1's IP address

![17](https://github.com/user-attachments/assets/14351f25-1de5-4d5e-ae45-22f8b0aa3324)

Thats it!  
We are all done creating the necessary infrastructure to support Active Directory in the next steps we will install active directory on Domain-Controller-1 and join Client-1 to the active directory domain  

##  Next steps

[Deploying Active Directory in Azure](https://github.com/RobGaughan/Deploying-Active-Directory-in-Azure)









