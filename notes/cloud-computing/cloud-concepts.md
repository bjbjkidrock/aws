## Cloud Computing
Cloud computing: on-demand delivery of compute, database storage, applications, and other IT resources through a cloud services platform via the internet with pay-as-you-go pricing. (Think of it as renting someone else's computer!)

Six advantages of cloud computing:
1. Trade capital expense for variable expense
	- not invested in data centers and servers before you know how you're going to use them 
	- paying only for actual consumption of resources
2. Benefit from massive economies of scale
	- Amazon has a larger purchasing power (they build their own servers!)
3. Not guessing about capacity
	- cloud can scale with business needs without a long-term contract
4. Increase speed and agility
	- it scales inifinitely with demand
	- no longer have to buy servers and rent data center space
5. Not spending money running and maintaining data centers
6. Go global in minutes
	- lower latency and better experience for customers

### Types of Cloud Computing: 
1. Infrastructure As A Service (IAAS): you manage a physical or virtual server; the data center provider will have no access to your server.
2. Platform As A Service (PAAS): someone else manages the underlying hardware and operating systems; you do not have to worry about security patching, updates, maintenance, etc. (ex: Elastic Beanstalk)
3. Software As A Service (SAAS): you only worry about the software, while someone else takes care of the data centers, servers, networks, storage, maintenance, patching, etc. (ex: Gmail)

### 3 Types of Cloud Computing Deloyments:
- Public Cloud (AWS, Azure, GCP)
- Hybrid (mixture of public and private)
- Private Cloud (On Premise); you manage it in your datacenter (Openstack, VMware)

### Core Services: 
- Compute 
	- EC2
	- Lambda
- Storage
	- S3: Simple Storage Service
	- Glacier	
- Databases
	- Relational Database Service (RDS)
	- DynamoDB (Non-relational Databases)
- Security, Identity, & Compliance
- AWS Cost Management
- Network
	- Route53
	- VPC

### AWS Global Infrastructure
- Availability Zone (AZ): one or more discrete data centers, each with redundant power, networking and connectivity, housed in separate facilities.
- Region: a geographical area in the world which consists of two or more Availability Zones.
- Edge location: endpoints for AWS used for caching content (typically this consists of CloudFront, Amazon's Content Delivery Network (CDN))

### Support Plans
- Basic
	- Access to community forums
- Developer
	- Technical support
	- Response time: 12-24 hours
	- Starts at $29/month
- Business
	- AWS Trusted Advisor
	- Response time: 1 hour
	- Starts at $100/month
- Enterprise
	- Technical Account Manager (TAM)
	- Response time: 15 minute
	- Starts at $15,000/month

## Identity Access Management (IAM)
- Global (you do not specify a region)
- Create a user or group (created globally)

### Access AWS platform in 3 ways:
- via the Console (console.aws.amazon.com)
- programmatically (using the command line)
- using the Software Developers Kit (SDK)

Root account: the email address you used to set up your AWS account.
	- has full administrator access
	- create a user for each individual within your organization
	- secure the root account using multi-factor authentication

Group: a place to store your users
	- users inherit all permissions that group uses
	- to set the permissions in a group, you must apply a policy to the group

Policy: consist of JSON (key value pairs)

## S3
S3: Simple Storage Service
	- A place to put your flat files (flat file: a file that doesn't change).
	- Object-based storage; allows you to upload files
	- Files 0 Bytes to 5 TB
	- There is unlimited storage
	- Files are sotred in Buckets (bucket must be a unique name globally)
	- S3 is a universal namespace (that is, names must be unique globally)
	- ex: https://s3-eu-west-1.amazonaws.com/acloudguru
	- Receive a HTTP 200 response code if upload was successful

S3 Object: think of Objects just as files.
	- Objects consist of:
		- Key: the name of the object
		- Value: the data, made up of a sequence of bytes
		- Version ID 
		- Metadata (data about the data you are storing)
		- Subresources
			- Access Control Lists
			- Torrent	
### Data Consistency for S3
1. Read after Write consistency 
	- PUTs of new Objects:
		- If you write a new file (putting a new file in S3) and read it immediately afterwards, you will be able to view the data immediately.
	- Overwrite PUTs (update an existing file, or delete a file)
		- When you read the file, you may get the older version
		- Changes to objects can take some time to propagate

### Features
- Tiered Storage Available
- Lifecycle Management
- Versioning
- Encryption
- Secure your data using Access Control Lists and Bucket Policies

### Storage Classes
- S3 Standard
	- stored redundantly across multiple devices in multiple facilities (designed to sustain the loss of 2 facilities concurrently)
- S3 IA (Infrequently Accessed)
	- data requires rapid access, for infrequent use; lower fee than S3, but you are charged a retrieval fee
- S3 One Zone IA
	- lower-cost option for infrequently accessed data, but do not require the multiple AZ data resilience
- S3 Intelligent Tiering
	- designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead
- S3 Glacier
	- secure, durable, and low-cost storage class for data archiving; retrieval times configurable from minutes to hours
- S3 Glacier Deep Archive
	- lowest-cost storage class, where a retrieval time of 12 hours is acceptable

### Pricing
- Storage
- Requests
- Storage Management Pricing
- Data Transfer Pricing
- Transfer Acceleration
	- enables transfers of files over long distances between end users and an S3 bucket; takes advantage of CloudFront 
- Cross Region Replication Pricing
	- replicates file to a secondary bucket so you have disaster recovery

### Created an S3 bucket: Tips
- Bucket names share a common name space; you cannot have the same bucket name as someone else
- When you view your buckets, you vie them globally but you can have buckets in individual regions
- You can replicate the contents of one bucket to another bucket automatically using Cross Region Replication
- You can change the storage class and encryption on the fly 
	- S3 Standard
	- S3 IA
	- S3 One Zone IA
	- S3 Intelligent Tiering	
	- S3 Glacier
	- S3 Glacier Deep Archive
- Transfer Acceleration
	- Upload file to an Edge location, then Amazon uses those backbone networks to upload the file to its location

### Create an S3 Website
- You can use bucket policies to make entire S3 buckets public
- You can use S3 to host static websites (such as .html; no database connections)
- S3 scales to meet your demand; useful for a large number of requests

## CloudFront
- A content delivery network (CDN) is a system of distributed servers (network) that deliver webpages and other web content to a user based on teh geographic locations of the user, the origin of the webpage, and a content delivery server.

### Terminology
- Edge Location: the location where content will be cached (more of these than there are regions). This is separate to an AWS Region/AZ. You can read and write to Edge Locations.
- Origin: the origin of all the files that the CDN will distribute. This can be an S3 Bucket, an EC2 instance, an Elastic Load Balancer, or Route53.
- Distribution: this is the name given the CDN which consists of a collection of Edge Locations.
	- Web Distribution (typically used for websites)
	- RTMP (used for media streaming)
- TTL: Objects are cached for the life of TTL (Time To Live) in seconds.

Example: we have our origin in London (this is our S3 bucket containing our files). The users query the file (do you [Edge Location] have a copy of this file?). The Edge Location will not have the file upon this first query, and there will be added latency for this user. The Edge Location will connect to the origin and download the file, then stream it to the user. When the second user queries the same file; that file is already cached at the Edge Location, so the second user doesn't have to download it from the origin. They can get it from an Edge Location nearest them.

You can clear your cached objects, but you will be charged for doing that.

## EC2: Elastic Compute Cloud
A virtual server (or servers) in the cloud.
	- Reduces the time to obtain and boot new server instances to minutes,
	  allowing you to quickly scale capacity (up and down) as your computing requirements change

### Pricing Models
**On Demand**
	- Allows you to pay a fixed rate by the hour (or by the second) with no commitment
**Reserved**
	- Provides a capacity reservation, and offer a significant discount on thehourly charge for an instance
	- Contract terms are 1 year or 3 year terms
**Spot**
	- Enables you to bid whatever price you want for instance capacity
	- Provides for even greater savings if your applicans have flexible start/end times
**Dedicated Hosts**
	- Physical EC2 server dedicated for your use
	- Helps reduce costs by allowing you to use your existing server-bound software licenses

### EC2 Instance Types
Fight Dr. McPxz 
F - FPGA
I - IOPS
G - Graphics
H - High Disk Throughput
T - Cheap general purpose (think T2 Micro)
D - Density
R - RAM
M - Main choice for general pupose apps
C - Compute
P - Graphics (think Pictures)
X - Extreme Memory
Z - Extreme Memory and CPU

### EBS
_A virtual disk in the cloud that the virtual servers run off._

Allows you to create storage volumes and attach them to EC3 instances. Once attached, you can create a file system on top of these volumes, run a database, or use them in any other way you would use a block device. 

Placed in an Availability Zone (same one as the EC2 instance), where they are automatically replicated for protection from the failure of a single component.

Types (of virutal disks in the cloud):
1. SSD
	- General Purpose SSD (GP2)
	- Provisioned IOPS SSD (IO1) 
		- Highest performance
2. Magnetic
	- Throughput Optimized HDD (ST1)
	- Cold HDD (SC1)
	- Magnetic - _Previous Generation_

## Provision an EC2 Instance
_Build web servers in the cloud._

It's not serverless, it's an actual server in the cloud.

### Common Ports (How Computers Communicate)
Linux = SSH
(Port 22)

Microsoft = Remote Desktop Protocol (RDP)
(Port 3389)

HTTP = Port 80
HTTPS = Port 443 

### Key Pair
How we log into our EC2 instances.

You can have many copies of the padlock, and a key to unlock it.
1. Public Keys
	- many keys can unlock the padlock
2. Private Keys 
	- only one key can unlock the padlock (this is what you use to connect to EC2)

### Security Group
Virtual firewalls in the cloud. You need to open ports in order to use them. 
To let everything in: 0.0.0.0/0
To let just one IP in: X.X.X.X/32

### Launch Instance
Download your private key. 

From the Terminal, navigate to Downloads and change the permissions for your private key:
`chmod 400 MyPrivateKey.pem`

Then SSH into your instance:
`ssh ec2-user@3.81.122.72` 

Note: `3.81.122.72` will be the IP address generated after you have launched your instance. You can find the key _IPv4 Public IP_ in the Description of your instance, then copy the IP address to your clipboard.

To become the root user:
`sudo su`

Update your EC2 instance with the latest security patches.
`yum update -y`

### Design for Failure
Have one instance in each availability zone.

## Command Line
SSH into your EC2 instance 
`ssh ec2-user@3.81.122.72` 

`aws configure` to configure credentials:
AWS Access Key ID: (from the _credentials.csv_ file)
Aws Secret Access Key: (from the _credentials.csv_ file) 
_Note: Someone can obtain control of your AWS account with this information._

aws [service] [make bucket] [bucket name]
`aws s3 mb s3://carissaallen` 

### .aws
Contains _config_ and _credentials_ files. 

### Roles
- More secure than using access key IDs and secret access keys
- Policies are effective immediately
- Can apply roles to EC2 instances anytime
- Roles are universal (no need to specify what region they are in, similar to users)

### Build a Web Server
Install Apache
`yum install httpd`

Start the Service
`service httpd start`

Navigate to:
`cd /var/www/html/`

Create an _index.html_ file to create your website. 

To view the web server, paste your IP address into the web browser. Voila!

## Load Balancers
- Application Load Balancers: Layer 7 Aware (Make Intelligent Decisions)
- Network Load Balancers: Extreme Performance/Static IP Addresses 
- Classic Load Balancers: Test & Dev, Keep Costs Low

#### ElastiCache
_A caching engine in the cloud for your most common queries; will take a big load off your production databases because they are querying ElastiCache instead of your production databases._

A web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud.
Improves performance of web applications by allowing you to retrieve information from fast, managed, in-memory cahches, instead of relying entirely on slower disk-based databases. 

You don't want web servers querying the same information over and over again. So it queries ElastiCache (holding most common queries) to improve performance.
 
Supports two open source in-memory caching engines:
	- Memchached
	- Redis

## Provision an RDS Instance
- Provision an RDS Instance
- Provision an Application Load Balancer
- Create a Target Group 
- Create an EC2 Instance
- Register the EC2 Instance to the Target Group
- Open MySQL Port to Web-DMZ SG
- Connect to DNS name of ALB
- Install Word Press
- Take a Snapshot

```
#!/bin/bash
yum install httpd php php-mysql -y
cd /var/www/html
echo "healthy" > healthy.html
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cp -r wordpress/* /var/www/html/
rm -rf wordpress
rm -rf latest.tar.gz
chmod -R 755 wp-content
chown -R apache:apache wp-content
service httpd start
chkconfig httpd on
```

1: Shebang provides path to our interpreter; interprets the commands and runs them at root level.
2: Installing Apache (turns our EC2 Instance into a web server), PHP, and PHP MySQL.
3: Change our directory to where our web server will be stored.
4: Create a file called _healthy.html_.
-: Installing Word Press (unzips, deletes, changes permissions on Apache so we can view) 

WordPress site: http://my-alb-1487840289.us-east-1.elb.amazonaws.com/index.php/2019/03/18/welcome/ 
(DNS name of the Application Load Balancer (ALB))

Snapshot of web server: You can go to your EC2 Instance to create an image under _Actions_. Takes a photograph of the server (hard driver) and stores that image in S3 so you can provision exact copies of that web server. You can do that behind an Auto Scaling Group (ASG). You can view this image in _AMIs_.

AMI = Amazon Machine Image

Web server is behind an Application Load Balancer, and we have two EC2 instances in multiple availability zones. _We have a fault tolerant website in the cloud._

## Domain Name System (DNS)
Works like a phonebook: comptuers use it to resolve domain names to IP Addresses.

### Route53
_This is Amazon's DNS Service._

- Named after Route66, the first interstate highway across the United States. DNS works on Port 53.
- You can use it to direct traffic around the world, and to register a domain name.
- Route53 is global (similar to IAM and S3).

### Elastic Beanstalk
_Allows you to provision your EC2 Instance, Security Groups, Application Load Balancers, etc. at the click of a button._ 

- Quickly deploy and manage applications in the AWS Cloud.
- Do not have to worry about the infrastructure for those applications.
- Simply upload your application, and Elastic Beanstalk will automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

_Grows out your infrastructure based on the code you provide it._ 

### Cloud Formation
_Helps you model and set up your Amazon Web Services resources._

- Spend less time managing AWS resources, more time focusing on your applications that run in AWS.
- Create a template that describes all the AWS resources that you want (e.g., EC2 instances or RDS DB instances), and CloudFormation provisions and configures those resources for you.
- Do not need to individually create and configure AWS resources (or worry about what is dependent on what). 

#### Elastic Beanstalk & CloudFormation
- Free services; however, the services they provision are _not_ free.
- Elastic Beanstalk is limited in what it can provision and is not programmable.
- CloudFormation can provision almost any AWS service and is completely programmable.

## Architecting for the Cloud: Best Practices
**_Read the whitepaper._**

### Traditional Computing vs. Cloud Computing
- IT Assets are available as provisioned resources
	- CloudFormation allows us to have templates (using JSON) to create EC2 Instances, S3 Buckets, almost anything inside the AWS ecosystem.
	- Global, Available, and Scalable Capacity.
	- Higher Level Managed Services (for Machine Learning).
	- Built-in Security (IAM, etc.)
	- Architecting for Cost (can architect your environment to be cost-efficient).
	- Operations on AWS.
- Traditional Copmuting
	- Get a purchase order, purchase physical servers, 3-5 year contract, the servers would need to be racked, connected to the networking gear, must install the operating systems, etc.  

### Design Principles

#### Scalability
**Scale Up**
	-  Increasing RAM or amount of CPU inside a virtual machine.
**Scale Out**
	- Add multiple virtual machines behind an application load balancer, for example.
		- Stateless Applications (Lambda)
		- Distribute Load to Multiple Nodes (e.g., multiple EC2 servers, and database replicas)
		- Stateless Components 
			- Do not need to remember the information.
		- Stateful Components 
			- Do not want to lose information; store in database or something stateful.
		- Implement Session Affinity
			- Sticky session: put a cookie in a user's browser so every time they visit that website the Application Load Balancer will detect that cookie and send them back to that same EC2 Instance. You're "stuck" to a particular EC2 Instance.
		- Distributed Processing
		- Implement Distributed Processing
			- Elastic map reduce; it allows you to have a whole bunch of different EC2 Instances, and they process large, complex data. You have thousands of instances to reduce the time to process that data.
		- Instantiating Compute Resources
			- Bootstrapping (you do not want to manually configure your EC2 Instances; we can use a bootstrap script to install updates, or Word Press, for example)
			- Golden Images (set up autoscaling; took an image of our configured EC2 Instance for resuse)
			- Containers
			- Hybrid (containers and EC2 Instances)

#### Infrastructure As Code
_Disposable Resources Instead of Fixed Servers._	
- CloudFormation

#### Automation
- Serverless Mangement and Deployment
	- Using code pipeline, code deploy, etc.
- Infrastructure Mangement and Deployment
	- AWS Elastic Beanstalk
	- Amazon EC2 auto recovery
	- AWS Systems Manager
	- Auto Scaling

- Alarms and Events
	- Amazon CloudWatch alarms	
	- Amazon CloudWatch events
		- A way of having your environment proactively respond to a change in the environment.
	- AWS Lambda scheduled events
	- AWS WAF security automations
		- WAF: Web Application Firewall
		- Can automatically respond to someone doing something to your site (e.g., SQL Injection)

#### Loose Coupling
- Well Defined Interfaces
	- Amazon API Gateway
		- Allows you to create your own APIs and expose them to the internet
- Service Discovery
	- Implement Service Discovery
		- If you have an EC2 Instance that needs to connect to an RDS instance using its DNS name with multiple AZ turned on, if the RDS instance fails, AWS will switch it to the other availability zone. Allows one component of AWS automatically discover another component of AWS.
- Asynchronous Integration
- Distributed Systems Best Practices
	- Graceful Failure in Practice
		- Example: If you have an S3 website and a page doesn't exist, you have an error.hmtl page to tell the users there has been a failure. Additionally, you have a mechanism to report this back to your system administrators. 

#### Services Not Servers
_Use serverless services as much as possible so that servers do not have to be managed._

"No server is easier to manage than no server." - Werner Vogels (CTO, Amazon)

#### Databases
Relational Databases (Aurora)
	- Scalability 
	- Will always have 6 copies of your data across 3+ availability zones
	- Anti-patterns: would not use Aurora if you do not have a need for joins or complex transactions; use No-SQL instead

Non-Relational Databases (DynamoDB)
	- Scalability
	- High availability, multiple AZ
	- Anti-patterns: requires joins or complex transactions, or you have large binary files

Data Warehouse (Redshift)
	- Scalability
	- High availability, multiple AZ  
	- Anti-patterns: not meant for Online Transaction Processing (OLTP)

Graph Databases (Amazon Neptune)
	- Scalability
	- High Availability

Managing Increasing Volumes of Data: Data Lake
An architectural approach that allows you to store massive amounts of data in a central location (like S3!) so it is readily available to be categorized, processed, analyzed, and consumed by diverse groups within your organization. 

Since data can be stored as-is, you do not have to convert it to a predefined schema, and you no longer need to know what questions to ask about your data beforehand.

Removing Single Points of Failure
	- Introducing Redundancy
	- Detect Failure
	- Durable Data Storage
	- Automated Multi-Data Center Resilience
	- Fault Isolation and Traditional Horizontal Scaling
	- Sharding

Optimize for Cost
	- Right Sizing
	- Elasticity 
		- Your application will expand or contract depending on usage
	- Take advantage of the variety of purchasing options (spot, reserverd, etc.)

Caching
	- Application Caching
		- Using ElastiCache
	- Edge Caching
		- CloudFront

Security
	- Use AWS Features for Defense in Depth
	- Share Security Responsibility with AWS	
		- You and AWS are each responsibile for certain things
	- Reduce Privileged Access (give developers _enough_ access to do their job)
	- Security as Code 
	- Real-Time Auditing
		- AWS Inspector and other security services

Quiz Answers:
- Bucket Policies are used to make entire buckets (like one hosting an S3 website) public.
- Objects stored in S3 are stored in multiple servers in multiple facilities across AWS.
- Availability Zones are distinct locations from within an AWS region that are engineered to be isolated from failures.
- Lightsail is AWS' Platform-as-a-Service offering.
- CloudFront content is cached in Edge Locations.
- As there are at least two Availability Zones (AZ) per AWS Region, there will always be more AZs than Regions.
- The collection of a CDN's Edge Locations is called a Distribution.
- The AWS Support levels are Basic, Developer, Business, and Enterprise.
- The Root account should have MFA enabled; you should always create individual users (the Root account should never be used for actual work); and groups should be used to grant permissions to the users you create.
- A Policy is the document used to grant permissions to users, groups, and roles.
- A Region is a distinct location within a geographic area designed to provide high availability to a specific geography.
- S3 Transfer Acceleration uses AWS' network of Edge Locations to more quickly get your data into AWS.
- The number of Edge Locations is greater than the number of Availability Zones, which is greater than the number of Regions. 
- Reserved instances are the most economical option for long-term workloads with predictable usage patterns.
- The two types of access are AWS Management Console access and Programmatic Access via the AWS API, the CLI, and the SDKs.
- To restrict access to an entire bucket, you use bucket policies; and to restrict access to an individual object, you use access control lists.
- A CloudFront Origin can be an S3 bucket, an EC2 instance, an Elastic Load Balancer, or Route 53.

 



















 














































































































 
