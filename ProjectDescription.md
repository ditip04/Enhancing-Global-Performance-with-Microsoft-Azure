# Deploying an Online Movie Watching Application on Microsoft Azure

## 1. Project Overview

This project demonstrates the design and deployment of a cloud-based online movie application using Microsoft Azure.

An online entertainment provider, experienced website performance issues as its global user base increased. Serving website content from a single location resulted in slower page-loading times, particularly for geographically distributed users.

To address these challenges, an Azure-based cloud solution was designed and implemented using Platform as a Service (PaaS), cloud storage, content delivery, DNS management, and virtual machine connectivity.

The solution incorporates Azure App Service for application hosting, Azure Storage for static website content and shared files, Azure Front Door for global content delivery, Azure Traffic Manager for DNS-level traffic routing, and Azure Virtual Machines for infrastructure connectivity testing.

## 2. Project Objectives

The main objectives of this project were:

- Design an appropriate Azure cloud architecture to support global website traffic.
- Deploy a web application using Azure App Service.
- Upload static website content to Azure Storage.
- Configure Azure Front Door to distribute and cache static content.
- Understand DNS-level traffic management using Azure Traffic Manager.
- Implement resource governance across Development, Testing, and Production environments.
- Apply resource tagging and Role-Based Access Control (RBAC).
- Monitor and manage cloud expenditure using Azure Cost Management.
- Create an Azure File Share for centralised team file sharing.
- Connect a Windows Virtual Machine to Azure Storage using the SMB protocol.
- Verify application availability, storage accessibility, and VM-to-storage connectivity.

## 3. Azure Services and Technologies Used

| Azure Service | Purpose |
|---|---|
| Azure App Service | Hosts the Online web application. |
| Azure Storage Account | Stores static website content and shared files. |
| Azure Static Website | Hosts HTML, CSS, JavaScript, and image files. |
| Azure Front Door | Provides global content delivery and caching. |
| Azure Traffic Manager | Provides DNS-level traffic routing between application endpoints. |
| Azure DNS | Manages DNS zones and domain name resolution. |
| Azure Virtual Machine | Provides a Windows compute environment for connectivity testing. |
| Azure Files | Provides shared file storage for project team members. |
| Azure Resource Groups | Organises and separates cloud resources. |
| Azure RBAC | Controls user permissions and resource access. |
| Azure Cost Management | Monitors cloud expenditure and manages budgets. |

## 4. Solution Architecture

The proposed architecture uses Microsoft Azure services to improve website performance, support global traffic distribution, and provide centralised storage.

### High-Level Architecture

```text
                  GLOBAL USERS
                       |
                       v
                  AZURE DNS
                       |
                       v
              AZURE TRAFFIC MANAGER
                       |
              +--------+--------+
              |                 |
              v                 v
        AZURE APP SERVICE  AZURE APP SERVICE
         PRIMARY REGION    SECONDARY REGION
              |                 |
              +--------+--------+
                       |
                       v
                AZURE STORAGE
                       |
              +--------+--------+
              |                 |
              v                 v
          $web Container    teamfiles Share
              |                 |
              v                 v
        AZURE FRONT DOOR     AZURE FILES
          / CDN Layer           |
                                v
                           WINDOWS VM
```

*Note: The diagram represents the proposed production architecture. The lab implementation focused on deploying and validating the individual Azure services using the resources available in the environment.*

### Architecture Components

1. **Azure DNS:** Provides DNS hosting and domain name resolution.
2. **Azure Traffic Manager:** Supports DNS-based routing between primary and secondary application endpoints.
3. **Azure App Service:** Hosts the online movie application without requiring management of the underlying server infrastructure.
4. **Azure Storage:** Stores static website content and shared team files.
5. **Azure Front Door:** Distributes static content through a global delivery network.
6. **Azure Files:** Provides centralised file sharing for project team members.
7. **Windows Virtual Machine:** Connects to Azure Files using SMB for shared file access.

## 5. Implementation and Deployment

### Phase 1: Cloud Solution Design and Resource Governance

Designed an Azure cloud solution to address the website performance and global traffic management requirements.

**Resource Group Strategy**

Separate resource groups were proposed to logically organise resources across different environments.

| Environment | Resource Group |
|---|---|
| Development | `rg-bingewatch-dev` |
| Testing | `rg-bingewatch-test` |
| Production | `rg-bingewatch-prod` |

**Resource Tagging**

Recommended resource tags were defined to support ownership identification and cost tracking.

| Tag | Example Value |
|---|---|
| Project | BingeWatch |
| Environment | Dev / Test / Prod |
| Owner | BingeWatch Team |
| Department | Entertainment |
| CostCenter | BW001 |

**Role-Based Access Control (RBAC)**

RBAC was incorporated into the governance design to provide permissions based on user responsibilities.

- Administrators: Manage cloud resources.
- Developers: Manage application resources.
- Testers: Access testing resources.
- Read-only users: View resources using Reader permissions.

**Outcome:** Established a structured approach to managing resources, permissions, and environments across the proposed cloud architecture.

### Phase 2: Azure App Service – Web Application Deployment

Deployed the Online website using Azure App Service.

**App Service Configuration:**

| Parameter | Configuration |
|---|---|
| Service | Azure App Service |
| Application 1 | `app-bwo-cus` |
| Application 2 | `app-bwo-isc` |
| Deployment Method | Manual Push |
| Deployment Directory | `/wwwroot` |

**Website Structure:**

```text
BingeWatchWebsite/
│
├── index.html
├── about.html
│
├── css/
│   └── style.css
│
├── js/
│   └── script.js
│
└── images/
    ├── movie1.jpg
    ├── movie2.jpg
    └── movie3.jpg
```

**Implementation Steps:**

1. Created the Azure App Service resources.
2. Prepared the static website files using HTML, CSS, and JavaScript.
3. Accessed the App Service deployment environment.
4. Uploaded the website files to the `/wwwroot` directory.
5. Verified the deployed directory structure.
6. Accessed the application using the App Service URL.
7. Confirmed that the website loaded successfully.

**Outcome:** Successfully deployed and verified the Online website using Azure App Service.

### Phase 3: Azure Storage – Static Website Hosting

Created an Azure Storage Account to host static website content separately from the application hosting environment.

**Storage Account Configuration:**

| Parameter | Configuration |
|---|---|
| Storage Account | `bwostaticglobal` |
| Service | Azure Storage |
| Feature | Static Website Hosting |
| Container | `$web` |

**Static Website Structure:**

```text
bwostaticglobal/
│
└── $web/
    ├── index.html
    ├── about.html
    ├── css/
    ├── js/
    └── images/
```

**Implementation Steps:**

1. Created the Storage Account named `bwostaticglobal`.
2. Enabled the Static Website feature.
3. Accessed the `$web` container.
4. Uploaded the HTML, CSS, JavaScript, and image files.
5. Retrieved the static website endpoint.
6. Accessed the endpoint through a web browser.
7. Verified that the website content loaded successfully.

**Outcome:** Successfully configured Azure Storage Static Website Hosting and verified the availability of website content through the storage endpoint.

### Phase 4: Azure Front Door – Global Content Delivery

Configured Azure Front Door to distribute static website content and improve content delivery for geographically distributed users.

**Front Door Configuration:**

| Parameter | Configuration |
|---|---|
| Profile Name | `bwocdn` |
| Pricing Tier | Azure Front Door Standard |
| Origin Type | Storage (Static Website) |
| Origin Host | `bwostaticglobal.z6.web.core.windows.net` |
| Origin Group | `default-origin-group` |
| HTTP Port | 80 |
| HTTPS Port | 443 |

**Implementation Steps:**

1. Created an Azure Front Door profile named `bwocdn`.
2. Selected the Azure Front Door Standard pricing tier.
3. Configured the Azure Storage Static Website endpoint as the origin.
4. Set the origin host and origin host header.
5. Created the origin group named `default-origin-group`.
6. Configured a route to forward incoming requests to the storage origin.
7. Retrieved the Azure Front Door endpoint domain.
8. Tested the endpoint to verify static content delivery.

**Outcome:** Successfully configured Azure Front Door with Azure Storage as the origin to deliver static website content through the global content delivery layer.

### Phase 5: Azure Files – Shared Storage for Team Members

Created an Azure File Share to provide centralised storage for project team members.

**Azure Files Configuration:**

| Parameter | Configuration |
|---|---|
| Storage Account | `bwostaticglobal` |
| Storage Service | Azure Files |
| File Share Name | `teamfiles` |

**Example Files Uploaded:**

```text
bwostaticglobal/
│
└── teamfiles/
    ├── team-notes.txt
    ├── movie1.jpg
    ├── movie2.jpg
    └── requirements.txt
```

**Implementation Steps:**

1. Accessed the `bwostaticglobal` Storage Account.
2. Navigated to the File Shares section.
3. Created a File Share named `teamfiles`.
4. Uploaded sample project files through the Azure Portal.
5. Verified that the files were accessible within the File Share.

**Outcome:** Successfully implemented centralised file storage to support project collaboration and shared access.

### Phase 6: Windows VM – Azure Files Connectivity Using SMB

Connected a Windows Azure Virtual Machine to the `teamfiles` Azure File Share using the Server Message Block (SMB) protocol.

**VM Configuration:**

| Parameter | Configuration |
|---|---|
| Virtual Machine | `vm-bingewatch` |
| Operating System | Windows |
| Remote Access | Remote Desktop Protocol (RDP) |
| Storage Service | Azure Files |
| Protocol | SMB |
| Network Drive | `Z:` |

**Azure File Share UNC Path:**

```text
\\bwostaticglobal.file.core.windows.net\teamfiles
```

**Implementation Steps:**

1. Created a Windows Virtual Machine in Azure.
2. Connected to the VM using Remote Desktop Protocol (RDP).
3. Accessed the Azure File Share connection details.
4. Configured the SMB connection using the Azure File Share UNC path.
5. Mapped the shared folder as the `Z:` network drive.
6. Opened the mapped drive through Windows File Explorer.
7. Verified that the files uploaded through the Azure Portal were accessible from the VM.

**Two-Way Connectivity Testing**

To verify file-sharing functionality, two-way file transfer tests were performed.

**Test 1: Windows VM to Azure File Share**

- Created a test file named `vm-test.txt` from the Windows VM.
- Saved the file inside the mapped `Z:` network drive.
- Refreshed the Azure Portal.
- Verified that `vm-test.txt` appeared in the `teamfiles` File Share.

**Test 2: Azure File Share to Windows VM**

- Uploaded a file named `portal-test.txt` through the Azure Portal.
- Accessed the mapped `Z:` drive from the Windows VM.
- Verified that `portal-test.txt` was accessible.

**Connectivity Workflow:**

```text
          WINDOWS VM
          vm-bingewatch
                |
                |
           SMB Protocol
             Port 445
                |
                v
         AZURE FILE SHARE
             teamfiles
                |
                v
         NETWORK DRIVE
                Z:
```

**Outcome:** Successfully established SMB connectivity between the Windows VM and Azure Files, demonstrating two-way file access between the VM and cloud storage.

### Phase 7: Azure Traffic Manager and DNS Management

Azure Traffic Manager and Azure DNS were included in the proposed architecture to support DNS-level traffic routing and global application access.

**Proposed Configuration:**

- Azure Traffic Manager manages application endpoints.
- Primary and secondary Azure App Service endpoints can be registered.
- Traffic Manager can use health monitoring and configured routing methods to direct users to appropriate endpoints.
- Azure DNS can host the organisation's DNS zone and manage application domain records.

**Outcome:** Defined the DNS and traffic management components required to extend the solution into a multi-region production architecture.

### Phase 8: Azure Cost Management and Billing

Azure Cost Management was incorporated into the governance strategy to monitor cloud expenditure and control resource costs.

**Key Cost Management Activities:**

- Monitor current cloud expenditure.
- Analyse costs by resource and Azure service.
- Track spending trends.
- Create project budgets.
- Configure budget alerts.
- Identify resources contributing to expenditure.
- Remove unused resources after testing.

**Example Budget Alert Thresholds:**

| Spending Threshold | Alert Level |
|---|---|
| 50% | Monitoring Alert |
| 75% | Warning |
| 90% | Critical Warning |
| 100% | Budget Reached |

Costs can also be analysed using resource groups, subscriptions, services, and resource tags.

**Outcome:** Established a cost-monitoring and budget-management approach to improve cloud financial visibility and resource utilisation.

## 6. Security Considerations

Security was considered across the application, storage, and infrastructure components.

Key security practices included:

- **Azure RBAC:** Assign permissions based on user responsibilities and the principle of least privilege.
- **Storage Account Security:** Protect storage account access keys and avoid exposing credentials in source code or documentation.
- **Identity-Based Authentication:** Prefer Microsoft Entra ID-based authentication where supported instead of sharing storage account keys.
- **Network Security:** Restrict RDP access to trusted source IP addresses.
- **Storage Network Access:** Configure appropriate storage firewall and network access rules.
- **Environment Separation:** Use separate resource groups to organise Development, Testing, and Production resources.

## 7. Testing and Validation

The following tests were performed to verify the implementation.

| Test Scenario | Expected Result | Status |
|---|---|---|
| Azure App Service | Website loads successfully | Passed |
| Static Website Endpoint | Website content loads successfully | Passed |
| `$web` Container | HTML, CSS, JavaScript, and images available | Passed |
| Azure Front Door | Static content served through delivery endpoint | Passed |
| Azure File Share | `teamfiles` created successfully | Passed |
| File Upload | Files visible in Azure File Share | Passed |
| Windows VM Access | VM accessible through RDP | Passed |
| SMB Connectivity | Azure File Share accessible from VM | Passed |
| VM to Storage | `vm-test.txt` visible in File Share | Passed |
| Storage to VM | `portal-test.txt` visible from VM | Passed |

## 8. Project Outcomes and Key Achievements

The project successfully demonstrated the deployment and integration of multiple Azure cloud services to support the Online application.

Key achievements include:

- **Cloud Application Hosting:** Deployed the website using Azure App Service.
- **Static Website Hosting:** Configured Azure Storage Static Website Hosting using the `$web` container.
- **Global Content Delivery:** Configured Azure Front Door to distribute static website content.
- **Cloud Storage Management:** Created an Azure Storage Account for website and shared file storage.
- **Team Collaboration:** Implemented Azure Files for centralised project file sharing.
- **VM-to-Storage Connectivity:** Established SMB connectivity between a Windows VM and Azure File Share.
- **Two-Way File Sharing:** Successfully verified file uploads from both the Windows VM and Azure Portal.
- **Traffic Management Design:** Incorporated Azure Traffic Manager and Azure DNS into the proposed global routing architecture.
- **Resource Governance:** Defined resource group separation, tagging, and RBAC strategies.
- **Cost Management:** Established a budget monitoring and expenditure control approach.

## 9. Key Skills Demonstrated

- Microsoft Azure Cloud Services
- Azure App Service Deployment
- Azure Storage Account Configuration
- Azure Static Website Hosting
- Azure Front Door Configuration
- Azure Traffic Manager
- Azure DNS Management
- Azure Files and File Shares
- Windows Virtual Machine Administration
- SMB Protocol and Network Drive Mapping
- Azure Resource Groups and Resource Organisation
- Azure Role-Based Access Control (RBAC)
- Azure Cost Management
- Cloud Infrastructure Deployment
- Network Connectivity and Troubleshooting
- Cloud Governance and Security

## 10. Conclusion

This project provided practical experience in designing and implementing a cloud-based web application solution using Microsoft Azure.

Azure App Service was used to host the Online website, while Azure Storage provided static website hosting and shared file storage. Azure Front Door was configured as the content delivery layer to improve the distribution of static website assets.

Azure Files was successfully integrated with a Windows Virtual Machine using SMB, and two-way file access was verified through practical testing.

The project also explored Azure Traffic Manager, Azure DNS, resource governance, RBAC, and Cost Management as components of a scalable production architecture.

Overall, the solution demonstrates practical cloud engineering skills across application hosting, storage management, content delivery, infrastructure connectivity, cloud governance, and cost optimisation.

---

**Technology Stack:** Microsoft Azure | Azure App Service | Azure Storage | Azure Front Door | Azure Traffic Manager | Azure DNS | Azure Files | Azure Virtual Machines | SMB | Azure RBAC | Azure Cost Management
