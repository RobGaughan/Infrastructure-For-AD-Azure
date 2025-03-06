# Preparing infrastructure for Active Directory in Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>


<h1>Preparing infrastructure for Active Directory in Azure</h1>
In this lab we will be creating the infrastructure required to host an on premises deployment of Active Directory using Azure  

We will install nessary Virtual networks and Virtual machines as well as installing Active directory. 

Then we will create a client virtual machine that will act as an end user.  



<h2>Video Demonstration</h2>



<h2>Environments and Technologies Used</h2>



<h2>Operating Systems Used </h2>

<h2>Azure Terminology</h2>

![0](https://github.com/user-attachments/assets/66ceaafe-ba90-49ab-b6f4-e61ef6482e33)


When working with Azure it is important to understand the terminology used in our lab :

A Tenant is an Organization, It is created when an organization or single entity create an azure account

Inside The Tenant is the Subscription which is the payment for the services azure provides it is also useful for resource organization as well as placing quotas or limits for certain services.

Inside The Subscription we will create a resource group which functions like a folder in azure as it groups multiple resources together


Inside the Resource group we will create a VNET of (10.0.0.0/16)

Inside the Vnet we will create a subnet of 10.0.0.0/24
 
Inside the subnet we will create our VMs Domain-Controller-1 and Client VM 


<h2>High-Level Deployment and Configuration Steps</h2>

1. Create an Azure Virtual Network (VNet)
2. Create a Windows Server VM
3. Create Client VM
4. Configure DNS settings
5. Verify DNS settings on Client-1  


<h2>Deployment and Configuration Steps</h2>

### 1. Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below

  
![image](https://github.com/user-attachments/assets/32ef39ea-8bb9-493d-9e6d-512f20bc51af)

- Click "Create new" and create a new resource group called "AD-LAB" <br>

- Name the Vnet "AD-LAB-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![2](https://github.com/user-attachments/assets/a6a12c6a-eddb-4763-ac38-9800de36d89a)

### 2. Deploy a Windows Server VM and place it in the Vnet
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
> Navigate to the "Networking" tab and Verify that AD-Hybrid-VNET is selected for virtual network

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
| Image:  | Windows 1O Pro, version 22H2 - x64 Gen2|
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


#### Disable firewall on Domain-Controller-1

![image](https://github.com/user-attachments/assets/797d96bb-6f3c-4394-a15a-c6c13d70d976)

![12](https://github.com/user-attachments/assets/2c337af3-b0f8-4e01-adf4-73bdd78f52f1)


Make sure you tunr off Domain Profile, Private profile, and Public profile

![image](https://github.com/user-attachments/assets/1a201109-b571-4b7b-bb4e-0fbd9b0a4402)



Connect to Client-1 

![image](https://github.com/user-attachments/assets/fac010a6-f9c2-40b7-af21-3e2544dd1d22)

login using RDP 

![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)

![17](https://github.com/user-attachments/assets/14351f25-1de5-4d5e-ae45-22f8b0aa3324)












