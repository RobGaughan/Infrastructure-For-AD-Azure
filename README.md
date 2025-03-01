# Deploying Hybrid Azure Active Directory with MFA in Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Hybrid Azure Active Directory with MFA in Azure</h1>



<h2>Video Demonstration</h2>



<h2>Environments and Technologies Used</h2>



<h2>Operating Systems Used </h2>



<h2>High-Level Deployment and Configuration Steps</h2>

1. Create an Azure Virtual Network (VNet)
2. Deploy a Windows Server VM 
3. Install Active Directory 
4. Create an Azure AD (Microsoft Entra ID) Tenant
5. Enable Azure AD (Microsoft Entra ID)  Connect on the On-Prem VM
6. Verify Sync Status
7. Enable Multi-Factor Authentication (MFA)
8. Set Conditional Access Policies
9. Test Authentication

<h2>Deployment and Configuration Steps</h2>

### 1. Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below
   
![Pasted image 20250228191832](https://github.com/user-attachments/assets/e52b2502-33b0-4e63-bf2c-f8391ec1f44f)

 
- Click "Create new" and create a new resource group called "AD-Hybrid-lab" <br>

- Name the Vnet "AD-Hybrid-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![Pasted image 20250228195212](https://github.com/user-attachments/assets/ec630dc3-11d7-4af7-b260-2ca4f212ee12)




### 2. Deploy a Windows Server VM and place it in the Vnet
- Navigate to the Virtual machines page and click on create
  
![Pasted image 20250228195539](https://github.com/user-attachments/assets/8d6986e1-686c-4a5f-909c-57191158e23c)

Configure the following in the highlighted areas in the picture below:
- resource group:
   - Select the "AD-Hybrid-lab" group
- Virtual machine name:
    - Name it "Domain-Controller-1"
 - Region
    - ***Select same region as the vnet from the previous step in my case it is East US***
- Image:
   - select "Windows Server 2022 Datacenter: Azure Edition - x64 Gen2"
- Size: 
   - select "Standard_D4s_v3 - 4 vcpus, 16 GIB memory"
- Username and password
   - For lab purposes I chose Username: LabADMIN Password: LabPassword123

> [!IMPORTANT]
> Make sure the region matches the Vnet's region

![Create a virtual machine - Microsoft Azure - portal azure com](https://github.com/user-attachments/assets/32a1c163-8d6e-40c5-bca9-704d3b8a028c)

> [!IMPORTANT]
> Navigate to the "Networking" tab and Verify that AD-Hybrid-VNET is selected for virtual network

After you verify then "click "Review + create" and proceed to create it 

![image](https://github.com/user-attachments/assets/b015a682-dffa-485a-be09-c9170248d818)


Set the VM to have a static IP to do that we must do the following:

Go to Virtual Machines --> Domain-Controller-1 --> Networking --> Network settings --> NIC --> IPconfig --> Private IP address settings --> select static and save 

Refer to the gif below: 
![ezgif-872af4bccc6f3e](https://github.com/user-attachments/assets/8962c61e-0581-4d65-887c-30373dcba181)

### 3. Install Active Directory 
First we must RDP into Domain-Controller-1 to do that we need to find its public IP address

We can find the IP address by navigating to: 
 - Virtual Machines --> Domain-Controller-1 --> Networking --> Network settings
   
![image](https://github.com/user-attachments/assets/36c65ae5-cd40-42f9-ae4a-7f5e4c3c7b91)

- Next we will RDP in using Remote Desktop Connection using the public IP 
   - The login credential will be Username: LabADMIN Password: LabPassword123 from earlier

![image](https://github.com/user-attachments/assets/e2ed221a-4c8e-44bc-b203-4bfc1720ae35)

- When you connect you should be met with this screen
   -To begin installing Active Directory click on "add roles and files"
  
  ![418238521-7925871f-0282-4e5b-a517-ed33bbcd5fc4](https://github.com/user-attachments/assets/00bd1956-3a31-4375-ae36-3b3b285224c9)

- Next we will be met with the activation wizard we can click next on all prompt until we get to the "Server roles" secction

![image](https://github.com/user-attachments/assets/1dbe9ded-acb3-4b01-824a-4d1160ee089f)


 - On the "Server roles" secction check the highlighted "Active Directory Domain Services" box accept any addtional prompts and hit next until you get to "Confirmation"

![image](https://github.com/user-attachments/assets/e87b07f4-50f5-466b-9f5f-048fe84dc7e8)

 - Check the "Restart the destination server automatically if required" and click install
   
![image](https://github.com/user-attachments/assets/d28d5c80-7f28-455a-afe5-39b30ec4b33a)


![image](https://github.com/user-attachments/assets/b0fe3318-9eee-4a6e-843f-86179d20f2f5)

![image](https://github.com/user-attachments/assets/0c471f93-38a7-4c8c-8278-e8927ae05579)

![image](https://github.com/user-attachments/assets/e2aba41f-d5a8-4205-92d8-6dc92a67031e)

- uncheck "Create DNS delegation
![image](https://github.com/user-attachments/assets/0fc79da9-cec5-44f6-80a5-3cbca26a7579)









