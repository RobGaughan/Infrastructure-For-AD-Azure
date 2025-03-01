# Hybrid-Azure-Active-Directory-with-MFA


<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Hybrid Azure Active Directory with MFA in Azure</h1>



<h2>Video Demonstration</h2>



<h2>Environments and Technologies Used</h2>



<h2>Operating Systems Used </h2>



<h2>High-Level Deployment and Configuration Steps</h2>

1. Create an Azure Virtual Network (VNet)
2. Deploy a Windows Server VM and place it in the Vnet
3. Install Active Directory 
4. Create an Azure AD (Microsoft Entra ID) Tenant
5. Enable Azure AD (Microsoft Entra ID)  Connect on the On-Prem VM
6. Verify Sync Status
7. Enable Multi-Factor Authentication (MFA)
8. Set Conditional Access Policies
9. Test Authentication

<h2>Deployment and Configuration Steps</h2>

### Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below
   
![Pasted image 20250228191832](https://github.com/user-attachments/assets/e52b2502-33b0-4e63-bf2c-f8391ec1f44f)

 
- Click "Create new" and create a new resource group called "AD-Hybrid-lab" <br>

- Name the Vnet "AD-Hybrid-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![Pasted image 20250228195212](https://github.com/user-attachments/assets/ec630dc3-11d7-4af7-b260-2ca4f212ee12)




### Deploy a Windows Server VM and place it in the Vnet
- Navigate to the Virtual machines page and click on create
  
![Pasted image 20250228195539](https://github.com/user-attachments/assets/8d6986e1-686c-4a5f-909c-57191158e23c)

Configure the following in the highlighted areas in the picture below:
- In resource group:
   - Select the "AD-Hybrid-lab" group
- For the Virtual machine name:
    - Name it "Domain-Controller-1"
 - Region
    - ***Select same region as the vnet from the previous step in my case it is East US***
- Image:
   - select "Windows Server 2022 Datacenter: Azure Edition - x64 Gen2"

> [!IMPORTANT]
> Make sure the region matches the Vnet's region

![Create a virtual machine - Microsoft Azure - portal azure com](https://github.com/user-attachments/assets/32a1c163-8d6e-40c5-bca9-704d3b8a028c)



