---
title: "AWS Certified Cloud Practitioner Preparation: Key AWS Terms Explained"
collection: teaching
type: "Preparation"
permalink: /teaching/Prepare-for-AWS-CCP
venue: "25, june"
date: 2025-06-25
location: "Utrecht, Netherlands"
---

Preparing for the AWS Certified Cloud Practitioner exam: This article breaks down the key terms and core concepts you’ll need to understand. Whether you're just starting with cloud computing or looking to strengthen your AWS knowledge, this simplified guide will help you.

AWS Certified Cloud Practitioner
======
A road to the certification

# Handy links

**Certification page**: [AWS Certified Cloud Practitioner Certification AWS Certification AWS](https://aws.amazon.com/certification/certified-cloud-practitioner/)

**AWS Skillbuilder page**: [Prepare for AWS Certified Cloud Practitioner](https://skillbuilder.aws/exam-prep/cloud-practitioner)

**Degreed pathway**: [AWS Certified Cloud Practitioner (CLF-C02) Pathway Degreed](https://degreed.com/pathway/49e67k7195/pathway?newWindow=true)

**Plurasight pathway**: [AWS Foundations Path Pluralsight](https://app.pluralsight.com/paths/skill/aws-foundations)

**Learn path:**

[\[RETIRING\] Exam Prep Standard Course: AWS Certified Cloud Practitioner (CLF-C02 - English)](https://explore.skillbuilder.aws/learn/courses/16434/retiring-exam-prep-standard-course-aws-certified-cloud-practitioner-clf-c02-english/lessons)

[AWS Certified Cloud Practitioner - Foundational (Partner) - AWS Skill Builder](https://explore.skillbuilder.aws/learn/learning-plans/879/AWS-Cloud-Practitioner-Certification-Partner-Learning-Plan)

# Overview exam

**Format**: Multiple choice and multiple response.

**Type**: Foundational

**Number of questions**: 65 (50 questions that affect your score and 15 unscored questions for research future use).

**Score**: 100-1000. The minimum passing score is 700.

**Time**: 90 minutes

**Cost**: 100 USD (88,80 EUR)

**Schedule exam when you are ready:** <https://www.aws.training/certification/?ch=tile&tile=getstarted>

**If you take the exam online**: <https://www.pearsonvue.com/us/en/aws/onvue.html>  

**For the online exam**: Use a personal computer, audio and video, using an ethernet cable or capable Wi-Fi. Clear your desk and only use 1 screen. Plugin your laptop before the check in process. Be in a private room, no one else can be nearby. Don’t read questions out loud. Don’t leave the camera. Remove your (smart)watch. No earbuds, books, notepads, pens, whiteboards and food. A beverage is allowed. All personal items must be stored out of sight. No scratch paper to use notes. Check-in half an hour before the exam. The check-in process should take around 15 minutes. Be sure to close all applications accept OnVue.

**If you fail an exam**: wait 14 days before you are eligible to retake the exam. There is no limit on exam attempts. You must pat the full registration fee for each exam attempt.

**Validation**: AWS certifications are valid for three years.  
Option 1: Complete the new AWS Cloud Quest: Recertify Cloud Practitioner game-based training. No exam or prep required. (free during the beta period, November 21, 2023 through the end of July 2025.)  
Option 2: Pass the latest version of the AWS Certified Cloud Practitioner exam.  
Option 3: Pass any one of the Associate-level exams.  
Option 4: Pass any one of the Professional-level exams. (Options 2, 3, and 4 can be taken using the 50% discount voucher in your AWS Certification Account.)

# Terms per domain

# Domain 1: Coud Concepts (24%)

## 1.1 Define benefits of AWS Cloud Concepts

- **High availability**
  - **Availability Zones**
  - **Regions**
  - **Edge Locations**
- **Elasticity**:
    The ability to activate resources as you need them and return resources when you no longer need them.
- **Agility**  
- **Disaster recovery strategies**
  - Warm Standby strategy
  - Pilot Light strategy
  - Multi-site active-active strategy
  - Backup & Restore strategy

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175457.png'>

- **Vertical scaling**
- **Horizontal scaling**
  - **Sticky sessions**
- **Amazon EC2**:
    Amazon EC2 (Elastic Compute Cloud) is a service provided by Amazon Web Services (AWS) that offers scalable computing capacity in the cloud.
- **AWS Outposts**:
    AWS Outposts is a fully managed service that offers the same AWS infrastructure, AWS services, APIs, and tools to virtually any data center, co-location space, or on-premises facility for a truly consistent hybrid experience. AWS Outposts is ideal for workloads that require low latency access to on-premises systems, local data processing, data residency, and migration of applications with local system interdependencies.

## 1.2 Identity design principles of the AWS Cloud

- **AWS Well-Architected Framework (tool)**  
    AWS best practices. Based on 6 principles: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimalization, Sustainability. Compare your infrastructure with the best AWS practices design.

## 1.3 Understand the benefits of and strategies of the AWS Cloud

- **AWS Cloud Adoption Framework**  
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175514.png'>

- **Migration strategies (7Rs)**  
    The 7Rs are seven migration strategies to the cloud: Retire (applications to retire), Retain (applications to keep in the source env or not ready to migrate), Rehost (lift-and-shift applications to the cloud), Relocate (servers with 1 or more applications), Repurchase (drop-and-shop for applications with a different version or product), Replatform (lift-tinker-and-shift application need some lever of optimalisation) and Refactor or re-architect (migrate applications and take full advantage of AWS features).
- **AWS Snowball edge**
    Move large amount of data in or out to AWS
- **AWS Database Migration Service (AWS DMS)**:
    Database replication
- **Amazon S3 (Simple Storage Service)**:
    Not the best choice for low latency.
  - **S3 Glacier**:
        Lowest cost s3 storage class allowing you to archive large amount of data at low cost.
- **Amazon RDS (Relational Database Service)**:
    A manage database service, scalable relational database with fast performance, high availability and security.
- **Amazon EFS (Elastic File System)**:
    Provides scalable file storage with amazon EC2 instances and the storage capacity is elastic too. EFS file system can be mounted on instances across multiple Availability Zones (AZ)
- **Amazon dynamo DB**:
    A fully managed SQL database service that provides fast and predictable performance with its scalability. Key value and document database and stores JSON type documents.
- **Amazon Aurora**:
    Amazon Aurora is een volledig beheerde relationele database-engine die compatibel is met MySQL en PostgreSQL.
- **Amazon Machine Images (AMI)**:
    Amazon Machine Images  

## 1.4 Understand concepts of cloud economics

- Total cost of ownership (TCO)
  - Operational expenses
  - Capital expenses (CapEx)
  - Labor costs
  - Software licensing costs

# Domain 2: Security and Compliance (30%)

## 2.1 Understand the AWS Shared Responsibility Model

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175526.png'>

## 2.2 Understand AWS Cloud Security, governance, and compliance concepts

- **AWS Artifact**  
    Where you can find security and compliance information and reports about services.
- **AWS Security Hub**  
    Biedt een uitgebreide weergave van je beveiligingsstatus in AWS en helpt je om je omgeving te beoordelen aan de hand van beveiligingsnormen en best practices. Het verzamelt beveiligingsgegevens van verschillende AWS-accounts, services en ondersteunde producten van derden, en helpt je om beveiligingstrends te analyseren en prioritaire beveiligingsproblemen te identificeren.
- **AWS Inspector**  
    Is een kwetsbaarheidsbeheerservice die automatisch workloads ontdekt en continu scant op softwarekwetsbaarheden en onbedoelde netwerkblootstelling. Het genereert gedetailleerde rapporten over gevonden problemen en biedt aanbevelingen voor herstel.
- **Penetration Testing**  
    AWS customers can carry out security assessments or penetration tests against their AWS infrastructure without prior approval for few common AWS services. Customers are not permitted to conduct any security assessments of AWS infrastructure, or the AWS services themselves.
- **AWS Secrets Manager**  
    Helpt je bij het beheren, ophalen en roteren van database- en applicatie-inloggegevens, OAuth-tokens, API-sleutels en andere geheimen. Het elimineert de noodzaak voor hardcoded credentials in applicatiebroncode door deze dynamisch op te halen wanneer nodig.
- **AWS CloudHSM**  
    AWS CloudHSM is a cloud-based Hardware Security Module (HSM) that enables you to easily generate and use your encryption keys on the AWS Cloud. With CloudHSM, you can manage your encryption keys using FIPS 140-2 Level 3 validated HSMs. It is a fully-managed service that automates time-consuming administrative tasks for you, such as hardware provisioning, software patching, high-availability, and backups.
- **Customer Managed Key (CMK)**  
    An AWS KMS key is a logical representation of a cryptographic key. A KMS key contains metadata, such as the key ID, key spec, key usage, creation date, description, and key state. Most importantly, it contains a reference to the key material that is used when you perform cryptographic operations with the KMS key.
- **Amazon GuardDuty**  
    Is een dreigingsdetectieservice die continu AWS-gegevensbronnen en logs monitort, analyseert en verwerkt om verdachte en mogelijk kwaadaardige activiteiten te identificeren. Het gebruikt dreigingsinformatie en machine learning om beveiligingsproblemen te detecteren.
- **Amazon Macie**  
    Is een databeveiligingsservice die gevoelige gegevens ontdekt, monitort en beschermt door middel van machine learning en patroonherkenning. Het biedt inzicht in de beveiligingsrisico's van je Amazon S3-gegevens en genereert meldingen voor potentiële problemen.
- **AWS Shield**  
    Biedt automatische en aanpasbare bescherming tegen DDoS-aanvallen voor AWS-toepassingen en API's. Het detecteert en mitigeert geavanceerde netwerk- en applicatielaag DDoS-aanvallen.
  - **AWS Shield Advanced**  
        Biedt uitgebreide bescherming tegen DDoS-aanvallen, inclusief geavanceerde detectie en mitigatie, integratie met AWS WAF, en toegang tot het AWS Shield Response Team.
- **Amazon CloudWatch**  
    Is een monitoring- en beheerservice die gegevens en bruikbare inzichten biedt voor AWS, on-premises, hybride en andere cloudtoepassingen en infrastructuurbronnen.
- **AWS CloudTrial**  
    Helpt bij het auditen, beheren en naleven van je AWS-account door gebruikersactiviteiten en API-aanroepen vast te leggen als gebeurtenissen.
- **AWS Audit Manager**  
    Helpt je bij het continu auditen van je AWS-gebruik om risicobeheer en naleving te vereenvoudigen.
- **AWS Config**  
    AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations.

## 2.3 Identity AWS access management capabilities

- **AWS-account**  
    AWS-account is een unieke identiteit die je toegang geeft tot Amazon Web Services (AWS). Een AWS-account bevat je gebruikersinformatie en biedt toegang tot AWS-services en -resources. Het account wordt gebruikt voor het bijhouden van je gebruik en kosten van AWS-services. Je kunt meerdere gebruikers en rollen aanmaken binnen je account om toegang en permissies te beheren.
- **Root user**  
    Een **AWS root user** is de oorspronkelijke gebruikersidentiteit die wordt gecreëerd wanneer je een AWS-account aanmaakt. Deze gebruiker heeft volledige toegang tot alle AWS-services en -resources binnen dat account. Het is sterk aanbevolen om Multi-Factor Authentication (MFA) in te schakelen voor de root user om de beveiliging te verbeteren. Gebruik de root user alleen voor taken die root-level permissies vereisen, zoals het beheren van factureringsinformatie en het herstellen van accounttoegang. Maak IAM-gebruikers en -rollen aan voor dagelijkse taken om de root user te beschermen.
- **AWS IAM**  
    Is een webservice die je helpt bij het beheren van toegang tot AWS-resources. Met IAM kun je gebruikers en groepen maken, rollen toewijzen en beleid instellen om de toegang tot je AWS-account te beheren.
  - **arn:aws:iam::{account id}:user/arntestuser**
  - **arn=** Amazon Resource Name
  - **aws=** AWS Regions
  - **iam=** Identifies the namespace it is associated with
  - **account id=** Account number identifier
  - **user=** Resource type, eg user or group
  - **arntestuser=** The resource identifier
  - **Users**  
        Zijn individuele identiteiten die je aanmaakt binnen je AWS-account. Elke gebruiker heeft unieke inloggegevens en kan specifieke permissies krijgen om AWS-resources te gebruiken.
  - **Groups**  
        Zijn verzamelingen van IAM-gebruikers. Door gebruikers in groepen te plaatsen, kun je eenvoudig permissies beheren voor meerdere gebruikers tegelijk.
  - **Roles**  
        Zijn identiteiten binnen je AWS-account die specifieke permissies hebben, maar in tegenstelling tot gebruikers, zijn ze niet gekoppeld aan een specifieke persoon. Rollen worden vaak gebruikt om toegang te verlenen aan AWS-services of externe identiteiten. Bijvoorbeeld, een EC2-instance kan een rol aannemen om toegang te krijgen tot S3-buckets zonder dat er lange termijn credentials nodig zijn.
  - **Policies**  
        Zijn documenten die permissies definiëren en kunnen worden gekoppeld aan gebruikers, groepen of rollen. Policies bepalen welke acties een identiteit kan uitvoeren op welke resources. Ze worden meestal geschreven in JSON-formaat en kunnen zowel door AWS beheerde policies als door de klant beheerde policies zijn.
    - **Managed**  
            Policies die door AWS worden gemaakt en beheerd. Ze zijn ontworpen om veelvoorkomende gebruiksscenario's te ondersteunen, zodat je snel permissies kunt toewijzen aan gebruikers, groepen en rollen
    - **Unmanaged**  
            Policies die je zelf maakt en beheert. Ze bieden meer flexibiliteit en controle, omdat je ze kunt aanpassen aan de specifieke behoeften van je organisatie.
  - **Integration with other AWS services**
- **IAM Policy Simulator**  
    Een tool waarmee je IAM-policies kunt testen en troubleshooten.
- **AWS Acceptable Use Policy**  
    The Acceptable Use Policy describes prohibited uses of the web services offered by Amazon Web Services, Inc. and its affiliates (the “Services”).
- **Amazon Cognito**  
    Biedt gebruikersauthenticatie, autorisatie en gebruikersbeheer voor je web- en mobiele apps. Het ondersteunt zowel gebruikerspools voor authenticatie als identiteitspools voor het verstrekken van tijdelijke AWS-credentials.
- **MFA Delete**  
    Een beveiligingsfunctie voor Amazon S3-buckets die Multi-Factor Authentication (MFA) vereist voor bepaalde acties.

## 2.4 Identity components and resources for security

- **Security groups**  
    Zijn virtuele firewalls die de in- en uitgaande verkeersstromen van je AWS-resources regelen. Ze zijn **stateful**, wat betekent dat als je een inkomend verzoek toestaat, het bijbehorende uitgaande antwoordverkeer automatisch wordt toegestaan, en vice versa.
- **Network Access Control List**  
    Zijn ook virtuele firewalls, maar ze werken op subnetniveau en zijn stateless. Dit betekent dat je expliciet zowel inkomende als uitgaande regels moet definiëren.
- **AWS WAF**  
    Beschermt je webapplicaties tegen veelvoorkomende exploits zoals SQL-injectie en cross-site scripting (XSS). Je kunt beveiligingsregels maken om webverkeer te filteren op basis van IP-adressen, HTTP-headers, en andere voorwaarden.
- **Amazon Virtual Private Cloud (VPC)**  
    Amazon VPC stelt je in staat om AWS-resources te lanceren in een virtueel netwerk dat je zelf definieert. Het biedt volledige controle over je virtuele netwerk, inclusief IP-adressering, subnetten, routeertabellen en netwerk gateways.
- **AWS Trusted Advisor**  
    Is een online resource die je helpt bij het optimaliseren van je AWS-omgeving. Het biedt aanbevelingen op het gebied van kostenoptimalisatie, prestaties, beveiliging, fouttolerantie en servicegrenzen.
- **AWS MarketPlace**  
    Is een digitale catalogus waar je software, data en diensten van derden kunt ontdekken, kopen en beheren.
- **AWS Knowledge Center**  
    Biedt een verzameling van artikelen en video's die antwoorden geven op veelgestelde vragen van AWS-klanten.

# Domain 3: Cloud Technology and Services (34%)

## 3.1 Define methods of deploying and operating in the AWS Cloud

- **Programmatic access**  
    Stelt gebruikers in staat om AWS-services te beheren via API-aanroepen, AWS SDK's, of de AWS Command Line Interface (CLI), in plaats van via de AWS Management Console.
- **AWC Command Line Interface (AWS CLI)**  
    Is een open-source tool waarmee je AWS-services kunt beheren via de command-line interface.
- **AWS Management Console**  
    Is een webgebaseerde interface waarmee je AWS-resources kunt beheren.
- **Infrastructure as Code (IaC)**  
    Een methode om je infrastructuur te beheren en te voorzien via code, in plaats van handmatige configuratie. AWS biedt tools zoals AWS CloudFormation en AWS CDK (Cloud Development Kit) om infrastructuur te definiëren en te beheren als code.
- **Cloud native**  
    Verwijst naar het bouwen en beheren van applicaties die volledig gebruik maken van cloud computing-omgevingen. Cloud-native applicaties zijn meestal opgebouwd uit microservices, draaien in containers, en maken gebruik van continue integratie en levering (CI/CD) om snel te kunnen schalen en updaten.
- **Hybrid cloud**  
    Combineert on-premises infrastructuur of private clouds met public clouds.
- **On-premises**  
    Verwijst naar IT-infrastructuur die fysiek binnen de faciliteiten van een organisatie wordt beheerd.
- **AWS public zone**  
    Een AWS netwerk waar ook andere gebruikers op zitten zoals een S3 bucket.
- **AWS private zone**  
    Een netwerk zoals een Virtual private cloud (VPC) waar alleen jouw services op draaien en van niemand anders.
- **AWS Direct Connect**  
    Een cloudservice die een dedicated netwerkverbinding tussen je on-premises netwerk en AWS biedt.
- **AWS VPN**
  - **AWS Site-to-Site VPN**: Creëert een beveiligde verbinding tussen je datacenter of kantoor en je AWS-cloudresources. Dit is nuttig voor het verbinden van gedistribueerde applicaties en het beveiligen van communicatie tussen locaties.
  - **AWS Client VPN**: Een volledig beheerde, elastische VPN-service die je remote workforce veilig toegang geeft tot resources binnen zowel AWS als je on-premises netwerken.
- **Internet gateway**  
    Een VPC-component die communicatie tussen je VPC en het internet mogelijk maakt. Het ondersteunt zowel IPv4- als IPv6-verkeer en zorgt ervoor dat resources in je publieke subnetten verbinding kunnen maken met het internet, mits ze een openbaar IP-adres hebben.
- **NAT gateway**  
    Een NAT Gateway (Network Address Translation) stelt instances in een privé subnet in staat om verbinding te maken met het internet of andere AWS-services, terwijl inkomende verbindingen van buitenaf worden geblokkeerd.

## 3.2 Define AWS global infrastructure

<img src='/images/Teaching_AWS_CCP//Screenshot 2025-06-25 175544.png'>

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175552.png'>

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175559.png'>

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175606.png'>

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175614.png'>

- **Edge location - AWS CloudFront/Global Accelerator**  
    If a user requests certain information, that information is than cached at the edge location nearby you. So, the next time a user requests the same information, that information is already available and delivered to the user much faster.
  - **Amazon CloudFront**  
        Improves cache content.
  - **AWS Global Accelerator**  
        Improves performance for applications, HTTP uses cases for a static IP, Fast regional failover.

## 3.3 Identify AWS compute services

- **EC2 instances**  
    Runs on EC2 hosts, which are the physical hardware. Shared hosts is the default. Your instances run together with other user’s instances on the same hardware, but they are isolated from each other. With dedication host your paying for the entire EC2 host. You do not share it. EC2 hosts sit in an AZ.
  - **Instance types**  
        <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175622.png'>

- **Amazon Elastic Container Service (Amazon ECS)**  
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175629.png'>

- **AWS Fargate**  
    AWS Fargate is a serverless compute engine for containers. It works with both Amazon Elastic Container Service (Amazon ECS) and Amazon Elastic Kubernetes Service (Amazon EKS). AWS Fargate makes it easy for you to focus on building your applications. AWS Fargate removes the need to provision and manage servers, lets you specify and pay for resources per application, and improves security through application isolation by design.
- **Amazon Kubernetes Service (Amazon AKS)**
- **AWS Lambda**  
    A Function as a Service (FaaS), accept functions, an event-driven service, execute code on a trigger event, serverless
- **Auto scaling groups**  
    In the context of EC2 instances, auto scaling groups provide high availability and scalability. To automatically scale EC2 instances based on different criteria.
- **Load balancing**  
    A method used to distribute incoming connection across a bridge of services or servers and incoming connections go to the load balancer and the load balancer distributes them to EC2 instances.

## 3.4 Identity AWS database services

- **Amazon Relational Database Service (Amazon RDS)**  
    A Database as a Service (DBaaS). RDS provides managed databases. RDS supports MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server and Amazon Aurora. 
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175637.png'>

- **Amazon Aurora**  
    Very different from Amazon RDS. Uses the base architecture of a cluster.
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175643.png'>

  - **Amazon Aurora Serverless**  
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175651.png'>

- **AWS DynamoDB**  
    Highly available across multiple AZs in a region. A fully managed NoSQL database service. It’s designed for applications that require consistent, single-digit millisecond latency at any scale. DynamoDB stores data in tables, and each item (row) is a JSON-like structure.
- In-memory databases
  - **Amazon ElastiCache**  
        Managed in-memory cache, Performance for reads, Redis and memcacheD, store session states.
  - **Amazon DynamoDB Accelerator (Daccs)**  
        Managed in-memory cache for DynamoDB, Access in milliseconds, eventually consistent reads.
- **Amazon Redshift**  
    Petabyte scale data warehousing solution, column-based database engine, designed for Online Analytical Processing (OLAP), unload and uploads data to Amazon S3. Not a type of database where you update the records. Great at queries for analytics. Not used for online transactional processes.
- **Database migration tools**
  - **AWS Snow Family**  
        A collection of physical devices and services offered by Amazon Web Services (AWS) designed to help customers move large amounts of data into and out of the AWS cloud, especially in environments with limited or no network connectivity.
    - **AWS Snowcone**  
            Smallest and most portable device in the Snow Family.
    - **AWS Snowball**  
            Comes in two variants: Snowball Edge Storage Optimized: Designed for large-scale data transfer and local storage.

Snowball Edge Compute Optimized: Offers more computing power for edge processing. Rugged and secure, with tamper-resistant enclosures. Supports EC2 and Lambda functions for edge computing.

- **AWS Snowmobile**  
    A massive data transfer solution—literally a shipping container on a truck. Can transfer exabytes of data (1 exabyte = 1,000 petabytes). Used by enterprises for large-scale data center migrations.
  - **AWS Database Migration Service (DMS)**  
        Service that helps you migrate databases to AWS quickly, securely, and with minimal downtime. It supports a wide range of database engines and migration scenarios.
  - **AWS Schema Conversion Tool (SCT)**  
        Helps with the migration between different database engines. Great for scaling resources up or down with little downtime and for migrating between on-prem to AWS and from AWS to/from other cloud platforms.
  - **AWS DataSync**  
        Makes is simple and fast to move large amounts of data online between on-prem storage and amazon S3, ERF or Amazon FSX. Automates many jobs such as copy jobs, monitoring transfer, validating data and optimizing network optimalisation.

## 3.5 Identity AWS Network Services

- **Amazon Virtual Private Cloud (Amazon VPC)**  
    A foundational service in AWS that lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. A regional service. Isolated and private by default. There will be a default VPC on every account.

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175659.png'>

- **NAT gateway (Network Address Translation)**  
    The process of giving a private resource outgoing access to the internet. Like the Internet gateway in AWS of a VPC.
- **VPC peering**  
    A way to link multiple VPCs together and allows direct communication between 2 isolated sites using their private Ip addresses. Can span across different AWS accounts and regions. Also encrypted.
- **VPC endpoints**  
    Are gateway optics that operate inside the amazon VPC. To allow instances inside an amazon VPC to connect to AWS public services without the need of a gateway. 
    <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175706.png'>

- **AWS Virtual Private Connection (AWS VPN)**  
    Your own virtual private network or VPN between your on-prem datacenter and your Amazon VPC and use AWS as an extension of your current datacenter or current environment. A hybrid environment. Highly available. Use the public network, fully encrypted route.
- **AWS PrivateLink**  
    AWS PrivateLink enables private connectivity between VPCs and supported AWS services without traffic traversing the public internet. It ensures secure communication for applications and services.
- **AWS Transit Gateway**  
    AWS Transit Gateway is a highly scalable service that connects multiple VPCs and on-premises networks through a central hub. It facilitates secure, private connectivity between VPCs and supported services without using the public internet.
- **AWS Direct Connect**  
    Another option to connect on-prem data with an Amazon VPC. A dedicated physical connection between your on-premises network and AWS.
- **Amazon Route 53**  
    AWS there managed DNS product. You can register domains and host zones and routing policies. A global service and replicated between regions. Translates addresses from a URL to Ip address and back.

## 3.6 Identity AWS Storage services

**AWS storage options**

- **Instance Store**  
    An instance store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. This is a good option when you need storage with very low latency, but you don't need the data to persist when the instance terminates or you can take advantage of fault-tolerant architectures.
- **Object**
  - **Amazon Simple Storage Service (Amazon S3)**  
        A global storage service platform. A public service and runs from the AWS public zone. Your stored data is replicated across different AZs. Unlimited amount of data. S3 bucket with a unique name up to 5 TB per object.
  - **S3 Glacier**  
        Low-cost, suitable for archives that can wait minutes or hours for access.
  - **S3 Glacier Deep Archive**  
        The lowest costs option and supports long term retention and data can be restored is 12 hours.
- **File**
  - **Amazon Elastic File System (Amazon EFS)**  
        Basically a NAS. Ideal for use cases such as large content repositories, dev environment, media storage or user home directories. Provides network based file systems that can be mounted witch Linux EC2 instances and then can be used by instances at the same time. Can not be used for Windows.
        <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175714.png'>

  - **Amazon File System (FSx)**  
        A fully managed Windows file system share drive that supports the SMB protocol and Windows network technology file system. It supports AD and control lists.
    - **Amazon FSx for luster**  
            A parallel distributed file system for large scale computing. Lusters name comes from Linux and cluster. It is for Linux instances and cluster is for large scale computing. Great for machine learning and more. High level distribution and low latency.
- **Block**
  - **Amazon Elastic Block Store (Amazon EBS)**  
        Enterprise applications such as databases often require dedicated low-latency storage for each host. This is comparable to direct or local attach storage or storage area network. Persistent storage. EBS are provisioned with each virtual server and offer the ultra-low latency required for high performance workloads. Has different volumes types and can make snapshots. Can be mounted to a single instance in the same AZ.  
        <img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175721.png'>

- **AWS Storage Gateway**  
    A service that allows us to connect our on-prem datacenter storage to an AWS storage service and it helps to migrate part or all of your storage platform to AWS or helps extending your storage platform to AWS. A product you download and configure on-premises.
  - **File gateway**  
        Stores objects in S3 with a on-prem local cache. Great for Windows.
  - **Volume gateway**  
        Gateway stored volumes and gateway caches volumes. Basically ISCSI.
  - **Virtual tape**  
        Virtual tape drive to S3. Use this for migration over time.

## 3.7 Identity AWS artificial intelligence and machine learning and analytics services

<img src='/images/Teaching_AWS_CCP/Screenshot 2025-06-25 175735.png'>

- **Amazon Translate**: Translate text content
- **Amazon Polly**: Text to speech conversion
- **Amazon Lex**: Building conversational chat bots
- **Amazon Rekognition**: Add image and video analysis to your applications
- **Amazon SageMaker**: enables developers and data scientists to quickly and easily build, train and deploy machine learning models at any scale.

**AWS analytics services**

- **Amazon Athena**  
    An interactive **query service** that allows you to query and analyze data stored in S3 using standard SQL. Considered serverless.
- **Amazon Macie**  
    Aws **security service** that uses machine learning to discover, classify and protect sensitive data stored in S3. Macie also uses AI to recognize if your S3 objects contain any sensitive data and provide dashboards, reports and alerts.
- **Amazon Redshift**  
    Petabyte scale data warehousing solution, column-based database engine, designed for Online Analytical Processing (OLAP), unload and uploads data to Amazon S3. Not a type of database where you update the records. Great at queries for analytics. Not used for online transactional processes.
- **Amazon Kinesis**  
    Analyses and processes data at any scale as a fully managed service. You can ingest **real-time** data such as video, audio, application logs and website clickstreams for machine learning analytics and other applications.
- **AWS Glue**  
    A serverless data integration service that makes it easy for analytic users to discover, prepare, move and integrate data from multiple sources. Use it for analytics and machine learning.
- **Amazon QuickSight**  
    A fast **business intelligence service** that delivers insights to everyone in your organization. As a fully managed service, Amazon QuickSight lets you create and manage your dashboards that includes machine learning insights.
- **Amazon OpenSearch**  
    A database and search engine.
- **Amazon EMR (Elastic MapReduce)**  
    A service that helps to run big data frameworks to process vast amounts of data.

## 3.8 Identify services from other in-scope AWS service categories

**Application integration services**

Services that enable communication between decouple components within microservices, distributed systems and serverless applications.

- **Amazon EventBridge**  
    A serverless event bus service that makes it easy to connect applications using data from your own apps, integrated SaaS applications, and AWS services. It helps you build event-driven architectures by routing events from sources to targets based on rules.
- **Amazon Simple Notifications Service (Amazon SNS)**  
    A highly available and durable secure pub sub messaging. A public AWS service. The core is sending and delivering of messages and SNS is used by many AWS services such as CloudWatch alarm change states, and auto scaling groups.
- **Amazon Simple Queue Service (Amazon SQS)**  
    A queueing system that provides fully managed highly availability messages queues and SQS can be used for inter process, servers and services messaging. If you have a queue, you add a message to the queue and then something else retrieves something from the queue. It offers an asynchronous way to talk between two components of an application. Used when you need to **decouple** two different parts of a system.
  - **Short and long polling**  
        Short polling is a single API call to check the queue for any massages and you will get the messages available in the queue up to a max of 10 messages or you will get no messages. Uses a lot of calls. Long polling is the same process for checking messages in the queue as short polling, however you wait for a certain amount of time (wait time seconds). More efficient, lot les API calls to the queue and empty API calls.
  - **Amazon MQ**  
        Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers on AWS.
- **Amazon CloudWatch**  
    Is een monitoring- en beheerservice die gegevens en bruikbare inzichten biedt voor AWS, on-premises, hybride en andere cloudtoepassingen en infrastructuurbronnen.
- **Amazon EC2 Auto Scaling**  
    In the context of EC2 instances, auto scaling groups provide high availability and scalability. To automatically scale EC2 instances based on different criteria.

**Business application services**

A service that improves agility and lowers cost.

- **Amazon Connect**  
    A content center that provides customer service for voice chat across services and agents.
- **Amazon Simple Email Service (Amazon SES)**  
    A cost-effective flexible and scalable email service to send mail from within any application. You can send emails securely globally and at scale.

**Customer engagement services**

Help to create the best customer experience across the entire life cycle.

- **AWS Activate**  
    Provides eligible startup free tools, resources and content designed to simplify every step of the startup journey.
- **AWS IQ**  
    Connects you to an AWS certified expert to get hands-on help for your AWS projects.
- **AWS Managed Services (AMS)**  
    Infrastructure operation management for AWS and it is an enterprise service that provides ongoing management of your AWS infrastructure.
- **AWS Support**  
    Offers a range of plans that support access to tools and expertise to support the success and operational help of your AWS solutions. Provide 24/7 access to provide customer service, AWS documentation, technical papers and support forums. You can choose a support plan for your use-case.

**Developer services**

Designed to help developers design and practice DevOps and deliver and deploy.

- **AWS AppConfig**  
    A service that helps you safely deploy application configurations to your environments without needing to redeploy code.
- **AWS CodePipeline**  
    A fully managed continuous integration and delivery (CI/CD) service that automates the build, test, and deploy phases of your release process.
- **AWS CodeDeploy**  
    AWS CodeDeploy is a service that automates code deployments to any instance, including Amazon EC2 instances and instances running on-premises. AWS CodeDeploy makes it easier for you to rapidly release new features, helps you avoid downtime during deployment, and handles the complexity of updating your applications.
- **AWS CodeCommit**  
    AWS CodeCommit is a fully-managed source control service that hosts secure Git-based repositories. It makes it easy for teams to collaborate on code in a secure and highly scalable ecos
- **AWS CodeBuild**  
    A fully managed build service that compiles source code, runs tests, and produces software packages ready for deployment.
- **AWS CodeArtifact**  
    A fully managed artifact repository service that makes it easy to securely store, publish, and share software packages used in your development process.
- **AWS X-Ray**  
    A distributed tracing service that helps developers analyze and debug production and distributed applications, including microservices.
- **AWS CloudShell**  
    A browser-based shell environment that provides command-line access to AWS resources directly from the AWS Management Console.
- **AWS Elastic Beanstalk**  
    AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services. You simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring.
- **Amazon LightSail**  
    Amazon Lightsail is designed to be the easiest way to launch and manage a virtual private server (VPS) with AWS. Amazon Lightsail plans include everything you need to jumpstart your project – a virtual machine, SSD- based storage, data transfer, Domain Name System (DNS) management, and a static IP address – for a low, predictable price. It is great for people with little cloud experience to launch quickly a popular IT solution ready to use immediately.

**End-user computing services**

Provide secure access to the application and desktop.

- **Amazon AppStream 2.0**  
    A fully managed application streaming service that provides users with instant access to their desktop applications from anywhere.
- **Amazon Workspaces**  
    Helps to provision vertical cloud based on Microsoft Windows, Amazon Linux or Ubuntu Linux desktops for users knows as workspaces.
- **Amazon Workspaces Web**  
    An on-demand, fully managed, Linux based service designed to facilitate secure browser access to internal websites and SaaS applications.

**Frontend web and mobile services**

To help developers develop workflows and deliver and deploy securely.

- **AWS Amplify**  
    A complete solution that lets front end web and mobile developers easily build, ship and host full stack applications on AWS.
- **AWS AppSync**  
    Provides a robust and scalable GraphQL interfaces for applications developers to combine data from multiple sources, including Amazon DynamoDB, AWS Lambda and HTTP APIs.

**IoT services**

Help to securely connect and manage devices, collect and analyze device data and build and deploy solutions. Provides software.

- **AWS IoT Core**  
    A managed cloud service that enables connected devices—such as sensors, actuators, embedded devices, or smart appliances—to securely interact with cloud applications and other devices.
- **AWS IoT Greengrass**  
    Extents cloud capabilities to local devices to collect and analyze data closer to the source of information, react autonomously to events and communicate securely with each other on local networks.

**Amazon search services**

- **Amazon Kendra**  
    Amazon Kendra is an intelligent search service powered by machine learning. Kendra reimagines enterprise search for your websites and applications so your employees and customers can easily find the content they are looking for, even when it’s scattered across multiple locations and content repositories within your organization.

# Domain 4: Billing, pricing, and Support (12%)

## 4.1 Compare AWS pricing models

- **Right-sizing**  
    Picking the correct instances for your current resources but also for resources you plan to use.

**Pricing models**

- **Reserved Instances**: commitment for a lower price. Up to 70% but one-to-3-year commitment.
- **On-Demand Instances:** Pay-as-you-go.
- **Spot Instances**: cheapest 90% off. Left over capacity. With interruptions.
- **Dedicated Instances**: Instances that you pay per hour running on a single tenant.
- Capacity reservations.
- **Dedicated hosts**: Reserved for you alone and not shared with other AWS customers.
- **Saving Plans**: Great for compute usage.
- **AWS Cost Explorer (service):** Monitor your costs.
- **AWS Trusted Advisor (service):** Monitors your infrastructure and can make recommendations on how to make your infrastructure more optimized.

**Best practices**

- Define and enforce tags
- Define your account structure
- Define and use metrics: track progress of goals
- Share ownership: give access to your team
- Cloud Center of Excellence (CCoE): a team who stays up to date to best practices, new releases and more to ensure your are using AWS in the most efficient and cost-effective ways.

## 4.2 Understand resources for billing, budget, and cost management

**AWS cost management tools**

- **AWS Cost Explorer**  
    AWS Cost Explorer has an easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time. AWS Cost Explorer includes a default report that helps you visualize the costs and usage associated with your top five cost-accruing AWS services, and gives you a detailed breakdown of all services in the table view.
- **AWS Cost and Usage Report**  
    The AWS Cost & Usage Report (AWS CUR) contains the most comprehensive set of cost and usage data available. You can use AWS Cost & Usage Report (AWS CUR) to publish your AWS billing reports to an Amazon Simple Storage Service (Amazon S3) bucket that you own. You can receive reports that break down your costs by the hour or month, by product or product resource, or by tags that you define yourself. AWS Cost & Usage Report (AWS CUR) cannot be used to identify under-utilized Amazon EC2 instances.
- **AWS Budgets**  
    Create billing alarms, free tier alarms and alarms with AWS budgets. Trigger automated actions based on defined budget thresholds.
- **AWS Organizations**  
    Multiple account treated as if they are one account, consolidated billing. Combines usage of all accounts to determine which volume pricing tier to apply.
- **AWS Control Tower**  
    Help to centrally manage and control access, compliance, security and shared resources too.
- **Amazon QuickSight**
- **Amazon Auto Scaling**
- **Amazon EC2 Auto Scaling**
- **AWS Trusted Advisor**  
    AWS Trusted Advisor is an online tool that provides real-time guidance to help provision your resources following AWS best practices. Whether establishing new workflows, developing applications, or as part of ongoing improvement, recommendations provided by Trusted Advisor regularly help keep your solutions provisioned optimally. AWS Trusted Advisor analyzes your AWS environment and provides best practice recommendations in five categories: Cost Optimization, Performance, Security, Fault Tolerance, Service Limits.
- **Amazon Data Lifecyle Manager**  
    Can automatically delete data
- **AWS Backup**
- **Amazon S3**

\- AWS charges you for data transfer between regions.  
\- There is a one-minute minimum charge for Linux based EC2 instances.

- **AWS Billing Conductor**  
    A custom billing service that supports show back and charge back workflows of AWS solution providers and enterprise customers.

## 4.3 Identify AWS technical resources and AWS support options

**AWS Support options**

- **AWS Enterprise Support**: 24/7 support from engineers and tools to automatically help to manage your environment. Account manager. Subject matter experts. You should use the AWS Enterprise Support plan to provide customers with concierge-like service where the main focus is helping the customer achieve their outcomes and find success in the cloud. With AWS Enterprise Support plan, you get 24x7 technical support from high-quality engineers, tools and technology to automatically manage the health of your environment, consultative architectural guidance delivered in the context of your applications and use-cases, and a designated Technical Account Manager (TAM) to coordinate access to proactive/preventative programs and AWS subject matter experts.
- **AWS Developer Support**: an unlimited amount of cases that can be opened. You should use the AWS Developer Support plan if you are testing or doing early development on AWS and want the ability to get email based technical support during business hours as well as general architectural guidance as you build and test.
- **AWS Enterprise on-Ramp Support**  
    You should use the AWS Enterprise On-Ramp Support plan if you have production/business critical workloads in AWS and want 24x7 access to technical support and need expert guidance to grow and optimize in the Cloud. AWS Enterprise On-Ramp Support plan provides 24x7 phone, email and chat access to technical support however it's costlier than the AWS Business Support plan.
- **AWS Basic Support**
- **AWS Business Support**: access to infrastructure event management for an additional fee. You should use the AWS Business Support plan if you have production workloads on AWS and want 24x7 phone, email and chat access to technical support and architectural guidance in the context of your specific use-cases.
- **AWS documentation**
- **AWS re:Post**
- **AWS white papers**
- **AWS blogs**