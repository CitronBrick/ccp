# Database

## Relational Database Service (RDS)

Managed Relational Database Service, in which AWS takes care of OS, Infrastructure, Scaling, Backup & Database Software patching.

### Shared responsibility
RDS takes care of hosting, infrastructure of DB instances & clusters.  
User responsible for query tuning & monitoring (can use RDS Performance Insights)

### Instance types

* General purpose
	* db.m6g: AWS Graviton 2 processors
	* db.m6gd: AWS Graviton 2 processors with SSD block-level storage.
	* db.m6i: General purpose
	* db.m5d: optimized for low latency
	* db.m5: general purpose. Powered by AWS Nitro. Computing better than db.m4
	* db.m4: computing better than db.m3
	* db.m3: computing better than db.m1

* Memory optimized
	* db.x2g: AWS Graviton 2. Low cost
	* db.z1d: High memory & high compute
	* db.x2i: Powered by 2nd or 3rd gen Intel Scalable processors (Cascade Lake or Ice Lake).
	* db.x1e: Lowest price
	* db.x1: Lowest price

### Billing

* DB instance hours /h
* Storage: Gb/month
* IO req: 1M req/month
* Provisioned IOPS: /IOPS/month
* Backup storage: /Gb/month
* Data Transfer: /Gb

#### On-Demand instances

Pay by hour for the DB instance. Bills are calculated per second using decimal. Minumum 10 seconds.

#### Reserved instances

Reserve DB instance for 1 or 3 years. Significant discount. Delete, start, stop multiple instances within the hour & still get discount.



## Aurora

Fully managed relational database compatible with MySQL (5x throughput) & PostgreSql (3x throughput).

## DynamoDB

Fully managed NoSQL database, with fast & predictable performance & scalability.  
Encryption at rest. On-Demand backup.

## RedShift

Fully managed petabyte scale data warehouse service, to analyze data using business intelligence tools.  
Optimized for 100 Gb - 1 Pb. Costs < 1000$/Tb a year.

## ElastiCache

Setup, manage & scale distributed in-memory databases (Redis & MemCached) in the cloud.





# Security

## Web Application Firewall (WAF)

Web Application Firewall monitors web requests forwarded to CloudFront or an Application Load Balancer.  
Can conditionally block from ip addresses.

## AWS Firewall Manager

Administer & maintain WAF across multiple accounts & resources.

## Shield 

protects from DDoS. Free.

## Shield Advanced

expanded DDos protection. Rapid access to DDoS mitigation experts. Paid.
During attack, advances ACLs to application's network border.

## Artifact

On-demand downloads of AWS security & compliance documents, viz: PCI reports, Service Organizational Control (SOC) reports, ISO certifications etc.

## AWS Certificate Manager

Creates, stores & renews public & private SSL/TLS X.509 certificates & keys.  
Certificates are deployed through Elastic Load Balancing, CloudFront, Amazon API Gateway etc. 
For public websites.
Not intended for use with a standalone web server.

## Cloud HSM

Provides customers with hardware security modules (HSMs) in the AWS cloud.  
HSM is a computing device that processes cryptographic operations & safely stores cryptographic keys.  
Pay hourly with no long-term commitments nor upfront payments.

## AWS License Manager

Manages licenses from  software license vendors.  
Bring Your Own License (BYOL) model saves money.  
Provides dashboard showing license usage.

## Detective

Analyze, investigate & identify root cause of security findings. Automatically collects log data from AWS resources.  
Uses machine learning, graph theory & statistical analysis.

## GuardDuty
Security monitoring service that analyzes & processes:
* Foundational data sources: CloudTrail management events, CloudTrail event logs, VPC flow logs & DNS logs.
* Features: k8s audit logs, RDS login activity, S3 logs, EBS volumes,   
Informs you of the status of findings.

## Macie 
Fully managed data security & data privacy service. Discovers sensitive data in S3 through machine learning.

## Inspector
Vulnerability management service that scans EC2 instances, ECR images & Lambda functions for CVE vulnerabilities.




# Integration

## Amazon MQ

managed message broker service. Compatible with popular message brokers & protocols.

## Step Functions

Serverless orchestration service. Describes workflows (series of event driven steps) using state machines.

## Amazon Simple Queue Service (SQS)

Fully managed message queuing service that makes it easy to decouple microservices.
Incompatible with existing messaging systems. 1,000,000 free requests per month.
1 req = max 64 kb. Extra charges for interaction with S3 & KMS.

## Amazon Simple Notification Service (SNS)

Managed publisher-> subscriber topic based messaging service. App->App & App->Person.
Messages can be received through email, HTTP, Lambda, mobile push notifications, SMS & Kineses Data Firehose.
Used for Fanout scenario. Filter policies based on message boy or attributes.

# Compute

## Batch

Run batch jobs (.sh, Docker images, Linux executables) across AZs. No infrastructure management.
Can use EC2 or Fargate. Integration with EventBridge, CloudWatch & CloudTrail.  
Region based quotas.


## Amazon Workspaces

Provision virtual cloud based (Windows, Linux) desktops for users. Accessible through browser & multiple devices.  
PCoIP or WSP.  
Can use KMS to encrypt data at rest. Each workspace is associated with a VPC.


# Storage 

## S3

can be queried with S3 Select, Athena & Redshift.

Bucket: container of objects.

* since Jan 23, default free server-side encryption with  Amazon S3 managed keys (SSE-S3 )

Object: file or file metadata. Cannot exist without a bucket.

* Versioning
* Replication
* Locking
* Tags (case sensitive key value pair)
* Batch operations

### Object Lock

Write Once Read Many (WORM) model. Prevents objects from being deleted for a period or forever. *CFTA* & *FINRA* regulations.

### Replication

Asynchronous copying of objects across buckets. Destination bucket(s) can be present in any Region & can be owned by any account.  
**S3 Storage Lens**: analytics feature for S3.

| Cross-Region Replication | Same-Region Replication |
| --------------------     | ----------------------  |
| Compliance requirements  | Sovereignty laws        |
| Minimize latency         | Aggregate logs in 1 bucket |
| Increase operational efficiency | Live replication between prod & test accounts |

### S3 with Outposts.

* Max bucket size 50Tb
* Max 100 buckets/account

#### Unsupported on S3 in Outposts

* ACLs
* CORS
* Batch operations
* Inventory reports
* Public buckets
* Server side encryption with KMS
* S3 Events Notifications & Lambda events
* CloudWatch metrics
* S3 Object Legal Hold
* Object Lock retention
* MFA delete

### S3 Storage Classes

**S3 Standard**: default
**Reduced Redundancy**: not recommended  
**S3 Standard IA**: stored across multiple AZ. Millisecond acces. Infrequently accessed.
**S3 1 Zone IA**: stored in 1 AZ. Infrequently accessed. Millisecond acces. Less available, less resilient. Use if data can be recreated & for object replicas when configuring Cross-Region Replication.
**S3 Glacier Instant Retrieval** : Use for rarely accessed data with millisecond access. Cheaper than S3 Standard IA with same latency & throughput. Higher data access cost than S3 Standard IA.
**S3 Glacier Flexible Retrieval**: Min 90 days storage. Expediated retrieval in 1-5min. Free bulk retrieval in 5-12 hours.
**S3 Glacier Deep Archive**: Min 180 days storage. Default retrieval time: 12 hours.

### S3 Versioning

* Multiple variants in the same bucket. 
* Disabled by default
* Deleted objects are just marked as deleted.
* Suspendable

### S3 Inventory

Inventory  is a list of objects in the source bucket & is stored in the destination bucket. Equivalent to List API.  
Source & destination buckets can be the same. Destination should be in the source's Region.  
Eventually consistent for PUT & DELETE.

#### Inventory Formats

* CSV compressed with Gzip
* ORC compressed with Zlib
* Parquet compressed with Snappy

#### Inventory metadata

* Bucket name
* Key name
* Version ID
* Storage class
* Delete marker
* IsLatest
* size
* Last modified date
* ETag (hash of object's content)
* Multipart upload flag
* Replication status
* Encryption status
* Lock Retain until date
* Lock mode
* Lock Legal Hold status
* Bucket key status
* Checksum algo
* Intelligent Tiering access tier

## AWS Backup

Fully managed, automated, centralized Backup service. Configure backup policies. Cross region backup possible.  
Support for S3, EBS, EC2, EFS, FSx, DocumentDB, RDS, Aurora, Neptune, Timestream, VMWare & CloudFormation templates.  
Support varies per Region. Create policies (backup plans).  
If full support available for Region, then automatically encrypted with KMS key of Backup vault (not source).  
Locking available. PCI, HIPAA, GDPR etc compliant. Maintains EBS & EC2 lifecycle even after the former's deletion. 

## EBS (Elastic Block Store)

Payment: Gb/month.

## Snow Family

### Snowcone

Snowcone has 2 vCPUs, 4 Gb memory & 8 Tb hard disk (HDD).  
Snowcone SSD has 2vCPUs, 4 Gb memory & 14 Tb hard disk (SSD).

Weighs 2kg. Can run EC2. In North America, can use Wifi. NFS support.

### Snowball

80Tb Snowball retired.

### Snowball Edge

| Device Configuration | Capacity |
| ----- | --------- |
| Storage optimized | 100 Tb |
| Storage optimized with EC2 Compute | 80Tb, 24 vCPUs, 32 Gb ram, 1Tb SSD |
| Compute optimized | 104 vCPUs, 416 Gb RAM |
| Compute optimized with GPU | Same as above with GPU |

### Snowmobile

Move upto 100 Pb data in a shipping container. 

## AWS Storage Gateway

Service connecting on-premise storage with AWS cloud storage.

### S3 File Gateway

File interface into S3 using NFS & SMB. The Gateway is a VM deployed on-premise.

### FsX File Gateway

Low latency access to cloud FsX for Windows File Server.

### Tape Gateway

Store data on virtual tape catridges.  
Can host either onsite as VM or on cloud as EC2.

### Volume Gateway

Cached Volumes: Data in S3. Cache in on-premise.
Stored Volumes: Data in on-premise. Snapshots in S3

# Container

## Elastic Container Service (ECS)

* Fully managed container orchestration service with built-in AWS configuration & best practices.
* No need to manage control planes, nodes & add-ons.
    * Serverless option with AWS Fargate
    * External instance option with ECS Anywhere
    * EC2 option
* CloudWatch integration

## Elastic Kubernetes Service (EKS)

Managed service to run k8s on AWS & operate your own k8s clusters.


# Analytics

## Athena

Interactive query service for S3. Can be used with Spark. Serverless.

## Quicksight

* Fast business analytics service 
* build visualizations (charts) , 
* perform ad hoc analysis
* get buisness insights from data
* can use relational data (db) sources (S3, RedShift, MySQL, Terradata, Spark, Timestream etc.)
* query data using Superfast Parallel In-memroy Calculation Engine (SPICE)

## Kinesis

Collect, process, & analyze video & data streams in real time.

* **Video Streams**: capture, process & store video streams.
* **Data Streams**: Build custom applications to analyze data streams using popular stream processing frameworks.
* **Data Firehose**: Load data streams into AWS data stores.
* **Data Analytics**: process & analyze data using SQL or Java
* **Kinesis Agent for MS Windows**: collect, parse, transform, stream logs, events & metrics from fleet  of Windows computers & servers (on-premise or AWS Cloud)

# Networking & Content Delivery

## VPC (Virtual Private Cloud)

Virtual network, resembling a traditional data center network.

### Subnet

Range of IP addresses that belong to 1 AZ. Contain resources.

### IP addressing

IPv4 & IPv6 IP addresses can be added to VPCs & subnets.  
You can bring your own IPv4 & IPv6 GUA addresses to AWS & allocate them   
to resources, viz. EC2 instances, NAT gateways & Network Load Balancers.

### Routing

Use route tables

### Gateways & endpoints

Gateway connects a VPC to another network.  
Use Internet Gateway to connect VPC to internet.  
Use VPC endpoint to connect to AWS services privately without Internet Gateway or NAT device.  

### Transit Gateway

Central hub, to route traffic between VPCs, VPN & AWS Direct Connect connections.

### VPC Flow Logs

capture informationa bout IP traffic.

### AWS Virtual Private Network (VPN)

connect VPC to on-premise networks.

### VPC peering connection

Networking connection between 2 VPC, using private IP addresses.  
The VPCs can be in different accounts & regions.  
Free within same AZ.

### Traffic Mirroring

VPC feature to copy traffic from an elastic network interface.  
Sent to out-of-band security & monitoring appliances.  
The latter instances, can be deployed individually or as a fleet behind a Network Load Balancer or Gateway Load Balancer, both with UDP listener.  
Supports filtering & truncation.

### Security Group

Controls traffic to/from resource. A VPC has default Security Group when created.

| Security Group | Network Access Control List |
| ----  | ---- |
| instance level | subnet level |
| associated instance | all instances in subnet |
| allow rules only | allow & deny rules |
| evaluates all rules | evalualtes rules in order |
| stateful: return traffic is always allowed | stateless: return traffic must be explicitly allowed |


## CloudFront

Speeds up distribution of static & dynamic web content, through edge locations.  
Can be used for Video On Demand or live streaming.  
Payment for storing in S3, serving from edge locations & data transfer.  

## AWS Direct Connect

Links internal network to AWS Direct Connect center through Fiber Ethernet.  
Bypass ISP. Multi-region (except China).

## Amazon API Gateway

create, publish, maintain, monitor & secure HTTP & Websocket APIs at any scale.
IAM, Lambda authorizer functions, Cognito User Pools.  
Use CloudFormation.  
Integrationw with WAF, X-Ray, CloudWatch & CloudTrail.

## Route 53

DNS web service. 
1. Register domain name
2. Route traffic to the resources for your domain
3. Check resources health

## Elastic Load Balancers.

Automatically distribute your incoming traffic across multiple targets (EC2, containers, IPs),  
in 1 or more AZ. Monitors target health & routes only to healthy ones.

### Application Load Balancer

Routes HTTP/HTTPS requests.

### Network Load Balancer

Routes TCP, UDP & TLS traffic.

### Gateway Load Balancer

Deploy, manage & scale virtual appliances viz. firewalls, deep packat inspectors, intrusion detectors & preventors.  
Works at Network layer. Single point of entry & exit.





# Developers

## CodeBuild

Fully managed build service to compile, run unit tests & produce deployable artifacts.  
Price & limits per each supported region.

## CodeCommmit

Version Control service (git) hosted by AWS to privately store code.  
HIPAA, PCI DSS, FIPS & ISO 27001 compliant.  
Free tier (Rep/account, requests/month, Gb storage)

## CodeDeploy

Deployment service to EC2, Lambda, ECS & on-premise.  
Does not store code. TLS encryption in transit.

## CodePipeline

Continuous Delivery service to model, visualize & automate steps to release software.  
Free tier. 1$ per active pipeline/month.

## CodeStar

Cloud based service for creating, managing & working with software development projects.  
Creates & integrates AWS services via templates. Depending on template, may include  
version control, build, deployment, virtual servers or serverless.  
Also manages permissions for team members. Includes Project Dashboard & Issue Tracking.

# Misc

## AWS Marketplace

Digital catalog to find, deploy, manage 3rd party software, data & services. Listed by categories.  
Can by AMIs. Providers need to meet **AWS Data Eligibility** requirements.  
Use *Standard Contract for AWS Marketplace* as EULA between seller & buyer.  
*AWS Marketplace Seller Terms*

## Amazon Connect

Contact Center as a Service (CCaS). Omni channel cloud contact center.  
Setup contact center & add agents (available in specific regions only).

# Management

## Well Architected Framework Pillars

* Operational Efficiency
* Security
* Reliability
* Performance Efficiency
* Cost Optimization
* Sustainability


# Billing

## Pricing Calculator

Create cost estimates for your AWS use case.  
Useful for both non-prior AWS users, & those who want to expand its usage.

## AWS Cost Management Console

Optimize future costs.

### Cost Explorer

View & analyze costs & usage reports & graphs. View data for last 12 months & forecast for next 12 months. 
Free UI, 0.01$ per API request. Cannot be disabled after being enabled.


## Service Quotas

view & manage AWS quotas from a central location.

## Support Plans

### Basic

For all. 24x7 documentation, whitepapers, re:Post, Trusted Advisor, Personal Health Dashboard.

| Empty | Developer | Business | Enterprise On-Ramp | Enterprise |
| --- | --- | --- | --- | --- |
| Trusted Advisor| Basic | All | All | All | All checks + Recommendations |
| Enhanced support | Cloud associates in Business hours | 24x7 phone, web, chat with Cloud support Engineers, Slack, re:Post priority | = | = |
| ETA | < 12h | < 1h | < 30min | <15min |
| Technical Account Manager | - | - | Pool | Dedicated |
| Concierge support team | - | - | yes | yes |
