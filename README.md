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



<h2>High-Level Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/9e656ee5-e745-4ed0-9934-9088ce27b3cc)

1. Create an Azure Virtual Network (VNet)
2. Create a Windows Server VM
3. Install Active Directory
4. Promote Windows Server VM to a domain controller
6. Create Client VM
7. Configure DNS settings
8. Verify DNS settings on Client-1  


<h2>Deployment and Configuration Steps</h2>

### 1. Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below

  
![image](https://github.com/user-attachments/assets/32ef39ea-8bb9-493d-9e6d-512f20bc51af)

 
- Click "Create new" and create a new resource group called "AD-Hybrid-lab" <br>

- Name the Vnet "AD-Hybrid-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![image](https://github.com/user-attachments/assets/71ed9a7e-fa7e-4c61-9dc4-38bdade12583)




### 2. Deploy a Windows Server VM and place it in the Vnet
- Navigate to the Virtual machines page and click on create
  
![Pasted image 20250228195539](https://github.com/user-attachments/assets/8d6986e1-686c-4a5f-909c-57191158e23c)

Configure the following in the highlighted areas in the picture below:

| First Header  | Second Header |
| ------------- | ------------- |
| resource group: | AD-LAB  |
| Virtual machine name:  |  Domain-Controller-1 |
| Region  | ***Select same region as the vnet from the previous step in my case it is East US***|
| Image:  | Windows Server 2022 Datacenter: Azure Edition - x64 Gen2 |
| Size:   | Standard_D4s_v3 - 4 vcpus, 16 GIB memory |
| Username |  LabADMIN |
| password  | LabPassword123 |

> [!IMPORTANT]
> Make sure the region matches the Vnet's region

![image](https://github.com/user-attachments/assets/b0d267a6-be3f-4bfd-af95-945e443bd588)



> [!IMPORTANT]
> Navigate to the "Networking" tab and Verify that AD-Hybrid-VNET is selected for virtual network

After you verify then "click "Review + create" and proceed to create it 

![image](https://github.com/user-attachments/assets/f39c9c10-9492-4f8b-8b9c-6dee3e2e3825)







### Create Client VM
While we are waiting on the Domain-Controller-1 VM to restart this would be a good time to set up the client VM 

![image](https://github.com/user-attachments/assets/da25b6b7-1b01-4144-bc9a-8060aa9b5223)

![418949828-bf057c79-9a3a-4804-a6f1-e1e9102313f8](https://github.com/user-attachments/assets/6745ff9e-82e9-462e-a49e-f6728c170910)


Set the VM to have a static IP to do that we must do the following:

Go to Virtual Machines --> Domain-Controller-1 --> Networking --> Network settings --> NIC --> IPconfig --> Private IP address settings --> select static and save 

Refer to the gif below: 

![ezgif-872af4bccc6f3e](https://github.com/user-attachments/assets/8962c61e-0581-4d65-887c-30373dcba181)

setting Client-1 DNS settings:

![cropped-client-1-dns-settings](https://github.com/user-attachments/assets/d0d6d329-9cf9-4cd5-96bd-a7f4700afc86)

Restart VM for DNS settings to apply

![image](https://github.com/user-attachments/assets/1a4d4a8c-07cb-4020-87c3-e1a3754cb1b9)

Disable firewall on Domain-Controller-1

![image](https://github.com/user-attachments/assets/797d96bb-6f3c-4394-a15a-c6c13d70d976)

![image](https://github.com/user-attachments/assets/a56da034-d9bd-4e29-b3aa-54a9d1d110c9)

Make sure you tunr off Domain Profile, Private profile, and Public profile

![image](https://github.com/user-attachments/assets/1a201109-b571-4b7b-bb4e-0fbd9b0a4402)





#### Client VM DNS Configuration

Before we create the VM it is important to understand what we will be doing to simplify this I created the diagram below

**On the left/orange section:** 
- This is what Azure deaults to when we create the client VM and when we created the Domain-Controller-1 VM 

**On the right/blue section:**
- What we will be configuring our Domain-Controller-1 VM and Client-1 VM

Do do this configuration we will need to do the following steps:
1. Configure a static IP on  Domain-Controller-1
2. Then we must tell Client-1 to use Domain-Controller-1 as its DNS server as well as joining it to the Active Directory Domain

![image](https://github.com/user-attachments/assets/dc6edbc0-9a9f-4a89-bcd1-3c5e3be7d484)


Connect to Client-1 
![image](https://github.com/user-attachments/assets/fac010a6-f9c2-40b7-af21-3e2544dd1d22)

login using RDP 
![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)

![image](https://github.com/user-attachments/assets/7917b1a7-e7cb-4396-801c-b41e39b9c58d)












