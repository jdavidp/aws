# AWS Certified Solutions Architect (CSA)

## S3 Basics

- Remember that S3 is object-based (i.e., allows you to upload files)
- Files can be from 0 bytes to 5 TB
- There is unlimited storage
- Files are stored in buckets
- S3 is a universal namespace. That is, names must be unique globally.
- S3 bucket URL format  = `https://s3-{region}.amazonaws.com/{bucketName}`
- S3 bucket URL example = `https://s3-eu-west-1.amazonaws.com/acloudguru`
- Read after write consistency for PUTS of new objects
- Eventual consistency for overwrite PUTS and DELETES (can take some time to propagate)
- S3 storage classes/tiers:
  - S3 (durable, immediately available, frequently accessed)
  - S3-IA (durable, immediately available, infrequently accessed)
  - S3 One Zone-IA (even cheaper than IA, but only in one AZ)
  - Glacier - archived data, where you can wait 3-5 hours before accessing
- Remember the core fundamentals of an S3 object
  - Key (name)
  - Value (data)
  - Version ID
  - Metadata
  - Subresources
    - ACL
    - Torrent
- Object-based storage only (for files)
- Not suitable to install an operating system on
- Successful uploads will generate a HTTP 200 status code
- NOTE: Read the S3 FAQs before taking the exam. It comes up A LOT!

## Create an S3 Bucket

- Buckets are a universal name space
- Upload an object to S3, receive a HTTP 200 code
- S3, S3-IA, S3 Reduced Redundancy storage
- Encryption
  - Client side encryption
  - Server side encryption
    - Server side encryption with Amazon S3 Managed Keys (SSE-S3)
    - Server side encryption with KMS (SSE-KMS)
    - Server side encryption with Customer Provided Keys (SSE-C)
- Control access to buckets using either a bucket ACL or using bucket policies
- BY DEFAULT, BUCKETS ARE PRIVATE AND ALL OBJECTS STORED INSIDE THEM ARE PRIVATE

## S3 Versioning

- Stores all versions of an object (including all writes and even if you delete an object)
- Great backup tool
- Once enabled, Versioning cannot be disabled, only suspended
- Integrates with Lifecycle rules
- Versioning's MFA Delete capability, which uses multi-factor authentication, can be used to provide an additional layer of security

## S3 Cross Region Replication

- Versioning must be enabled on both the source and destination buckets
- Regions must be unique
- Files in an existing bucket are not replicated automatically. All subsequent updated files will be replicated automatically.
- You cannot replicate to multiple buckets or use daisy chaining (at this time)
- Delete markers are replicated
- Deleting individual versions or delete markers will not be replicated
- Understand what Cross Region Replication is at a high level

## Storage Gateway

- File Gateway - For flat files, stored directly on S3
- Volume Gateway:
  - Stored Volumes - Entire dataset is stored on site and is asynchronously backed up to S3
  - Cached Volumes - Entire dataset is stored on S3 and the most frequently accessed data is cached on site
- Gateway Virtual Tape Library (VTL)
  - Used for backup and uses popular backup applications like NetBackup, Backup Exec, Veeam, etc.

## RAID Groups

- AWS does not recommend ever putting RAID 5's on EBS. So, if RAID 5 is in the possible answers, DO NOT SELECT IT!

## EBS vs Instance Store

- Instance Store Volumes are sometimes called Ephemeral Storage
- Instance Store Volumes cannot be stopped. If the underlying host fails, you will lose your data
- EBS backed instances can be stopped. You will not lose the data on this instance if it is stopped.
- You can reboot both, you will not lose your data
- By default, both ROOT volumes will be deleted on termination; however, with EBS volumes, you can tell AWS to keep the root device volume.

## ELB

- 3 Types of Load Balancers
  - Application Load Balancer
  - Network Load Balancer
  - Classic Load Balancer
- 504 Error means the gateway has timed out. This means that the application not responding within the idle timeout period.
  - Troubleshoot the application. Is it the Web Server or Database Server?
- If you need the IPv4 address of your end user, look for the X-Forwarded-For header
- Instances monitored by ELB are reported as: InService or OutOfService
- Health Checks check the instance health by talking to it
- Have their own DNS name. Your are never given an IP address.
- NOTE: Read the ELB FAQ for Classic Load Balancer

## CloudWatch

- Standard Monitoring = 5 minutes
- Detailed Monitoring = 1 minute
- Dashboards - Creates dashboards to see what is happening with your AWS environment
- Alarms - Allows you to set Alarms that notify you when particular thresholds are hit
- Events - CloudWatch Events help you to respond to state changes in your AWS resources
- Logs - CloudWatch Logs helps you to aggregate, monitor, and store logs

## Placement Groups

- If not specified on the exam, assume Clustered Placement Group
- Two types of Placement Groups
  - Clustered Placement Group
    - Grouping of instances within a single AZ
    - Recommended for applications that need low network latency, high network throughput, or both
  - Spread Placement Group
    - Group of instances each placed on distinct underlying hardware
    - Can span multiple AZ's

## Lambda

- Lambda scales out (not up) automatically
- Lambda functions are independent, 1 event = 1 function
- Lambda is serverless
- Know what services are serverless! (S3, API Gateway, Lambda, DynamoDB, etc)
- Lambda functions can trigger other Lambda functions, 1 event can = x functions if functions trigger other functions
- Know your triggers!
  - API Gateway
  - AWS IoT
  - CloudWatch Events
  - CloudWatch Logs
  - CodeCommit
  - Cognito Sync Trigger
  - DynamoDB
  - Kinesis
  - S3
  - SNS
  - SQS

## EC2 Instance Types

- Know the differences between:
  - On Demand
  - Spot
  - Reserved
  - Dedicated Hosts
- Remember with spot instances:
  - If you terminate the instance, you pay for the hour
  - If AWS terminates the spot instance, you get the hour it was terminated in for free
- Instance Types (mnemonic)
  - __F__ - FPGA
  - __I__ - IOPS
  - __G__ - Graphics
  - __H__ - High Disk Throughput
  - __T__ - Cheap general purpose (think T2 Micro)
  - ~
  - __D__ - Density
  - __R__ - RAM
  - ~
  - __M__ - Main choice for general purpose apps
  - __C__ - Compute
  - __P__ - Graphics (think Pics)
  - __X__ - Extreme memory

## EBS

- EBS consists of:
  - SSD, General Purpose - GP2 - (Up tp 10,000 IOPS)
  - SSD, Provisioned IOPS - IO1 - (More than 10,000 IOPS)
  - HDD, Throughput Optimized - ST1 - frequently access workloads
  - HDD, Cold - SC1 - less frequently accessed data
  - HDD, Magnetic - Standard - cheap, infrequently accessed storage
- You cannot mount 1 EBS volume to multiple EC2 instances. Instead, use EFS.
- Termination Protection is turned off by default, you must turn it on
- On an EBS-backed instances, the default action is for the root EBS volume to be deleted when the instance is terminated
- EBS-backed Root volumes can now be encrypted using AWS API or console, or you can use a third party tool (such as bit locker etc) to encrypt the root volume
- Additional volumes can also be encrypted

## Volumes vs Snapshots

- Volumes exist on EBS:
  - Virtual Hard Disk
- Snapshots exist on S3
- You can take a snapshot of a volume, this will store that volume on S3
- Snapshots are point-in-time copies of volumes
- Snapshots are incremental. This means that only the blocks that have changed since your last snapshot are moved to S3.
- If this is your first snapshot, it may take some time to create
- Snapshots of encrypted volumes are encrypted automatically
- Volumes restored from encrypted snapshots are encrypted automatically
- You can share snapshots, but only if they are unencrypted
  - These snapshots can be shared with other AWS accounts or made public
- To create a snapshot for Amazon EBS volumes that serve as root devices, you should stop the instance before taking the snapshot

## DNS 101

- ELB's do not have pre-defined IPv4 addresses, you resolve to them using a DNS name
- Understand the difference between an Alias Record and a CNAME
  - Alias Record acts sort of like a CNAME, except you can resolve individual AWS resources
- Given the choice, always choose and Alias Record over a CNAME (because you don't get charged)

## Route 53 Routing Policies

- Simple
  - The default routing policy when you create a new record set
  - Most commonly used when you have a single resource that performs a given function for your domain
  - Example: one web server that serves content for the http://acloud.guru website
- Weighted
  - Lets you split your traffic based on different weights assigned
  - Example: You can set 10% of your traffic to go to US-EAST-1 and 90% to go to EU-WEST-1
- Latency
  - Allows you to route your traffic based on the lowest network latency for your end user (i.e., which region will give them the fastest response time)
- Failover
  - Used when you want to create an active/passive set up
  - Route53 will monitor the health of your primary site using a health check
  - A health check monitors the health of your end points
  - Example: You may want your primary site to be in EU-WEST-2 and your secondary DR Site in AP-SOUTHEAST-2
- Geolocation
  - Lets you choose where your traffic will be sent based on the geographic location of your users (i.e., the location from which DNS queries originate)
  - Example: You might want all queries from Europe to be routed to a fleet of EC2 instances that are specifically configured for your European customers. These servers may have the local language of your European customers and all prices are displayed in Euros.

## Databases

- RDS - OLTP
  - SQL Server
  - MySQL
  - PostgreSQL
  - Oracle
  - Aurora
  - MariaDB
- DynamoDB - NoSQL
- Redshift - OLAP
  - Single Node (160 GB)
  - Multi-Node
    - Leader Node (manages client connections and receives queries)
    - Compute Node (store data and perform queries and computations). Up to 128 Compute Nodes.
- Elasticache - In Memory Caching
    - Memcached
    - Redis

## VPC Overview

- Think of a VPC as a logical datacenter in AWS
- Consists of IGWs (or Virtual Private Gateways), Route Tables, Network Access Control Lists, Subnets, and Security Groups
- 1 Subnet = 1 Availability Zone
- Security Groups = stateful
- NACLs = stateless
- NO TRANSITIVE PEERING
- The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use and cannot be assigned to an instance

## NAT Instances

- When creating a NAT instance, Disable Source/Destination Check on the instance
- NAT instances must be in a public subnet
- There must be a route out of the private subnet to the NAT instance in order for this to work
- The amount of traffic that NAT instances can support depends on the instance size. If you are bottlenecking, increase the instance size
- You can create high availability using Autoscaling Groups, multiple subnets in different AZs, and a script to automate failover
- They are behind a Security Group

## NAT Gateways

- Preferred by the enterprise
- Scale automatically up to 10Gbps
- No need to patch
- Not associated with security groups
- Automatically assigned a public IP address
- Remember to update your route tables
- No need to disable Source/Destination Checks
- More secure than a NAT instance

## Network ACLs

- Your VPC automatically comes with a default network ACL, and, by default, it allows all outbound and inbound traffic
- You can create custom network ACLs. By default, each custom network ACL denies all inbound and outbound traffic until you add rules
- Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL
- You can associate a network ACL with multiple subnets. However, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed
- A network ACL can span multiple AZs, a subnet cannot

## VPC Flow Logs

- You cannot enable flow logs for VPCs that are peered with your VPC unless the peer VPC is in your account
- You cannot tag a flow log
- After you've created a flow log, you cannot change its configuration; for example, you can't associate a different IAM role with the flow log
- Not all IP traffic is monitored
  - Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged
  - Traffic generated by a Windows instance for Amazon Windows license activation
  - Traffic to and from 169.254.169.254 for instance metadata
  - DHCP traffic
  - Traffic to the reserved IP address for the default VPC router

## NAT vs Bastions

- A NAT is used to provide internet traffic to EC2 instances in private subnets
- A Bastion is used to securely administer EC2 instances (using SSH or RDP) in private subnets. In Australia, they are called jump boxes

## SQS

- SQS is a distributed message queueing system
- Allows you to decouple the components of an application so that they are independent
- Pull-based, not push-based
- Standard Queues (default) - best effort ordering; message delivered at least once
- FIFO Queues (First In First Out) - ordering strictly preserved, message delivered once, no duplicates (e.g., good for banking transactions which need to happen in strict order)
- Messages are 256 KB in size
- Messages can be kept in the queue from 1 minute to 14 days. The default is 4 days.
- Visibility Timeout
  - The amount of time that the message is invisible in the SQS queue after a reader picks up that message. Provided the job is processed before the visibility timeout expires, the message will then be deleted from the queue. If the job is not processed within that time, the message will become visible again and another reader will process it. This could result in the same message being delivered twice.
  - Default is 30 seconds - increase if your task takes >30 seconds to complete
  - Max 12 hours
- Short Polling - returned immediately even if no messages are in the queue
- Long Polling - polls the queue periodically and only returns a response when a message is in the queue or the timeout is reached

## API Gateway

- Remember what API Gateway is at a high level
- API Gateway has caching capabilities to increase performance
- API Gateway is low cost and scales automatically
- You can throttle API Gateway to prevent attacks
- You can log results to CloudWatch
- If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway

## Kinesis 101

- Know the difference between Kinesis Streams and Kinesis Firehose. You will be given scenario questions and you must choose the most relevant service
- Understand what Kinesis Analytics is
