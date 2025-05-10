---
title: "AZ-900 Preparation: Key Azure Terms Explained"
collection: teaching
type: "Preparation"
permalink: /teaching/Prepare-for-AZ-900
venue: "10, may"
date: 2025-05-10
location: "Utrecht, Netherlands"
---

Preparing for the AZ-900: Microsoft Azure Fundamentals certification? This article breaks down the key terms and core concepts you’ll need to understand. Whether you're just starting with cloud computing or looking to strengthen your Azure knowledge, this simplified guide will help you.

# Terms per domain

## Benefits of cloud services

Agility
:   Cloud services help organizations react quickly to changes. If the market shifts, companies can adjust their tools and services fast. This flexibility supports innovation and faster decision-making without big delays.

Elasticity
:   Elasticity means a system can automatically grow or shrink based on what’s needed. If more users show up, the system adds resources. If fewer users are active, it reduces resources. This happens without manual work and helps keep performance high and costs low.

Availability
:   This shows how often a system is working and responding properly. It’s usually shown as a percentage over time—like 99.9% uptime.

Resilience
:   Resilience means a system can recover quickly after a major issue, like a regional outage. The system is built to stay running or bounce back fast in case of big problems.

Consumption-Based Pricing
:   You pay only for what you actually use. If you use fewer resources, you pay less. This is also called "pay-as-you-go" and helps save money when your usage goes up and down.

Disaster Recovery
:   This is about getting systems back up after a serious failure. It focuses on how fast recovery can happen and how much data (if any) might be lost during that time.

Economies of Scale
:   The more services or resources a cloud provider uses, the cheaper each one becomes. This lets providers offer lower prices to customers.

Capital Expenditure (CapEx)
:   CapEx means spending money upfront on physical equipment or software. A downside is that you can’t deduct this cost from taxes right away, and it’s a fixed investment whether you use it fully or not.

Azure Government
:   This is a special version of Azure for U.S. government agencies—federal, state, local, and tribal. It follows strict rules for privacy and security and uses a separate portal.

Governance
:   Governance in the cloud means having rules and controls to manage how cloud resources are used. It helps organizations stay secure, follow policies, and keep costs under control by deciding who can do what.

## Service lifecycle in Azure

Private preview mode
:   Some Azure features are tested in a "private preview" before they are released to everyone. Only selected users can try them out, often by invitation or approval from Microsoft. These features may not have full support or official service guarantees yet.

## Core Azure products

Azure Virtual Machine Scale Sets
:   This service helps you run and manage many virtual machines that are all the same. You can easily add or remove VMs based on how much computing power you need. With standard images, you can go up to 1,000 VMs; with custom ones, the limit is 600. It’s part of Azure’s IaaS (Infrastructure as a Service).

Site-to-Site VPN
:   To link your company network with Azure, you need a VPN Gateway. This creates a secure, encrypted connection between your on-premises network and Azure using IPsec. It often involves a physical device at your site, and a virtual gateway in Azure.

Content Delivery Network (CDN)
:   A CDN stores copies of static files (like images and videos) on many servers around the world. When someone accesses your site, they get the content from the server closest to them. This makes websites load faster and reduces the load on the main server.

Azure DevOps
:   Azure DevOps includes tools like Pipelines, which automate how code moves from development to deployment. It helps teams build, test, and release software faster and more reliably.

Azure Load Balancer
:   This tool spreads incoming traffic across multiple virtual machines so that no single VM is overloaded. It helps keep apps running smoothly and evenly handles requests.

Azure Functions
:   Functions are small pieces of code that run only when needed. They’re perfect for simple tasks that start, do their job, and stop quickly—without running all the time.

## Core Azure components

Azure Availability Zones
:   These are separate datacenters within an Azure region, each with its own power, cooling, and networking. By using more than one zone, you can protect your apps from data center outages. They’re not the same as regions or resource groups.

Azure Resource Manager (ARM)
:   ARM is the service that lets you manage all your Azure resources. You can create, update, and delete resources using tools like the portal, PowerShell, CLI, or APIs—all in a consistent way.

Azure Region
:   Azure has over 60 regions across more than 10 global areas. Each region contains one or more datacenters. You choose a region to place your resources based on location, compliance, or performance.

## Core Azure solutions

Azure Cognitive Services (AI Services)
:   These are ready-to-use AI tools that let you add things like image recognition, speech-to-text, language translation, and natural-sounding text-to-speech to your apps. You use them through easy-to-call APIs.

Serverless Model
:   In serverless computing, you don’t need to manage servers. Azure handles all the setup, scaling, and maintenance. You just focus on writing the code. Examples include Azure Functions, Logic Apps, and Service Fabric.

Azure SQL Database
:   This is a fully managed database service that works like SQL Server. You don’t need to install or manage anything—it’s Database-as-a-Service (DBaaS).

Azure HDInsight
:   HDInsight is a cloud version of the Apache Hadoop big data tools. It helps process large datasets using popular open-source systems like Hive, Spark, and HBase. It also supports coding in R, Python, Scala, and .NET.

Azure Marketplace
:   The Marketplace is where you can find and rent thousands of pre-made apps and services that run in Azure. It includes tools from both Microsoft and other vendors.

Azure Cosmos DB
:   Cosmos DB is a super-fast, globally distributed database. It’s great for apps that need low-latency storage for lots of small pieces of data.

Azure Blob Storage
:   Blob Storage is used to store large amounts of unstructured data like text, images, or videos. It’s ideal for storing any file that doesn’t follow a fixed format.

Azure File Storage
:   This provides shared file storage in the cloud. Multiple virtual machines can access the same files, just like a network drive. It’s best for traditional file shares.

Azure Table Storage
:   Table Storage is used for structured NoSQL data, like key-value pairs. It’s best for fast access to organized data—not for storing files or binary data.

Azure Queue Storage
:   Queue Storage holds messages that can be passed between apps or services. It’s made for communication between components, not for storing files or raw data.

## Azure management tools

Network Security Group (NSG)
:   An NSG lets you control traffic going in and out of your Azure resources. You set rules that allow or block certain sources, destinations, and ports—helping secure your virtual network.

Azure Advisor
:   Azure Advisor looks at how you use Azure and gives personalized tips to save money, improve performance, and increase security.

Azure Portal
:   This is the main website where you manage your Azure services and resources through a visual interface.

Azure PowerShell / Azure CLI
:   These command-line tools let you manage Azure through scripts. They’re great for automating tasks instead of doing them manually in the portal. With Azure CLI, you log in using az login.

Azure Cloud Shell
:   Cloud Shell is a built-in terminal in the Azure Portal. It gives you access to PowerShell or Bash without needing to install anything on your own computer.

Azure Sovereign Regions
:   These special regions are made to meet local laws about data location and privacy. For example, Germany and China have their own Azure setups with extra rules.

## Monitoring and reporting

Log Analytics Workspace
:   This is where your monitoring data like logs and metrics are stored. It’s needed to collect and analyze Azure monitoring information.

Azure Monitor
:   Azure Monitor is a dashboard that gathers data like logs, alerts, and performance metrics from your services—so you can track what’s going on across your Azure environment.

Azure Monitor Account
:   This is the account that collects data from your services and connects to things like the Log Analytics Workspace and alerts.

Azure Service Health
:   It informs you about outages or issues affecting Azure services in your region. It includes Azure Status (global), Service Health (for your services), and Resource Health (for individual resources).

Resource Health
:   Each VM and service shows its own health info—like whether there’s a problem caused by Azure or a local configuration issue.

## Azure Identity services

Multi-Factor Authentication (MFA)
:   MFA adds another layer to logging in—beyond just a password. For example, it might send a code to your phone. This makes it much harder for attackers to get in.

Microsoft Entra ID
:   This is Microsoft’s cloud identity service (formerly Azure AD). It controls who can access apps and data, and supports features like MFA and single sign-on.

Single Sign-On (SSO)
:   SSO means you can log into all your company’s apps with one username and password—no need to remember different logins for each app.

Azure Tenant
:   An Azure Tenant is your organization’s private space in Microsoft Entra ID. It’s automatically created when you sign up for a Microsoft cloud service.

## IaaS PaaS and SaaS

Software as a Service (SaaS)
:   With SaaS, the provider manages everything—from hardware to the app itself. You just use the app through your browser or device without managing updates or servers.

Infrastructure as a Service (IaaS)
:   IaaS gives you access to virtual hardware (like servers and storage), but you manage the operating system and software. Azure takes care of the physical stuff.

## Azure SLAs

Azure Service Level Agreements (SLAs)
:   Each Azure service has its own SLA, which describes the expected uptime or performance. If Azure doesn’t meet this, you may get a small refund—usually 10% or 25% of your monthly bill.

Azure Service Lifecycle Phases
:   Azure services go through 3 phases: Private Preview – Limited access, by invite only. Public Preview – Anyone can try it, but it’s not fully supported. General Availability (GA) – Fully supported and ready for production use.

Azure Reserved Instances
:   You can save over 40% on VM costs by committing to use them for 1 or 3 years instead of paying as you go.

## Security tools and features

Microsoft Defender for Cloud
:   This is Azure’s built-in security center. It helps you find and fix security issues and shows threat alerts in a central dashboard.

Bastion
:   Azure Bastion lets you securely access virtual machines directly from your browser—without needing to expose them to the internet.

Shared Security Model
:   Security in Azure is a shared responsibility. Microsoft protects the platform, and you are responsible for your own data, users, and apps—depending on the service type (IaaS, PaaS, SaaS).

## Privacy and compliance

Microsoft Purview
:   Purview helps you manage and protect your data across different places—on-premises, in other clouds, and in SaaS apps. It helps you find, organize, and secure sensitive data.

Trust Center
:   This is where you can find a list of standards that Azure follows, like security certifications and compliance rules.

Purview Compliance Manager
:   A tool that helps track how well your company follows data protection laws and industry rules.

ISO
:   An international organization that sets standards—such as for security, quality, and privacy—that companies and cloud providers can follow.

Microsoft Service Trust Portal
:   A website with documents like security assessments, pen test results, and compliance certificates that show Microsoft’s efforts to stay secure and compliant.

## Azure governance methodologies

Azure Policy
:   Azure Policy lets you create rules to control what users can do, like limiting VM types or requiring tags. If something breaks the policy, it won’t run unless you remove or change the policy.

Policy Initiative
:   This lets you group several policies into one package, making it easier to apply them together.

Public, Private and Hybrid cloud

Public Cloud
:   Cloud services shared with the public, like Azure, AWS, or GCP. In contrast, private clouds are for a single organization and offer more control.

Hybrid Cloud
:   This setup mixes on-premises or private cloud systems with public cloud services. You can keep sensitive data private while using public cloud for extra power or storage.

Paired Regions
:   Azure links regions in pairs for backup and disaster recovery. If maintenance happens, it's done one region at a time to keep services running.

## Azure subscriptions

You can have multiple subscriptions, as a way to separate out resources between billing units, business groups, or for any reason you wish. There is not a limit to the number of subscriptions a single user can be included on.

Management Groups
:   These help you organize and apply policies across many Azure subscriptions. Rules set at the top level apply to everything below.

## Secure Azure Networking

Azure DDoS Protection
:   Protects your resources from massive attacks that flood your network or apps with traffic. It works well for network and transport layers, but for app-level attacks like SQL injection, use a Web Application Firewall (WAF).

DDoS Attack
:   A DDoS attack sends too much traffic to a service, making it slow or unreachable for real users.

Web Application Firewall (WAF)
:   WAF protects apps from harmful web traffic like cross-site scripting or SQL injection. It sits in front of your app and filters out bad requests.

## Azure costs

Pay-As-You-Go
:   You only pay for what you use. There’s no long-term contract, and pricing is flexible by the second.

Reserved Instances
:   You commit to a VM type for 1 or 3 years in exchange for a cheaper price. Great for predictable workloads.

Spot Pricing
:   You bid for unused Azure capacity. It’s cheap but risky—your resources can be taken back if someone bids higher.

Enterprise Agreement
:   A volume licensing plan for big companies. Offers discounts and extras, but less flexibility than pay-as-you-go.

Free Tier
:   Try Azure for free with limited services for up to 12 months. For example, 750 free VM hours with the B1S size.

Spending Limit
:   You can set a spending cap in the Azure account to avoid going over your budget.

## Describe Azure identity, access, and security

Azure B2B vs. B2C
:   B2B: Share apps with business partners. B2C: Let individual customers sign up and log in to your apps.

Microsoft Entra Domain Services
:   A cloud version of domain services. No need to manage domain controllers—Azure handles it.

Conditional Access Policy
:   You can block or allow access based on things like user role or device compliance. But you can’t block access just based on device type.

## Describe Azure compute and networking services

App Service Environment
:   Provides a private, isolated space for your Azure web apps. Resources aren’t shared with others.

Azure ExpressRoute
:   A dedicated, fast connection between your on-prem network and Azure. It’s more stable and faster than a VPN.

Azure Peering
:   Links two virtual networks in Azure securely. They can talk to each other without going through the internet.

Update Domains
:   Helps make sure not all VMs go down during updates. Azure updates groups of VMs one at a time to keep services running.

## Describe cloud service types

Lift-and-Shift Migration
:   You move apps to the cloud without changing them. This fits best with IaaS since you’re just copying the setup into Azure.

## Describe the benefits of using cloud services

Azure Reliability Strategy
:   Availability: Keep services up and running. Fault Tolerance: Keep systems working even if something breaks. Redundancy: Use backup systems to avoid downtime.

## Describe Azure storage services

AzCopy
:   A command-line tool for quickly copying data to and from Azure Storage.

Archive Tier
:   For data you rarely need. Cheap to store but slow to access—don’t use for data you need within 30 days.

Hot Tier
:   Best for data that’s accessed often. Fast but more expensive.

Cool Tier
:   For infrequently used data stored for at least 30 days. Cheaper than Hot Tier.

Premium Storage
:   High-speed, low-latency storage for critical apps.

Azure Storage Explorer
:   A tool with a graphical interface to manage Azure Storage accounts—like browsing files.

Azure File Sync
:   Keeps files on your local Windows server in sync with Azure Files. Great for hybrid setups.

LRS (Locally Redundant Storage)
:   Stores 3 copies of your data in the same datacenter to guard against hardware failure.

ZRS (Zone-Redundant Storage)
:   Stores data in multiple datacenters in the same region for extra protection.

GRS (Geo-Redundant Storage)
:   Copies your data to another region far away for disaster recovery.

RA-GRS (Read-Access Geo-Redundant Storage)
:   Same as GRS, but also lets you read from the backup location if the main one goes down.

Azure Disks
:   Block storage for VMs—good for OS disks and application data.

Premium SSDs
:   High performance, great for most business workloads.

Standard SSDs
:   Balanced option for general-purpose use.

Standard HDDs
:   Budget-friendly storage for less important or infrequent tasks.

Ultra Disks
:   Top performance for demanding apps like databases with heavy I/O.

Azure Data Box
:   A physical box you order from Microsoft to transfer large amounts of data to or from Azure.

Azure Storage Account
:   An account that lets you store data like files, blobs, tables, and queues. It can hold up to 5 Petabytes.