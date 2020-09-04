
## Cloud Computing
It is on demand delivery of compute power,database storage, applications and other IT storage.  

### Characteristics of Cloud Computing

- Trade capital expense over operational expense
- Benefits from massive economies of scale
- Stop guessing capacity
- Increase speed and agility 
- Stop spending money running data centers
- Go global in minutes

### Types of Cloud Computing

- IaaS
    - Provides networking, storage and computers
    - eg . EC2,  Azure , GCP
     
- PaaS
    - Removes need to manage underlying infrastructure
    - Focus on deployment and management of your applications
    - Application and Data is managed by you and rest all by AWS
    - eg. Elastic Beanstalk
   
- SaaS
    - Complete product is run and managed by AWS
    - eg. Rekognition, Lambda, Gmail , Dropbox,S3

### Pricing in the cloud
Pricing is dependent on :

- Compute
- Storage
- Data transfer out of the cloud
- In most cases, there is no charge for data transfer between other AWS services within the same region. 
- Outbound data transfer is aggregated across services and then charged at the outbound data transfer rate.

---

## AWS Global Infrastructure
- Regions
    - Contains AZ
    - Min : `2`, Max : `6` , Usually : `3`
- Availability Zones
    - Contains data centers(DC)
        - Min : `1`
        - DC has redundant power , n/w and connectivity
- Points of presence 
    - Content delivery as close to users
    - Total : `216`
        - Edge Location : `205`
        - Regional Cache : `11`

---

## Identity and Access Management 

- Least privilege principle
- A user can be part of none/single/multiple groups
- Group can not be part of a group
- Roles can be assigned to a user also
- **Global free** service
- User and groups are assigned via json documents c/a policies
- Policy, when associated with an identity or resource, defines their permissions.
- AWS evaluates these policies when an IAM principal (user or role) makes a request. 
- Permissions in the policies determine whether the request is allowed or denied.
- Each policy consists of:
	- Actions: What actions to allow or deny.
	- Resources: Which resources to allow or deny the action on.
	- Effect: What will be the effect when the user requests access—either allow or deny.
	- Conditions: Permissions are granted to IAM identities (users, groups, and roles) to determine whether they are authorized to perform an action or not.
- When you assume a role, it provides you with temporary security credentials for your role session. You can use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources.

### Multi Factor Authentication
Password + Device specific token

#### Virtual Devices 
- Google Authenticator (phone only)
- Authy  (multi device) , Support of multiple token on single device

#### Physical Devices 
- Universal 2nd Factor Security Key - Yubikey (Multiple key)
- Hardware Key Fob (Provided by Gemalto)
- Hardware Key for GovCloud (Provided by SurePassID)

### Access to AWS
AWS can be accessed by 3 modes- 
- CLI
- SDK
- Management console

### IAM Security Roles
- Some AWS services perform action on your behalf. Permissions are assigned to services with IAM roles
- Common Roles
    - EC2 Instance
    - Lambda
    - Roles for CloudFormation
    
### IAM Security Tools
- Credentials Report
    - Account level
    - Accounts, Credential Status 
- IAM Access Advisor    
	- User level
    - Shows services permissions assigned to accounts and when those permissions were last accessed

### Best Practices
- Don't use root account except for AWS a/c set up
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Use and enforce MFA
- Create and use roles to provide permissions to AWS Services
- Use access key for programmatic access (CLI/SDK)
- Audit permissions using Credential Report

---

## EC2

Provides option to chose 
- AMI (OS)
- Instance Size (Compute Power + RAM )
- Storage Service
- N/W card
- Firewall rules
- Bootstarp script (EC2 User Data)

- Allow you to install and run custom relational database

### Purchasing Option
Pay for what u choose


### Pricing depends on
EC2 instance pricing varies depending on many variables:
- The buying option (On-demand, Reserved, Spot, Dedicated)
- Selected AMI
- Selected instance type
- Region
- Data Transfer in/out
- Storage capacity

### Types of EC2

#### EC2 On Demand
- Pay for what you use
    - **Linux** - `Billing/sec` and minimum is `60 sec`
    - **Windows** - `Billing/hour`
- Short term , uninterrupted load
- Most expensive
- Predictable pricing
- Remove the need to buy “safety net” capacity to handle periodic traffic spikes. 

#### EC2 Reserved Instances
 - `72 %` discount
 - Reservation period : `1year/3 years`
 - Recommended for steady stage usage app (eg . DB)
 - Can be of 3 types :
    - Scheduled
        - Launched within the time window you reserved
        - When you require traction of days/months/weeks
        - Commitment for 1 to 3 yrs
    - Convertible
        - Can change EC2 instances
        - `54%` discount 
        - Long workloads with flex instances
    - Reserved Instance for long work loads

#### EC2 Spot Instance
- `90%` discount
- Instance can be lost anytime if ur max price is less than current spot price
- Not suitable for critical workloads
- Useful for workloads that are resilient to failure
    - Batch jobs
    - Data Analysis
    - Image processing
    - Distributed loads
    - Workloads with flex start / end time
- Most cost efficient
- Short workloads

#### EC2 Dedicated Instance 
- Instance running on H/W is dedicated to you
- No control over Control Instance Placement (Can move h/w between instances)
- May share hardware with instances in same account

#### EC2 Dedicated Hosts 
 - Dedicated host
 - `Allocated for 3 years`
 - Very expensive
 - Address compliance requirements
 - Reduce cost by allowing you to have 2 server outbound liscences
 - Useful for `Bring your own license(BYOL)`
 - Control instance placement

#### Shared Responsibility 
- AWS 
  - Infrastructure
  - Isolation of physical host
  - Replacing faulty h/w
  - Compliance validation 
  - Physical and Env controls

- User
  - Security group
  - O/S patches and updates
  - S/W and utilities on EC2
  - IAM roles
  - Data security
  - Patching EC2 instances
  - Service and Communication Protection and Zone security
---

## EC2 Instance Storage
   
### EBS Volume
- **Block Storage**
- Like a **network drive** that can be attached
- Persist data even after termination
- EBS Volume in one zone cannot be attached to another . It needs to be done via snapshot
- Uses n/w to communicate
- **Region specific**
- **Single AZ**
- Can copy snapshots across region/AZ
- Charged by **Volume Type** , **Provisioned iops(Input/Output Operations/second** and **Provisioned storage volume**
- Uses n/w to communicate and reduces latency
- It can be detached from one EC2 instance and attached to another one
- To create a snapshot, it is not recommended to detach a volume, but recommended
- Have a provisioned capacity (size in GB and vol)
    - You get billed for all provisioned capacity
    - You can **increase capacity of drive over time**
-  Primary storage device for data that requires frequent and granular updates. 
- Amazon EBS is the recommended storage option when you run a database on an EC2 instance. 

### AMI
- Created **Region specific**
- Can be copied across regions
- Building an AMI also causes EBS snapshots to be created
- **Server-less service**
- Customization of EC2

### EC2 Instance Store
- **Block Storage**
- Hardware can be attached
- Better I/O
- **Loses storage if the instance is stopped**
- Risk of data loss if h/w fails
- **High performance hard disk**
- Back up your responsibility
- **Good for buffer/cache/scratch data/temporary content**  

### EFS(Elastic File System)

- Managed NFS that **Can mounted on 100s of EC2 Instances in a region**
- **Multiple Availability zones**
- **Thrice the price** , pay per use
- **Shared by everything that is mounted on it**
- **Only wid linux ec2 instances in multi AZ**
- EC2 instances can access files on an EFS file system across many Availability Zones, regions and VPCs
- Scalable and expensive
- No capacity planning
- Can be directly used with on-premises systems

### Shared Responsibility for EC2 Storage

#### AWS
- Infrastructure
- Replication
- Replace faulty h/w
- Ensuring employees cannot see their data

#### Customer
- Setting up backup/snapshot process
- Setting up encryption
- Responsibility of data on drives
- Understanding risk

---

## Scalabilty

### Vertical 
- **Scale up** - Increase size
- **Scale down** - Decrease size

### Horizontal (Elasticity)
- Increase # of instances
- **Scale out** - Increase number of instances
- **Scale In** - Decrease number of instances
- Achieved via ELB and ASG

## Availability
- Execution of application in multiple AZs
- Achieved via ASG multi-AZ and LB muti-AZ

## Elasticity
- Auto scaling based on load
- Cloud friendly
- Pay per use and match demand

## Agility
- New IT resources are only click away
- Make them available real quick

---

## Load Balancing
- Spread load against multiple downstream instances
- Expose single DNS
- Seamlessly handle failures of downstream instances
- Do regular health checks
- Provide SSL termination (HTTPS) of ur instances

### ELB
- Can be **Multi AZ**
- Supports health checks
- Managed LB
- AWS guarantees that it will be working
- AWS takes care of upgrades , maintenance , HA
- AWS provides only few config knobs

#### Types of ELB
 There are 3 types of LB : 
- Application LB
	- HTTP/HTTPS
	- `Layer 7`
- Network LB
    - Ultra high-performance
    - Allows for TCP and UDP
    - `Layer4`
- Classic LB 
    - Layer 4 and 7
    - Retiring

### ASG
- **Scale out** - Adds instances to match increase in load
- **Scale in** - Remove instances to match decrease in load
- Replaces unhealthy instances
- Automatically registers new instances
- Ensures min and max # of machines are running
- Cost savings : Only run at an optimal capacity

---
 
## Amazon S3
- **Infinitely scaling storage**
- Disaster Recovery
- Archive
- Hybrid Cloud storage
- S/W delivery
- Directory -> **Buckets**
- Files - > **Objects**

### Objects
- Possess a key
- Buckets are defined at **Region level** but must have globally unique name
- Keys is full path `s3://mybucket/myfile.txt`
- Prefix + Object name
- No concepts of directories
- **Max obj size** : `5 TB`
- If `> 5GB` , then **multi-part upload**
- Metadata (K/V pair) - System /User metadata
- Tags(Unicode K/V pair - upto `10`),which is useful for security and lifecycle

### Security
- User based
    - IAM policies
    - Which API calls should be allowed for specific user from IAM console
- Resource based
    - Bucket policies : Bucket wide rules from S3 console - Allows cross a/c
    - Object ACL : Finer grain
    - Bucket ACL : Less common
    - Force objects to be encrypted at upload

### S3 Websites
- Can host static websites and have them accessible from www
- URL will be : 
    - <bucket-name>.s3-sebsite-<AWS region>.amazonaws.com
    - <bucket-name>.s3-sebsite.<AWS region>.amazonaws.com
- You will get 403 error will bucket policy does not allow public reads

### S3 Versioning
- Versioning is enabled at **Bucket level**
- Any file which is not versioned prior to enabling , will have versioning as NULL prior to enabling versioning
- Suspending versioning does not delete prior versioning
- Protect against unintended deletes
- Easy to rollback to previous version

### S3 Access Logs
- Log all access to S3 buckets
- For audit purpose
- Logging bucket

### S3 Replication
- Can happen in an async manner
- **Must enable versioning in source /target**
- Can happen only on new objects
- 2 Types
    - CRR : Cross Region Replication
        - Use Cases:
            - Compliance
            - Lower latency access
            - Replication across accounts
    - SRR : Same Region Replication
        - Use Cases : 
            - Log aggregation
            - Live replication between prod and test a/cs

### S3 Storage Class

#### Standard
- **Frequently access data**
- Low latency and high thruput
- **No Retrieval fees**
- Sustain `2` concurrent facility failure
- Use Case - Big Data , Mobile and Gaming , content distribution

#### Intelligent Tiering
- Low latency and high thruput perf
- Cost optimised by automatically moving object between 2 access tiers based on access patterns
- Types
    - Frequent access
    - Infrequent access
- **Resilient against events that impact entire AZ**
- ** No retrieval fees**

#### Standard Infrequent Access(IA)
- **Less frequent access data but rapid access**
- Lower cost compared to S3
- **Retrieval fees**
- Sustain 2 concurrent facility failure
- Use case : **Recovery and backups**

#### One Zone IA
- Same as IA but data is stored in `1 zone`
- Lower cost compared to S3-IA(`20 %`)
- Low latency and hi thruput perf
- Use case : **Storing secondary back up copies of on-prem data / storing data that can be recreated**

## Archive
- Low cost Object storage (in GB/month) 
- Meant for archiving/backup
- Data is retained for longer term (years)
- Various retrieval options of time 
- **Retrieval fees**

### Glacier 
- Back Up and Archive
- Cheap
- Data is retained for longer duration
- Types
    - Expedited (1 to 5 min)
    - Std (3 to 5 hrs)
    - Bulk (5 - 12 hrs)

- `90 days` of minimum storage duration
- `40 KB` of minimum capacity/object fee

### Glacier Deep Archive
- Cheapest
- Types 
    - Std - 12 hrs 
    - Bulk - 48 hrs
- `180 days` of min storage duration 
- `40 KB` of min cap/obj fee 

### S3 Storage Class Comparision
Using lifecycle config , storage class can be changed
- Availability zones
    - >= 3 Standard, Intelligent Tiering, Std IA, Glacier , Deep Archive
    - 1 : One Zone
    
- Min Capacity charge / obj
    - N/A - Std, Intelligent Tiering
    - 128 kb - Infrequent Access
    - 40 kb - Glacier , Deep archive
    
- Min storage duration charge   
    - N/A - Standard
    - 30 days - Intelligent Tiering , Infreq access , Infreq access - obe Zone
    - 90 days - Glacier
    - 180 days - Deep archive

 - Retreival fee
    - N/A - Std , Intelligent Tiering
    - Rest all - /GB retrieved

## Physical Data Transport

### Snowball

- **Physical data xfer in /out of AWS**
- **TB and PB of data**
- Pay/data Xfer job
- If it takes **1 week to xfer data** , then use snowball
- Use cases : Large data cloub migrations , DC decommision, DR
- Process is
    - Request AWS device
    - Install client
    - Connect and copy files
    - ship back device
    - Data will be loaded to **S3 bucket** and snowball will be wiped out

### Snowball Edge

- **Additional Computational capacity** to device
- Supports custom AMI so that u can perform processing on the go
- Useful to process data while moving
- **Supports custom Lambda functions**
- Useful for pre-process the data while moving
- Use cases : ML, IoT capture, Data Migration, Image collection

### AWS Snowmobile
- Truck
- xfers **exabytes of data**
- Has **100 PB** cap
- **Better than Snowball if you xfer > than 10 PB**

### Conversion matrix 
- PB= 1000 TB
- ExaByte = 1000 PB

---

### Storage Gateway
- **Bridge between on-prem data and cloud data in S3**
- **Expose S3 data on prem**
- **Hybrid service** to allow on prem to use AWS cloud
- `3 types` of Stotage Gateway are :
    - Volume
    - File
    - Tape

### Storage Options
- Block
    - EBS
    - Instance Store
- File
    - EFS
- Object
    - S3
    - Glacier
    
## AWS DB

### RDS
    - **Relational**
          - PostGres, MySQL, Oracle, SQL Server , Aurora
    - **Read replicas** for performance improvement
	- **Read Replicas** improves database scalability
	- Read Replicas features facilitates offloading of database read activity
    - **Multi AZ (optional - need to configure)**
    - _ + | scaling of instance size and storage
    - **Storage backed up by EBS**
    - Automated provisioning , OS patching
    - **Continuous backup and restore to specific times with a granularity of as little as 5 minutes(Point in time references)**
    - Dashboard monitoring
    - Disadv : cannot SSH into EC2
    - Maintenace windows for upgrades
	- AWS performs DB set up and management of OS

### Aurora
    - MySQL and PostGres
    - **5 X perf over MySQL & 3 X perf over postGres**
    - Storage auto grows in increments of 10 GB upto 64 TB
    - Cost is 20 % > RDS, but more efficient
    - Not in free tier

### Elastic Cache
    - **Mem cached or Redis**
    - In mem DB and high performance
    - Low latency
    - **Helps reduce load off database for read intensive workloads**
	- Improves web application performance
   
### Dynamo DB
    - **Fully managed and highly available across 3 AZ**
	- ** Multi AZ**
    - No SQL
    - **Automatically Scales to massive workloads**
    - **Serverless**
    - Fast and consistent in performance
    - **Single didgit mili sec latency** - low latency retreival
    - Low cost and auto scaling
    - 100s of TB storage and millions of req/sec
    - Low cost and auto scaling capabilities
    - Integrated with IAM
    - **K/V DB**
    - **DynamoDB Accelerator (DynamoDB DAX)** is an in-memory cache for DynamoDB that reduces response times from milliseconds to microseconds.
	- **Global Tables** : Name of the DynamoDB replication capability that provides fast read \ write performance for globally deployed applications. 
	- DynamoDB global tables are ideal for massively scaled applications with globally dispersed users. 
	- Global tables provide automatic replication to AWS Regions world-wide. 
	- They enable you to deliver low-latency data access to your users no matter where they are located.

### RedShift
    - **OLAP**
    - **Scales to PB**
    - MPP (Massive || Processing)
    - Pay as u go
    - **Based on Postgres SQL**
    - **Load data once every hr (not every sec)**
    - 10 X better perf than other DBS
    - SQL interface
    - Bi tools like AWS QuickSight and Tableau integrate with it
    - Can be deployed at **AZ level** only

### EMR
    - Cluster with 100 of EC2
    - **Machine Learning / Big data/ data processing/web indexing**
    - EMR takes care of all provisioning and config
    - **Auto Scaling and integrated with Spot instances**

### Athena
    - **Query Data in S3**
    - **Pay/ query**
    - Output result back to S3
    - **Fully Serverless**
    - Secured through IAM
    - Use Cases : Log Analytics, 1 time SQL query , serverless queries queries on S3

### DMS
    - DB migration service
    - **Quickly and securely migrate DB to AWS**
	- Resilinet
	- Self-healing
    - Source DB remains available during migration
    - Spports homogenous and heterogeneous migration
	- The AWS Database Migration Service can migrate your data to and from the most widely used commercial and open-source databases.

---

## Glue
- **ETL Service**
- **Fully serverless**
- S3 Bucket - > Extract -> Glue -> Load -> RS
- Glue Data Catalog : Discover datasets and build proper schemas
- Glue catalog will have alert refernce

---

## Container Services

### ECS (Elastic Container Service)
- **Launch docker container** on AWS
- You must **provision and maintain infra (the EC2 instances)**
- Package and deploy app into any container and can be executed on any OS
- AWS takes care of stopping/starting container
- Has integration with **Application Load Balancer**

### Fargate
- Launch docker containers on AWS
- **No need to configure infrastructure**
- **Serverless**
- AWS runs containers on CPU/RAM u need

### ECR
- Storage for container
- Private docker registry
- Can store docker images

**Serverles == FaaS**

## Serverless Offerings
- AWS Lambda
- Lambda
- Fargate
- S3
- Athena
- Redshift
- DynamoDB

## AWS Lambda
- **Serverless**
- Only **virtual functions**
- Run on demand
- **Automated scaling**
- Shorter execution of time
- **Invocation time** - `upto 15 mins`
- **Regional**
- Can support any language using API
- Natively supports # of programming languages like Java, Python , NodeJS

Banefits :
- **Pay/request** or **Pay/compute time**
- Free tier of 10000000 AWS Lambda requests and 40,000 GBs of compute time
- **Event driven**
- **Monitoring through Cloudwatch**
- **Does not go with Docker**
- Easy to get more resources per function (upto 3GB RaM)
- Increasing RAM will improve CPU and n/w
- $0.2/million req thereafter
- $1 for 600000 GB-sec
- Very cheap
- Use Cases
    - Run serverless cron job
    - Create thumbnails from images

---

## AWS Batch
- Fully managed batch processing at any scale
- AWS Batch provisions right amount of compute/memory
- Will **dynamically launch EC2/spot instances**
- **Batch jobs are defined as docker images which run on EC2**
- Helpful for cost optimization and focus less on infra
- You submit/schedule the job and AWS batch does the rest
- Job has start and end time
  
---

## Lambda vs Batch

- Lambda
    - Time limit
    - Limited runtime
    - Limited Temporary Disk
    - Serverless

- Batch
    - No time limit
    - Any runtime
    - **Rely on EBS/instance store for disk space**
    - **Relies on EC2**    
---


## Amazon Lightsail

- Virtual servers, storage, DB , n/w
- Low and predictable pricing
- **High availability but NO autoscaling**
- Great for people with little cloud experience
- Easiest way to launch and manage a Virtual Private Server (VPS) in the AWS Cloud
- Use cases
    - Simple web app
    - websites
    - Dev/Test env
    
---

## Cloud Formation

- **AWS only**
- **Infra as code**
- No resources are manually created
- **Infra focussed and not migration**
- Declerative way of outlining your AWS infra for any resource
- Creates resources in right order
- **Changes through infra are tracked via code**
- **You can estimate the cost using CloudFormation template**
- Repeat across regions and accounts

### Cost
    - Each resource within the stack is tagged with a identifier so that you can easly see how much stack costs you
    - Saving strategy : In dev , you can automation deletion of templates at `5 pm` and recreate at `8 am`

### Productivity
    - Ability to recreate and destory resouces on cloud on the fly
    - Automated generation of diagrams for ur template
    - Declerative Programming - No need to figure out ordering and orchestration

### Dont re-invent wheel
    - Leverage existing documenation
    - Levergae existing templates

### Suppport
    - Support all resources
    - You can use custom resources for the resources that are not supported
	- **Can be used to deploy to many AWS regions and accounts**

---

## Elastic BeanStalk

- **AWS Only**
- Developer centric view of deploying an app in AWS
- All services are in one view
- **We still have full control over config**
- **PaaS**
- **Free but need to pay for underlying services**
- Use all the components seen in material
- App/platform focussed
- Use Cloud formation underneath
- Managed Service
    - **Instance config/OS is handled by Beanstalk**
    - **Deployment strategy is configurable but performed by Beanstalk**
- Just the app code is responsibility of developer
- Deploy code consistently with known architecure
- AWS Elastic Beanstalk is the fastest and simplest way to get web applications up and running on AWS
- Developers simply upload their application code and the service automatically handles all the details such as resource provisioning, load balancing, auto-scaling, and monitoring
- Elastic Beanstalk is an end-to-end application platform, unlike CodeDeploy, which is targeted at code deployment automation for any environment (Development, Testing, Production). 
- It cannot be used to automatically deploy code to an Amazon EC2 instance.

### 3 Architectural modes
    Single instance deployment : dev
    LB + ASG : prod or pre-prod
    ASG only : Great for Non web app in prod(workers etc)

### Platforms
- Single container docker
- Multi container docker
- Preconfigured docker
- Go, Java SE, Python, Node JS, PHP, Ruby , Packer builder,. NET
- If not supported , you can write ur custom platform(advanced)

---

## AWS Code Deploy

- Deploy and upgrade app automatically
- Works with EC2
- Works with on-prem
- **Hybrid**
- Services/Instances must be provisioned ahead of time with **CodeDeploy agent**
- AWS CodeDeploy is a service that automates application deployments to a variety of compute services including Amazon EC2, AWS Fargate, AWS Lambda, and on-premises instances. 
- CodeDeploy fully automates your application deployments eliminating the need for manual operations.
- CodeDeploy protects your application from downtime during deployments through rolling updates and deployment health tracking.

---

## AWS Systems Manager (Patch fleet)

- **Patch, configure and run commands** as scale
- Manages EC2 and on-prem systems at scale
- **Hybrid AWS** Service
- **Get operational insight about the state of your infrastructure**
- Suite of 10 + products
- **Windows + Linux**
- Important features
    - **Patching automation for enhanced compliance**
    - **Run commands across entire fleet**
    - **Stores param config with Param store**
- Need to install `SSM agent` onto systems we control
- **Installed by default on Amazon Linux AMI**
- SSM agent run commands, patch and configure servers

---

## OpsWorks

- **Hybrid**
- **Chef and Puppet** perform server config automatically or repetitive actions
- Work gr8 with EC2 and on prem
- OpsWorks = Managed chef + Puppet
- **Alternative to SSM**
- **Only provision std resources**
    - EC2 instance, DB , Volume, LB
    
---

## Global Content Delivery N/W

### Route 53
- **Route users to closest deployment** wid least latency
- Great for **Disaster recovery**
- **Managed DNS**
- DNS is collection of rules/records which helps client determine how to reach server
- **Domain Name Registeration**
- You can also use geolocation routing to restrict the distribution of content to only the locations in which you have distribution rights
- You can use Route 53 geolocation routing policy to block certain geographies.
- Provides health check and monitoring
- Does not do ip routing
- Policies
    - Simple Routing Policy
          - **No health checks**
          - Web browser will query to 53 and get IP
    - Weighted
          - Allows to distribute traffic to **multiple AZ**
          - &&Weights are assigned to instances** and accordingly traffic is routed
    - Latency
          - If user is close then request will be routed to that server
    - Failover
          - Will check health of primary server. If its unhealthy , then request will be routed to secondary server

### CloudFront
- **Replicate part of your application to edge loc**
- Decreases latency
- **Cache Common requests**
- Improves performance for ur cachable content
- **Content is served at edge location**
- **Content Delivery N/W**
- Improves read perf
- Content is cached at edge . Inc perf
- 216 edge loc
- **DDos protection since integration with shield, WAF**
- **Global edge n/w**
- Files are cached for later
- **Great for static content**
- Origin                       
    - S3 bucket
          - For Distributed file and caching at edge loc
          - Enhanced security wid CloudFront Origin Access Identity (OAI)
          - CloudFront can be used as ingress (to upload file to S3)
- Custom Origin
    - App Load Balancer
    - EC2 instance
    - S3 website(must first enable bucket as a static S3 website)
    - Any HTTP backend
- S3 CRR
    - Must be set up for each region
    - Files are updated in real time
    - Read only
    - Great for dynamic content that needs to be available at low latency in few regions

### S3 Accelerate xfer
- Increases transfer speed by transferring file to AWS edge location, which will forward data to S3 bucket in target region
- **Accelerates global upload/download on AWS S3**
- Transfer files from all regions to a bucket
- File -> Edge Loc -> S3 Bucket
   
### AWS Global Accelerator
- Improves global app availability and performance using AWS global n/w
- **No caching**, proxy packets at the edge to application running in 1/more AWS regions
- Improves perf over wide range of apps over TCP or UDP
- Good for **HTTP use cases that requires static IP address**
- Good for **HTTP use cases that requires deterministic , fast regional failover**
- Leverage AWS n/w to optimize route to app (60 % improvement)
- `2 Anycast ip` are created for app and traffic is sent thru edge loc
- **Edge loc send the traffic to app**

### CloudFront vs AWS Global accelerator
- Both uses AWS global n/w and edge location
- Both integrate with AWS Shield for DDoS protection

---

## Cloud Integration

Communication
(i) Sync - App to app
(ii) Async :

### SQS
App -> SQS -> App
- **Queue model**
- Oldest AWS offering
- Queue sevice 
- Fully managed service (serverless)
- Sends 1 message/sec to 10000 message/sec
- **Rentention** : `4 days` , Max : `14 days`
- `No limit` on messages in queue
- Messages are deleted after they are read by consumers
- Low latency ( < 10 ms on publish and receive)
- Consumers share the work to read messages and scale horizontally
- **Multiple producers**
- **Multiple consumers** shares/deleted messages when done
- **Decouple applications in aws**
- Uses `PULL` mechanism

### SNS Overview
- **Pub/sub model**
- 1 message to many receivers
- **Notification service** 
- **Multiple subscribers** , send all messges
- Event publisher **only sends message to one SNS topic**
- As many event subscribers to listen to SNS topic notification
- Each subscriber will get the message
- Each SNS subscriber topic can have upto 1000000 sbs/topic, 100000 topic limit
- Uses `PUSH` mechanism
- **No message retention**

- Subscribers
    - Subscribers : email , lambda , SQS, HTTP ,mobile
    - HTTP/HTTPS(with delivery tries , how many times)
    - SQS Queue(Fan-out pattern), Lambda(write ur own integration)
---

## CloudWatch

- **Provides metrics** for all the services
- Metrics is variable to monitor
- Metrics have timestamps
- Can create cloudwatch dashboards of metrics
- **Sends alert when usage goes less**

### CloudWatch Metrics :
  - EC2 instance:
               - CPU utilization
               - Status check
               - Network (not RAM)
               - Default metric is every 5 mins
               - Option for detailed monitoring ($$$), metrices every 1 min.
  - EBS Volume: Disk read / writes
       - S3 buckets:
               -BucketSizeBytes,NumberOfObjects, AllRequests
       - Billing:
               - Total estimated charge (only in us-east-1)
       - Service limit: how much you've been using a service API
       - Custom metrics: Push your own metrics
       
### CloudWatch Alarms
- **Trigger notification for any metric**
- **Alarm** actions
    - AutoScaling : Increase or decrease EC2 instances "desired" count
    - EC2 Actions : Stop, terminate , reboot or recover EC2 instance
    - SNS notifications : **Sends notification to SNS topic**
- Various options ( samping , % max , min, etc)
- Can **choose a period on which to evaluate an alarm**
- Alarm staes : OK, INSUFFICIENT_DATA ALARM

### CloudWatch Logs
- Can collect logs from :
    - Elastic Beanstalk
    - ECS
    - AWS Lambda
    - CloudTrail based on filter
    - CloudWatch Log agents : on EC2 machine or onprem servers
    - Roue53 : Log DNS queries
- Enables **Realtime monitoring of logs**
- Adjustable CloudWatch Logs alarm
- By deafult , no logs will be sent to Cloudwatch
- You need to **Run a cloudwatch agent to push log files you want**
- **Cloudwatch log agents can be set up on prem too**

### CloudWatch Events
- Schedule : Cron jobs
    - Schedule every hr
- Event Pattern : Event rules to react to a service doing something
    Ex. IAM root user sign in event -> SNS topic wid email notiications
- Trigger Lambda fns , send SQS/SNS messages

### EventBridge
- Next evolution of CloudWatch Events
- Default event bus : generated by AWS services (Cloudwatch events)
- Partner event bus : receive events from saaS service or applications (Zendesk, Datadog,Segment, Auth0)
- Custom event bus : For your own app
- Schema Registery : Model event schema

## CloudTrail
- Provides **guarantee, compliance and audit** of ur a/c
- **Enabled by default**
- Get an **history of API calls** made within ur AWS a/c by :
    - Console
    - Service
    - CLI
    - SDK
- Can put CloudTrail logs in S3 or CloudWatch Logs
- Trail can be applied to **all regions/single region**
- **If a resources is deleted , start investigation by CloudTrail logs**

## X-Ray
- **Debugging in prod**
- Visual analysis of app
- **Troubleshooting/perf
- Understand dependencies in microservice architecture
- **Pinpoint service issues**
- Review request behaviours
- **Find error and exceptions**
- SLA identification, find where is throttle
- **Identify users that are impacted**
- Trace requests made through distributed apps

## Service Health Dashboard
- Shows **all regions and all services health**
- Shows **historical info for each day**
- **RSS feed** that can be subscribed to
- Gives **general status** of AWS services

### Personal Health Dashboard
- Provides **alert and remediation guidelines** when AWS is experience events that may impact u
- **Personalised view into performance and availability of the AWS services** underlying ur AWS resource
- Dashboard displays **relevant and timely info to help u manage events in progress** 
- Provides **proactive notification** to help you plan for scheduled events

## CloudTrail vs CloudWatch vs Config
- CloudTrail : Think account specific activity and audit
- CloudWatch : Think resource performance monitoring, events, and alerts; think CloudWatch
- Config : Think resource specific change history, audit and compliance
          
---

## VPC and Networking

### Virtual Private Cloud
- **Private n/w** to deploy resources
- **Regional(All AZ)**

### Subnets
- Allows you to **partition ur n/w** inside VPC
- **AZ LEVEL**
- Public subnet : Accessible from internet. eg : EC2, load balancer
- Private Subnet : Not accessible from internet . eg . Database

### Route Tables
- Define access to internet and between subnets

---

### Internet Gateway and NAT Gateway

#### Internet Gateway
- Helps VPC instances to connect to internet
- Public subnets have route to internet gateway

#### NAT Gateway and NAT Instances
- Allows instances in private subnet to access internet while remaining private
- **NAT Gateway** - AWS Managed
- **NAT Instance** - Self managed

---

## Network Security

### NACL (Network ACL)
- Firewall which controls traffic to and from subnet
- **Subnet level**
- **Allow and Deny**
- Are attached to subnet
- **Stateless : Return traffic must be explictly allowed**
- Automatically **applies to all instances in a subnet**
- Rules only include ip addresses
- Process rules in number of order when deciding whther to allow traffic

### Security Group   
- 2nd line of defense
- Firewall that controls traffic to/from an ENI/an EC2 instance
- **EC2 instance level**
- **Allow ONLY**
- **Stateful : Return traffic is automatically allowed**
- Rules include ip addresses , other security groups
- Evaluate all rules before deciding whether to allow traffic
- Apply to an instance only if someone specifies the sec grp when launching instance or associates the sec grp with the instances later on

---

## VPC Flow Logs

- Captures info of all the ip traffic going to interface
    - VPC Flow logs
    - Subnet flow logs
    - Elastic n/w interface flow logs
- Helps to monitor and troubleshoot connectivity issues
    - subnet to internet
    - subnet to subnet
    - internet to subnet
- Logs can be exported to **Cloudwatch/S3**
- Captures n/w info from AWS managed interfaces too : ELB, ElaticCache, RDS , aurora etc

# VPC Peering
- **Connect 2 VPCs privately using AWS n/w**
- Make them behave as same n/w
- Must not have overlapping CIDR ranges (IP Address range)
- **Connection is not transitive**

## VPC Endpoint
- Allows to **connect to AWS services using private net instead of pub n/w**
- Enhanced security and lower latency
- **VPC Endpoint Gateway** : S3 and Dynamo DB
- **VPC Endpoint Interface** : Rest

---    

## On Prem to VPC Connect

### Site to site VPN
- Connect an on-prem VPN to AWS
- Goes over **public n/w**
- **Connection is automatically encrypted**
- On prem must use **Customer Gateway (CGW)**
- AWS must use **Virtual Private Gateway (VGW)**

### Direct Connect (DX)
- Establish **physical connection betn on prem and AWS**
- **Connection is private**, secure and fast
- Goes over pvt n/w
- **Takes atleast a month to establish**
- Takes more time

### Transit Gateway
- For having **transitive peering betn 1000s of VPCs and On prem**
- Hub-and-Spoke (Star) connection
- Single gatewy to provide functioanlity
- Works with Direct Connect Gateway and VPC Connections

### Shared Controls in Shared Responsibility
Patch management, config management , awareness and training

---

## DDos(Distributed Denial of Service) Protection on AWS

### AWS Shield
- **Protects against DDOS attack** on website and app
- **Free service** for each customer
- Activated **by defaut**
- Provides protection layer from attacks such as SYN/UPD Floods, Reflection attacks ,other **layer 3(Routing protocal) and layer 4 attacks (TCP , UDP)**

### AWS Advanced Shield
- **24/7 Premium DDos protection**
- **Optional DDoS mitigation service ($3K/month/org)**
- Protect against sophisticated attack on EC2,ELB,Cloud Front,Global Accelerator, Route 53
- **24/7 access to AWS DDOS response team(DRP)**
- Protects against higher fees during usage spikes due to DDOS
- Provides intelligent DDos attack protection

### AWS WAF
- **Filter specfic req based on rules**
- **Protects your web app against web exploits (Layer 7) - HTTP**
- Deploy on **ALB, API Gateway, CloudFront**
- Define web ACLs
- Protects against common attacks - **SQL injection and cross site scripting**
- Size constraints, **geo match (block countries)**
- Rate based rules (to count occurrences of events) - for DDoS protection

### CloudFront and Route53
- Availability protection using global edge n/w
- **Combined with AWS Shield, provides attack mitigation at the edge**

---

## Penetration Testing on AWS
- AWS customers can carry out **security assessments or penetration tests against their AWS infra without prior approval** of 8 services :
- EC2 instances. NAT Gateway, ELB
- RDS
- CloudFront
- Aurora
- Lambda and Lambda Edge locations
- Lightsail resources
- Elastic Beanstalk env

### Prohibited Activities
- DNS zone walking via Route 53 Hosted zones
- Denial of Service, Distributed Denial of Service , Simulated DoS, Simulated DDoS
- Port flooding
- Request flooding (login request flooding, API request flooding)
- For simulated events - aws-security-simlated-event@amazon.com

---

## Encryption

### AWS KMS
- AWS manages s/w for encryption
- AWS manages encryption keys for us

##### Encryption-Opt in
- EBS volumes : encrypt volumes
- S3 buckets : SSE of ojects
- EFS drives : encryption of data
- Redshift DB : encryption of data
- RDS DB : encryption of data

##### Encryption default
- S3 Glacier
- Cloud Trail logs
- Storage Gateway

### AWS Cloud HSM
- **AWS provisioned encryption h/w**
- Dedicated h/w
- **You manage ur own ecryption key and not AWS**
- HSM client is temper resistant

## Key Types

### Customer Managed(CMK)
- Customer creates/manages ,can disable/enable
- Posibility of bring ur own key
- Possibility of rotation policy(new key generated and old key preserved)

### AWS Managed
- Managed by AWS
- Used by AWS services(S3, EBS, Redshift)

### Cloud HSM
- Keys generated from ur own CloudHSM h/w device
- Cryptographic ops are performed within Cloud HSM Cluster

## AWS Secrets Manager
- Meant for storing secrets
- Capability to force rotation of secrets every X days
- Automatic genertaion of secrets on rotation (uses Lambda)
- Secrets are encrypted using KMS
- Inegration with RDS (mySQL, Post Gres, Aurora)
- Mostly meant for RDS Integration

---

## Artifact(not really a service)

- Portal that provides customers with on demand access to **AWS compliance documentation and AWS agreements**
- Artifact Reports
    - Allows you to download AWS security and compliance documents, like AWS IO certifications, Payment Card Industry (PCI),ISO and System and Optimization (SOC) reports
- Artifact agreements
    - Allows you to review, accept, track status of AWS agreements such as Business Associate Addendum (BAA)
- Can be used to support internal audit/compliance

---

## Amazon Guard Duty
- Intelligent **Threat discovery**
- Uses ML algos, anomaly detection and 3rd party data
- **Input data is VPC logs, cloud trail logs and DNS logs**
- One click enable (30 days trial), no need to install s/w
- Input data includes
    - CloudTrail Logs : unusual API calls, unauthorized deployments
    - VPC Flow Logs : unusual internal traffic, unusual IP address
    - DNS Logs : Compromized EC2 instances semding encoded data within DNS queries
- **Can set up Cloudwatch Event rules to be notified in case of findings**
- CloudWatch event rules can target AWS Lambda or SNS

## Inspector
- Automated **security assessments for EC2**
- Analyse **running OS against known vulnerabilities**
- Analyse unintended n/w accessibility
- AWS Inspector agent must be installed on OS in EC2 instances
- After assessment , you get report with list of vulnerabilities
- Provide vulnerabities against best practices

## Config
- **Auditing and recording compliance of AWS resources**
- Records config and changes over time
- Possibility of **storing data in S3 (analyzed by Athena)**
- **Per region service**
- Can be aggregated across regions/accounts
- Questions that can be solved by Config
    - Is there unrestricted SSH access to my security groups
    - Do my buckets have public access
    - How has my ALB config chnaged over time ?
- **Paid service**

## Macie
- Fully managed data security and data privacy service
- Helps **identify and alert you to sensetive data, eg. PII**
- Uses pattern matching and ML to discover and protect ur insensetive data in AWS

---

## ML

### Rekognition
- Find objects ,people,text,scenes in images and video using ML
- **Facial analysis and facial search**  to do user verificaton , people counting
- Creates DB of familar faces or compare against celebrities
- **Regional**
- Use Cases
    - Labeling
    - Content Moderation
    - Text Detection
    - Face detection and analysis (gender, age, range, emotions)
    - Face search and verification
    - Celebrity Recognition
    - Pathing (for sports game analysis)

### Transcribe
- Convert **Speech to text**
- Use deep learning process c/a automatic search reognition to conver speech to text quickly and accurately
- Use Cases
    - Transcribe Customer Service Calls
    - Automate closed captioning and subtitling
    - Generate metadata for media assets to create fully serachable archive

## Polly
- Turn **Text to lifelike speech** using deep learning
- Allow you to create applications that talk

## Translate
- **Natural and accurate language translation**
- Allows you to localize content - such as websites and apps- for international users , and to easily translate large volums to text

## Lex and Connect
- Same tech as Alexa
- **Automatic speech recognition (ASR) to convert speech to text**
- Speech to text
- Natural Language Understanding to understand intent of text, callers
- **Chatbots and call center**

### Connect
- **Receive calls, create contact flows, cloud based Virtual contact Center**
- Can integrate wid other CRM systems or AWS
Phone call -> Connect --Stream-->Lex-->Lambda--> CRM

### Comprehend
- **NLP**
- **Fully managed and serverless**
- Use ML to find insights and relationships in text
    - Language of the text
    - Extract key phrases, places, brands , events
    - Understand how +ve or -ve the text is
    - Analyze text using tokenization and parts of speech
    - Automatically organizes collection of text files by topic

- Sample Use cases
    - Analyse customer interactions(emails) to find what leads to +ve/-ve experience
    - Create and groups articles by topics that Comprehend will cover

### SageMaker
- Fully managed service for developer to **build ML models**
   
---   
   
## AWS Organizations
- Global service
- **Manage multiple AWS a/cs**
- Main a/c is master a/c
- Cost Benefits
    - **Consolidated Billing across all accounts - single payment method**
    - Pricing benefit from aggregated usage (volume discount for EC2, S3)
    - **Pooling of Reserved EC2 instances for optimal savings**
- **API is available to automate AWS a/c creation**
- **Restrict account previleges using Service Control Policies(SCP)**
- AWS account must be able to operate as an standalone account .Only then it can be removed from Organizations

### Multi Account Strategies
- Create a/c per department, per env, per cost center, based on regulatory restrictions for better resource isolation, to have seperate /account service limits, isolate account for logging
- Use tagging standard for billing purpose
- Enable CloudTrail for all accounts
- Send logs to central S3 account
- Send CloudWatch Logs to central logging a/c

---

### Pricing models
    - Pay as u go : pay for what u use, remain agile, responsive, meet scale demands
    - Save when you reserve: minimize risks, predictably manage budgets, comply with long term requirements
          - Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElaticCache Resrved Nodes, RDS Reserved Instance, Redshift Reserved Nodes
    - Pay less by using more : Volume based discount
    - Pay less as AWS grows   
---

## Billing and Costing Tools

### Estimating cost

- TCO Calc
	- Reduce need to invest in large Capital Expense and provide Pay as u go model
    - Compare cost of app in **on-prem vs AWS**: Server , Storage, IT labour , N/W
    - Allows you to estimate cost savings when using AWS and provide detailed set of reports
	
- Simply monthly calc/Pricing calc
    - Estimate **cost of architectural solutions**
    - Replaced by AWS Pricing Calculator
    - Deprecated
	- Provides an estimate of usage charges for AWS services based on certain information you provide. 
	- It helps customers and prospects estimate their monthly AWS bill more efficiently.
	- This calculator provides a visual interface to enter details about the AWS services that you plan to use and then it outputs a detailed estimate of the monthly AWS bill. 

### Tracking Cost

- Cost and Usage Report
	- **Dive deeper into ur AWS a/c and usage**
    - Contains **most comprehensive set of AWS cost and usage data available , including additional metadata about AWS services, pricing and reservations**
    - List AWS usage for each service category used by account, its IAM users in hourly or daily line items as well as any tags that u have activated for cost allocation purposes
    - Can be integrated with Athena . Redshift , Quicksight
	- Allows you to see bill of past month
	- You can use Cost and Usage Reports to publish your AWS billing reports to an Amazon Simple Storage Service (Amazon S3) bucket that you own. 
	- You can receive reports that break down your costs by the hour or month, by product or product resource, or by tags that you define yourself.

- Cost Explorer
   - **Visualize understand and mange your AWS cost and usage over time**
   - Create custom reports that analyze cost and usage data
   - **Analyze ur data at a high level: total cost and usage across all accounts**
   - Or Monthly, hourly , resource level granularity
   - Choose an optimal savings plan
   - **Forcast usage upto 12 months based on prev usage**
   - Customers **can receive Savings Plan recommendations** at the member (linked) account level in addition to the existing AWS organization-level recommendations
   - Lets you explore your AWS costs and usage at both **a high level and at a detailed level of analysis**, and empowering you to dive deeper using several filtering dimensions (e.g., AWS Service, Region, Linked Account, etc.) 
   - AWS Cost Explorer also **gives you access to a set of default reports to help you get started, while also allowing you to create custom reports from scratch.**
		  
- Cost Allocation Tags
   - Use **cost allocation tags to track AWS cost at detailed level**
   - AWS generated tags
		- Automatically allowed to resource you create
		- Starts with prefix aws
   - User defined tags
        - Defined by user
        - Starts with prefix user
   - Tagging and Resource group
        - Tags are used for **organizing resources**
			- EC2
			- RDS, VPC resource,Route 53, IAM users
			- Resouces created by CloudFormation are tagged the same way
        - **Tags can be used to create Resource Groups**
			   
- Billing Dashboard
	- Provides recommendation on savings plan

### Monitoring against cost plans
    - Billing alarms
          - Billing data metric is stored in **us-east-1**
          - Billing data are for overall worldwide AWS costs
          - Its for **actual cost and not for projected cost**
          - Intended a simple alarm (not as powerful as AWS Budgets)
		  
    - Budgets
          - **Create budget and send alarms when Cost exceeds the budget**
          - 3 Types
               - Usage
               - Cost
               - Reservation
          - For Reserved Instance(RI)
               - Track utilization
               - Supports EC2, ElasticCache,RDS,RedShift
          - Can get upto `5 SNS notifications/budget`
          - Can filter by : Service , Linked a/c , Tag, Puschase Option, Instance Type, Region , AZ, API Operation
          - `2 budgets are free , then $0.02/day/budget`
          - Same options as AWS Cost Explorer 
		  - You can also use AWS Budgets to **set reservation utilization or coverage targets and receive alerts when your utilization drops below the threshold you define**
		  - AWS Budgets gives you the ability to set custom budgets that **alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.** 
		  - You can **define a utilization threshold and receive alerts when your RI usage falls below that threshold.**
		  - Reservation alerts are supported for Amazon EC2, Amazon RDS, Amazon Redshift, Amazon ElastiCache, and Amazon Elasticsearch reservations.         
 ---
 
## AWS Trusted Advisor
    No need to install anything -High level AWS account assessment
    - Account assessment
    - **Analyse accounts and provide recommendations**
    - **Core checks and recommendations - all customers**
    - Enable **weekly email notification from console**
	- AWS Trusted Advisor **can check Amazon Elastic Block Store (Amazon EBS) volume configurations and warns when volumes appear to be underused.**
	- Charges begin when a volume is created. If a volume remains unattached or has very low write activity (excluding boot volumes) for a period of time, the volume is probably not being used
    - Full Trusted advisor
          - Business and Enterprise support plans
          - Ability to **set cloudwatch alarms when reaching limits**
          - **Programmatic access using AWS Support APIs**

### Examples
    - **Cost Optimization**
          - Low utilization of Ec2 instances , idle LB , under utlized ELB
          - Reserved instance and savings plan optimizations
    - **Performance**
          - High utilization EC2 instances, CloudFront , CDN optimizations
          - EC2 to EBS thruput optimiations , Alias Record Recommendations
    - **Security**
          - MFA enabled on root, IAM key rotation , exposed Access Keys
          - S3 bucket permission for public access, security groups with unrestricted ports
    - **Fault Tolerance**
          - EBS snapshots age , AZ Balance
          - ASG Multi AZ ,RDS Multi AZ, ELB Config
    - **Service Limits**
          
---
  
AWS Ecosystem

## Support

- Personal Health Dashboard is available across all plans

### Basic

- **Free**
- **24 X 7 access to customer service , documentation , whitepapers and support forums**
- **AWS Trusted Advisor**
- Provides access to **7 core checks** from the AWS Trusted Advisor
- Guidance to provision resources following best practice to increase perf and improve security
- **AWS Personal Health dashboard** : personalized view of health servive and alers when the resources are impacted

### Developer
- Basic Support Plan +
- **Business hr email access to cloud support associates**
- **Unlimited cases/1  primary contact**
- General guidance < 24 hrs
- System impaired < 12 hrs
- Testing or doing early development on AWS and want the ability to get email-based technical support during business hours. 
- Supports general guidance on how services can be used for various use cases, workloads, or applications

### Business
- Intended to be used when you have **Prod workloads**
- **Trusted Advisor - Full set of check + API access**
- **24 X 7 phone, email chat access to Cloud Support Engineers**
- **Unlimited cases/ Unlimited contacts**
- **Access to Infrastructre Event management for additional fee**
- Case Severity/Response 
	- General guidance < 24 hrs
    - Sytem Impaired < 12 hrs
    - Prod system impaired - < 4 hrs
    - Prod system down - < 1 hrs
- Provides guidance ,configuration and troublshootng of **AWS interoperability with 3rd party software**  
- 24x7 phone, email and chat access to technical support and architectural guidance in the context of your specific use-cases.

### Enterprise

- All of Business Support Plan +
- **Mission critical workloads**
- Access to **Technical Account Manager (TAM)**
- **Conceirge support team (for billing/account best practices)**
- **Infrastructure Event management - Well - Archtected and Operations Reviews**
- Case Severity/response time
  - Prod system impaired : < 4 hrs
  - Prod system down : < 1 hr
  - Business-critical system down : < 15 mins
- Provides guidance ,configuration and troublshootng of** AWS interoperability with 3rd party software**  
- Provides **online training with self paced labs**

---

## Cognito
- Create a DB of user
- Identify ur Web and **Mobile app users (potentially millions)**
- Instead of creating IAM user , create a user in Cognito

---

## Directory Service

Integrate Microsoft AD in AWS

### AWS Managed Microsoft AD
    - Create ur own AD in AWS , manage user locally , supports MFA
    - Establish trust connection wid ur on prem AD

### AD Connector
    - Directory gateway (proxy) to redirect to your on prem AD
    - Users are managed on On prem AD

### Simple AD
    - AD compatible managed service on AWS
    - Cannot be joined with on-prem AD

---

## AWS SSO
- Centrally managed SSO to access multiplea/cs and 3rd party business app
- Integrated with AWS Organizations
- Supports SAML 2.0 markup
- Integration with on-prem AD

---

## AWS Architecting

- Stop guessing ur capacity needs
- Test system at prod scale
- Automate
- Allow for evolutionary architecture
- Drive architectues using data
- Improve thru game days
- Simulate app for flash sale days

5 pillars

### Operational Excellence
- **Run and monitor system to deliver business values and continually improve support procedures**
- Operations as code - **Infra as code**
- Annotate documentation - Automate creation of documents after every build
- Make freq small reversible changes
- Refine operation procedures frequently
- **Anticipate failures**
- **Automation**
- **Learn from all operational failures**

#### Prepare
- CloudFormation
- Config

#### Operate
- Cloud Formation , Run book prep,
- Config
  -
#### Scalability
- Disposable Resources
- Automation
- Loose coupling
- Services and not servers
eg. CloudTrail(Track API calls) , CloudWatch(Monitor perf of stack), X- Ray (Trace HTTP req)

#### Evolve
- CloudFormation
- CI/CD tool
- CodeBuild
- CodeCommit
- Code Deploy
- Code Pipeline

### Performance Efficiency
- Ability to **use computing resources efficiently to meet system requirements and to maintain that efficiency as demand changes**
- **Go global in minutes**
- **Serverless architecture**
- Mechanical sympathy - Be aware of all services
- **Experiment more often**
- Auto scaling , lambda
- Selecting the right resource types and sizes based on workload requirements, monitoring performance, and making informed decisions to maintain efficiency as business needs evolve

#### Selection
- Auto Scaling
- Lambda
- EBS
- S3
- RDS

#### Review
- CloudFormation

#### Monitoring
- CloudWatch
- Lambda

#### TradeOffs
- RDS
- ElasticCache
- Snowball
- CloudFront

### Security Excellence
Ability to **protect info, systems and assets thru risk assessment and mitigation strategies**
- Least Previlege access
- Enable Tracebility
- Apply security at all layers
- Automate
- Protect data in rest and transit - Encryption, Tokenization, Access Control
- Prepare for security event
eg.

#### Identity and Access Management
- IAM
- STS : To generate cred
- MFA
- AWS Organization
#### Detective Controls
- AWS Config
- CloudTrail
- CloudWatch
#### Infra Protection
- Shield
- VPC
- WAF
- Inspector
- CloudFront
#### Incidenet Response
- IAM
- CloudFormation
- CloudWatch Events

### Reliability
- **Ability of system to recover from service or infrastructure disruptions**
- **Dynamically acquire compute resources to meet demand and mitigate disruptions such as misconfig or transient n/w issues**

#### Design Principles
- Test Recovery procedures
- Stop guesssing capacity needs : Use auto scaling
- Scale horizontally : Distribute requests so that there is no common point of failre
- Manage chnage in automation : Use automation to make chnges in infra
- Automaticaaly tecover from failure

#### Foundation
- IAM
- VPC
- ServiceLimits
- Trusted Advisor

#### Change Management
- Auto Scaling
- CloudWatch
- CloudTrail
- Config
#### Failure Management
- CloudFormation
- S3
- Glacier
- Backups
- Route 53

### Cost Optimization
- Cost Optimization focuses on avoiding un-needed costs.
- Adopt consumption model
- Pay for what you use
- Stop spending money on data center operations
- Analyze and attribute expenditure  
- Make sure to use tags
- Use managed and app level services to reduce cost ownership
- Measure overall efficiency - Cloudwatch
eg-
#### Expenditure Awareness
  AWS Budgets
  Cost and Usage Report
  Cost Explorere
  Reserved Instance Reporting
#### Cost Effective resources
  - Spot Instance
  - Reserved Instance
  - Amazon S3 Glacier
#### Matching supply and demand
  - Auto Scaling
  - Lambda
#### Optimizing Over Time
  - Trusted Advisor
  - Cost and Usage Report

---

## Free resources
- AWS Blogs
- Forums
- Whitepaper and guides
- Quickstarts
	- Quick Starts are built by AWS solutions architects and partners to help you deploy popular technologies on AWS, based on AWS best practices for security and high availability. 
	- These accelerators reduce hundreds of manual procedures into just a few steps, so you can build your production environment quickly and start using it immediately.
	- Each Quick Start includes AWS CloudFormation templates that automate the deployment and a guide that discusses the architecture and provides step-by-step deployment instructions.
- Solutions

## AWS MarketPlace
- Digital catalog with 1000s of software listings from s/w vendors
- Buy and sell on AWS MarketPlce
- Example
  - Custom AMI
  - CloudFormation Templates
  - SaaS
  - Containers
- If you buy , it goes to AWS bill
The AWS Marketplace provides value to buyers in several ways:

1- It simplifies software licensing and procurement with flexible pricing options and multiple deployment methods. Flexible pricing options include free trial, hourly, monthly, annual, multi-year, and BYOL.

2- Customers can quickly launch pre-configured software with just a few clicks, and choose software solutions in AMI and SaaS formats, as well as other formats.

3- It ensures that products are scanned periodically for known vulnerabilities, malware, default passwords, and other security-related concerns.

## AWS Training
- Digital and Classroom
- Pvt Training (for ur org)
  - Training and cert for US govt
  - Training and cert for Enterprise
- AWS Academy
  - Help universities teach AWS
  
## Professional and Partner N/W
- Work alongside ur team and a chosen member of APN
- Leverage AWS Professional Services and set up AWS Landing Zone to accelerate the infrastructure migration

### Technology Partner 
- H/W ,connectivity , S/W

### Consulting Partner
- Professional services firm to help build on AWS

### Traiing Partner 
- Find who can help you learn AWS

### Competency Partner
- Given to APN partners who have demonstrated high Technical proficiency and proven customer success in specialized solution areas

### Navigate Program
- Help partner become better partner

## ABS Abuse Team
- AWS Abuse team can assist you when AWS resources are used to engage in following types of abusive behavior : 
      - Spam
      - Port Scanning
      - DoS Attacks
      - Hosting objectionable and copyrighted content
      - Distributing malware
---

## Free Services
- IAM
- CloudFormation
- ASG
- Beanstalk
- VPC
- Consolidated Billing
- Use private ip instead of public ip for good savings and better n/w perf
- Use same AZ for max savings

---

## Services that can b reserved
- Aurora
- RDS
- EC2

---
## Other Services - Misc 

### CodeCommit       
- Repository management service that allows for storing, versioining and managing app code

### CodePipeline
- Fully managed continuous delivey service that helps you to automate and release pipelines for fast and reliable application infra updates

### SWF (Amazon Simple Workflow Service )
- Web service that makes it easy to coordinate work across distributed application components. 
- Amazon SWF enables applications for a range of use cases, including media processing, web application back-ends, business process workflows, and analytics pipelines, to be designed as a coordination of tasks. 
- Tasks represent invocations of various processing steps in an application which can be performed by executable code, web service calls, human actions, and scripts. 
- The coordination of tasks involves managing execution dependencies, scheduling, and concurrency in accordance with the logical flow of the application.
- With Amazon SWF, developers get full control over implementing processing steps and coordinating the tasks that drive them, without worrying about underlying complexities such as tracking their progress and keeping their state.

### Application Migration Service
- AWS Application Discovery Service helps systems integrators quickly and reliably plan application migration projects by automatically identifying applications running in on-premises data centers, their associated dependencies, and their performance profiles. P
- Planning data center migrations can involve thousands of workloads that are often deeply interdependent. 
- Application discovery and dependency mapping are important early first steps in the migration process, but these tasks are difficult to perform at scale due to the lack of automated tools. 
- AWS Application Discovery Service automatically collects configuration and usage data from servers, storage, and networking equipment to develop a list of applications, how they perform, and how they are interdependent. This information helps reduce the complexity and time in planning your cloud migration.

## AWS Migration Hub
- AWS Migration Hub provides a single location to track the progress of application migrations across multiple AWS and partner solutions.

##  Amazon Cloud Directory
- Cloud-native, highly scalable, high-performance directory service that provides web-based directories to make it easy for you to organize and manage all your application resources such as users, groups, locations, devices, and policies, and the rich relationships between them.
- Unlike existing traditional directory systems, Cloud Directory does not limit organizing directory objects in a single fixed hierarchy. 
- In Cloud Directory, you can organize directory objects into multiple hierarchies to support multiple organizational pivots and relationships across directory information. For example, a directory of users may provide a hierarchical view based on reporting structure, location, and project affiliation. Similarly, a directory of devices may have multiple hierarchical views based on its manufacturer, current owner, and physical location. With Cloud Directory, you can create directories for a variety of use cases, such as organizational charts, course catalogs, and device registries.


## Security Bulletins
- AWS publishes security bulletins about the latest security and privacy events with AWS services on the Security Bulletins page.

## Other Notes 
- Per AWS Best Practices, proximity to your end users, regulatory compliance, data residency constraints, and cost are all factors you have to consider when choosing the most suitable AWS Region.
- Account Owner : If there is a requirement to grant a DevOps team full administrative access to all resources in an AWS account, then Account owner can give
- AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources.
- With the standard storage class you pay a per GB/month storage fee, and data transfer out of S3. oStandard-IA and One Zone-IA have a minimum capacity charge per object. Standard-IA, One Zone-IA, and Glacier also have a retrieval fee. You don’t pay for data into S3 under any storage class.
- A cloud practitioner needs to decrease application latency and increase performance for globally distributed users, then use CloudFront and S3
- AWS Lambda and Amazon API Gateway are both app-facing components of the AWS Serverless infrastructure
- AWS Step Functions is an orchestration service
- Which AWS service lets connected devices easily and securely interact with cloud applications and other devices : IoT Core
- AWS Shield Advanced provides enhanced detection and includes a specialized support team for customers on Enterprise or Business support plans. The AWS DDoS Response Team (DRT) are available 24/7 and can be engaged before, during, or after a DDoS attack.
- Amazon Elasticsearch Service is involved with operational analytics such as application monitoring, log analytics and clickstream analytics. Amazon Elasticsearch Service allows you to search, explore, filter, aggregate, and visualize your data in near real-time.
- Which AWS service or feature helps restrict the AWS service, resources, and individual API actions the users and roles in each member account can access - AWS Org
SCPs are used to restrict access within member accounts. For instance you can create an SCP that restricts a specific API action such as deploying a particular Amazon EC2 instance type. The policy would then prevent anyone, including administrators, from being able to launch EC2 instances using that instance type.
- AWS Simple Monthly calculator" is incorrect. AWS Simple Monthly calculator – shows you how much you would pay in AWS if you move your resources.
- Amazon CloudWatch Logs lets you monitor and troubleshoot your systems and applications using your existing system, application and custom log files. CloudWatch Logs can be used for real time application and system monitoring as well as long term log retention.
-  CloudTrail is used for logging who does what in AWS by recording API calls. It is used for auditing, not performance or system operational monitoring.
- A company has a static website hosted on an S3 bucket in an AWS Region in Asia. Although most of its users are in Asia, now it wants to drive growth globally. How can it improve the global performance of its static website, Use CloudFront
- AWS is responsible for setting up the software licenses used in their platform. AWS makes it is easy for you by partnering with vendors like Microsoft, IBM and other vendors to simplify running many commercial software packages on your EC2 instances. For some commercial software packages that AWS does not provide such as Oracle applications you still need to obtain a license directly from the vendors.
- Which of the following Cloud Computing deployment models eliminates the need to run and maintain physical data centers - Cloud
- Feature enables users to sign in to their AWS accounts with their existing corporate credentials - Federation
- The AWS Marketplace provides value to buyers in several ways:

1- It simplifies software licensing and procurement with flexible pricing options and multiple deployment methods. Flexible pricing options include free trial, hourly, monthly, annual, multi-year, and BYOL.

2- Customers can quickly launch pre-configured software with just a few clicks, and choose software solutions in AMI and SaaS formats, as well as other formats.

3- It ensures that products are scanned periodically for known vulnerabilities, malware, default passwords, and other security-related concerns.


To protect against data loss, you need to backup your database regularly. What is the most cost-effective storage option that provides immediate retrieval of your backups? S3

You manage a blog on AWS that has different environments: development, testing, and production. What can you use to create a custom console for each environment to view and manage your resources easily? Resource Group



  Anyone who has root user access keys for your AWS account has unrestricted access to all the resources in your account, including billing information. If you don't already have an access key for your AWS account root user, don't create one unless you absolutely need to. If you do have an access key for your AWS account root user, delete it. If you must keep it, rotate (change) the access key regularly.
  
  
          AWS doesn't charge usage for a stopped instance, or data transfer fees. For a stopped instance AWS will only charge you for EBS storage volumes attached to the instances.
		  
		  
		  
What does AWS Cost Explorer provide to help manage your AWS spend?

Forecasting capabilities have been enhanced to support twelve month forecasts (previously forecasts were limited to three months) for multiple cost metrics, including unblended and amortized costs.

The AWS tool that can provide accurate estimates of AWS service costs based on your expected usage is the AWS Simple Monthly Calculator
WS Cost Explorer forecasts your future costs based on your past usage; NOT based on your expected usage


Inbound Traffic is  NOT a factor when estimating the cost of Amazon CloudFront?
_Data Transfer Out
# and type of HTTP and HTTPS requests made
Edge loc thru which content is served

ACM : purchase and deploy SSL/TLS certificates


Which of the following factors should be considered when determining the region in which AWS Resources will be deployed? (Choose TWO)



Cost and



- You have just hired a skilled sys-admin to join your team. As usual, you have created a new IAM user for him to interact with AWS services. On his first day, you ask him to create snapshots of all existing Amazon EBS volumes and save them in a new Amazon S3 bucket. However, the new member reports back that he is unable to create neither EBS snapshots nor S3 buckets. What might prevent him from doing this simple task? Non explicity Deny to all users

Which AWS Service offers volume discounts based on usage? S3

Which of the following is NOT a factor when estimating the costs of Amazon EC2? (Choose TWO) Hosted Zones and Sec grp

 S
    

          