Deploying an Online Movie Watching Application on Cloud

1. Introduction -
An online entertainment provider that hosts a website for users to browse and watch online movie content. As the number of users increased, the company experienced performance issues due to global traffic being served from a single location. Static website content such as HTML pages, CSS files, JavaScript files and images also contributed to increased page-loading time.
To address these requirements, an Azure cloud-based solution was designed and implemented. Azure Platform as a Service (PaaS) and storage services were used to host the website, distribute static content, provide shared file storage and connect a virtual machine to the storage service.
The solution uses Azure App Service for hosting the web application, Azure Storage for static and shared files, Azure Front Door as the current Azure content delivery/CDN service, Azure Traffic Manager for DNS-level traffic routing, Azure VM for infrastructure connectivity testing, and Azure DNS for DNS management.
________________________________________
2. Proposed Cloud Solution -
The proposed solution uses Microsoft Azure to provide a scalable and reliable hosting environment for Client.
The major Azure services used in the solution are:
Azure Service	Purpose
Azure App Service	Hosts the online movie website/application
Azure Storage Account	Stores static website content and shared team files
Azure Front Door/CDN	Provides global delivery and caching of static content
Azure Traffic Manager	Provides DNS-level traffic routing between application endpoints
Azure DNS	Provides DNS hosting and domain name resolution
Azure Virtual Machine	Provides a Windows/Linux compute environment and connects to Azure Storage
Azure Files	Provides shared file storage for project/team files

For the implementation, the application and storage services were deployed according to the resources available in the environment. A production implementation can extend the design by deploying application instances in multiple Azure regions and placing them behind Azure Traffic Manager.
________________________________________
3. Requirement 1 – Appropriate Cloud Solution -
Objective -
The company requires a cloud solution capable of handling global traffic, improving website loading performance and distributing traffic at the DNS level.
Solution -
Azure was selected as the cloud platform. Azure App Service is used to host the website without requiring management of the underlying operating system or web server.
Azure Storage is used to store static website content such as:
•	HTML files
•	CSS files
•	JavaScript files
•	Images
•	Other static assets
Azure Front Door/CDN is configured to distribute and cache static content closer to users. This can help reduce latency when users access static website resources.
Azure Traffic Manager is used for DNS-level traffic routing. In a production implementation, application endpoints deployed in different Azure regions can be registered with Traffic Manager. Traffic Manager can then route users to an appropriate healthy endpoint according to the configured routing method.
Azure DNS can be used to host the company's DNS zone and provide DNS records required to access the application.
The solution therefore separates the application hosting, content delivery, DNS traffic management and storage responsibilities across appropriate Azure services.
________________________________________
4. Requirement 2(a) – Governance of Development, Testing and Production Resources -
Objective -
The company needs an approach to govern resources used for development, testing and production environments.
Resource Group Strategy -
Separate resource groups can be created for each environment:
rg-bingewatch-dev
rg-bingewatch-test
rg-bingewatch-prod
This provides logical separation between environments and makes it easier to manage, monitor and delete resources.
For example:
Development -
rg-bingewatch-dev
    |
    +-- App Service
    +-- Storage
    +-- VM
    +-- CDN/Front Door
Testing -
rg-bingewatch-test
    |
    +-- App Service
    +-- Storage
    +-- VM
    +-- CDN/Front Door
Production -
rg-bingewatch-prod
    |
    +-- App Service
    +-- Storage
    +-- Traffic Manager
    +-- DNS
    +-- CDN/Front Door
Resource Tagging -
Azure tags can be used to identify ownership, environment and cost allocation.
Recommended tags include:
Tag	Example -
Project	BingeWatch
Environment	Dev/Test/Prod
Owner	BingeWatch Team
Department	Entertainment
CostCenter	BW001
For example:
Project = BingeWatch
Environment = Dev
Owner = BingeWatch Team
CostCenter = BW001
Tags allow administrators to identify resources and analyze costs by project or environment.
Role-Based Access Control
Role-Based Access Control can be used to provide users with only the permissions required to perform their tasks.
For example:
•	Administrators can manage resources.
•	Developers can manage application resources.
•	Testers can access testing resources.
•	Users who only need to view resources can receive Reader access.
The principle of least privilege should be followed so that users are not given unnecessary permissions.
________________________________________
5. Requirement 2(b) – Billing and Cost Management -
Azure Cost Management can be used to monitor and control the cost of resources used by Client.
The Azure portal provides Cost Management + Billing features that allow administrators to:
•	View current expenditure
•	Analyze resource costs
•	Monitor spending trends
•	Create budgets
•	Configure cost alerts
•	Identify resources contributing to expenditure
A budget can be configured for the project with alerts at predefined thresholds.

For example:
Budget: Based on approved project amount

Alert thresholds:
50%  → Monitoring alert
75%  → Warning
90%  → Critical warning
100% → Budget reached
Cost analysis can also be filtered using resource groups, services, subscriptions and tags.
This allows the organization to maintain a separate track of the billing lifecycle and identify which environment or service is contributing to cloud expenditure.
Unused resources should also be removed after testing to prevent unnecessary charges.
________________________________________
6. Requirement 3 – Upload Static Website Content to Cloud -
Objective -
The static content of the Client website must be uploaded to the cloud.
A static website was created containing HTML, CSS, JavaScript and image files.
The website structure was:
BingeWatchWebsite
│
├── index.html
├── about.html
│
├── css
│   └── style.css
│
├── js
│   └── script.js
│
└── images
    ├── movie1.jpg
    ├── movie2.jpg
    └── movie3.jpg
Azure App Service
An Azure App Service was created to host the sample website.
The deployed App Service was:
app-bwo-cus
app-bwo-isc
The website files were uploaded to the App Service wwwroot directory.
The resulting structure was:
/wwwroot
│
├── index.html
├── about.html
├── css
├── js
└── images
The application was then accessed through the Azure App Service URL and verified successfully.
This demonstrated that the Client website was successfully deployed to Azure App Service.
________________________________________
7. Static Website Storage -
An Azure Storage Account named:
bwostaticglobal
was created to store static website content.
The Static Website feature was enabled on the storage account.
The $web container was used to store the website files:
$web
│
├── index.html
├── about.html
├── css/
├── js/
└── images/
The static website endpoint was tested and confirmed to serve the website successfully.
The $web container is specifically used for website content and is kept separate from the team file-sharing requirement.
________________________________________
8. Requirement 4 – Create CDN Endpoint and Serve Static Files
Objective
The company wants static content to be delivered efficiently to users from locations closer to them.
Azure Front Door was configured as the content delivery/CDN layer for the static website.
The configured profile was:
Profile: bwocdn
Pricing Tier: Azure Front Door Standard
The endpoint created for the content delivery service uses the Azure Front Door endpoint domain.
Origin Configuration
The static website hosted in Azure Storage was configured as the origin.
The origin configuration used:
Origin type:
Storage (Static website)

Origin host:
bwostaticglobal.z6.web.core.windows.net

Origin host header:
bwostaticglobal.z6.web.core.windows.net

HTTP:
80

HTTPS:
443
The origin group was:
default-origin-group
The route was configured to forward requests for the website to the configured origin.
The static website content stored in the $web container is therefore available through the Front Door endpoint.
________________________________________
9. Requirement 5 – Use Storage Service to Share Files with Teammates -
Objective -
A shared storage location is required so that members of the Client project team can upload and access project files.
Azure Files was selected for this requirement.
A File Share named:
teamfiles
was created inside the storage account:
bwostaticglobal
The File Share was used to store project and team files.
Example files uploaded were:
team-notes.txt
movie1.jpg
movie2.jpg
requirements.txt
The structure was:
bwostaticglobal
│
└── teamfiles
    ├── team-notes.txt    
    ├── movie1.jpg
    ├── movie2.jpg
    └── requirements.txt
The files were uploaded using the Azure Portal and verified from the File Share.
Azure Files provides a centralized shared location that can be accessed by authorized team members and connected to virtual machines.
________________________________________
10. Requirement 6 – Connect Windows VM to Azure Storage -
Objective -
The Azure File Share created in Requirement 5 must be connected to a Windows or Linux virtual machine.
A Windows Azure Virtual Machine was used for this implementation.
The VM was created in Azure and accessed using Remote Desktop Protocol (RDP).
The Windows VM was then connected to the Azure File Share using the SMB protocol.
Azure File Share Path -
The Azure File Share was accessed using the following UNC path:
\\bwostaticglobal.file.core.windows.net\teamfiles
The File Share was mapped to the Windows VM as a network drive.
For example: Z:
The resulting structure on the Windows VM was:
This PC
│
├── Windows (C:)
│
└── teamfiles (Z:)
      │
      ├── teammates.txt  
      ├── movie1.jpg
      ├── movie2.jpg
     
Connectivity Verification -
The connection was verified by opening the mapped Z: drive from the Windows VM and viewing the files stored in the Azure File Share.
A test file was then created from the Windows VM:
vm-test.txt
The file was created inside the mapped network drive.
After refreshing the Azure Portal, the vm-test.txt file was visible inside the teamfiles File Share.
This confirmed connectivity in the following direction:
Windows VM
    |
    | SMB
    v
Azure File Share
A reverse test was also performed by uploading a file from the Azure Portal:
portal-test.txt
The file was then verified from the Windows VM through the mapped Z: drive.
Therefore, two-way access was successfully demonstrated:
             Azure Storage
             File Share
             "teamfiles"
                  ↕
              SMB / 445
                  ↕
             Windows VM
             "vm-bingewatch"
                  |
                  v
             Z: Network Drive
________________________________________
11. Security Considerations -
Security was considered while designing the solution.
Role-Based Access Control can be used to control administrative access to resources. Users should be assigned only the permissions required for their responsibilities.
Storage access keys should be protected and should not be included in source code, screenshots or project reports.
For a production implementation, identity-based authentication should be preferred where possible instead of sharing storage account keys.
Network access to the Windows VM should also be restricted. RDP access should preferably be limited to trusted source IP addresses instead of exposing port 3389 to the entire internet.
The Azure Storage account should also use appropriate firewall and network access rules.
________________________________________
12. Testing and Validation -
The following tests were performed to validate the implementation:
Test	Expected Result	Status
App Service website access	Website loads successfully	Passed
Static website endpoint	Website content loads	Passed
$web content	HTML/CSS/JS/images available	Passed
Front Door endpoint	Static content served through CDN layer	Passed
Azure File Share	teamfiles created successfully	Passed
File upload	Files visible in File Share	Passed
Windows VM connectivity	VM accessible through RDP	Passed
SMB connectivity	Azure File Share accessible from VM	Passed
VM → Storage	vm-test.txt visible in File Share	Passed
Storage → VM	portal-test.txt visible on VM	Passed
________________________________________
13. Conclusion -
The Client website was successfully designed and deployed using Microsoft Azure cloud services.
Azure App Service was used to host the web application, while Azure Storage was used for static website content and shared team files. Azure Front Door was configured as the content delivery layer to improve the distribution of static assets. Azure Traffic Manager and Azure DNS provide the components required for DNS-level traffic management and global application routing in the proposed production architecture.
Azure Files was implemented to provide centralized file sharing for the project team. A Windows Azure Virtual Machine was successfully connected to the File Share using SMB, and two-way file access was verified.
Resource Groups, tags, Role-Based Access Control and Cost Management provide an approach for governing resources, controlling access and monitoring cloud expenditure across development, testing and production environments.
Overall, the proposed architecture provides a scalable cloud-based foundation for the Client application and addresses the requirements related to website hosting, static content delivery, global traffic management, shared storage, VM connectivity, governance and cost management.

