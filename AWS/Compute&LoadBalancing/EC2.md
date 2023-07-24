# EC2

## EC2 Instance Types - Main ones

- R: applications that needs a lot of RAM - in-memory caches
- C: applications that needs good CPU - compute / databases
- M: applications that are balanced (think "medium") - general / web app
- I: applications that need good local I/O (instance storage) - databases
- G: applications that need a GPU - video rendering / machine learning
- T2/T3: burstable instances(up to a capacity)
- T2/T3 - umlimited: umlimited burst

------

## EC2 - Placement Groups

- Control the EC2 Instance placement strategy using palcement groups
- Group Strategies:
  - _Cluster_ - clusters instances into a low-latency group in a single Availability Zone
  - _Spread_ - spreads instances across underlying hardware (max 7 instances per group per AZ) - critical applications
  - _Partition_ - spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
- **You can move** an instance into or out of a placement group
  - Your first need to **stop** it
  - You then need to use the CLI (modify-instance-placement)
  - You can then **start** your instance

## Placement Groups Cluster

- Pros: Great network ( 10 Gbps bandwidth between instances with Enhanced Networking enabled - recommended )
- Cons: If the rack fails, all instances fails at the same time
- Use Case:
  - Big Data job that needs to complete fast
  - Application that needs extremely low latency and high network throughput
- Up to 7 partitions per AZ
- Up to 100s of EC2 instances
- The instances in a partition do not share racks with the instances in the other partitions
- A partition failure can affect many EC2 but won't affect other partitions
- EC2 instances get access to the partition information as metadata
- _Use cases_: HDFS, HBase, Cassandra, Kafka
 
## Placement Groups Spread

- Pros:
  - Can span across Availability Zones (AZ)
  - Reduced risk is simultaneous failure
  - EC2 Instances are on different physical hardware
- Cons:
  - Limited to 7 instances per AZ per placement group
- Use case:
  - Application that needs to maximize high availability
  - Critical Applications where each instance must be isolated from failure from each other
----

## EC2 Instance Launch Types

- **On Demand Instances**: short workload, predictable pricing, reliable
- **Stop Instances**: short workloads, for cheap, can lose instances (not reliable)
- **Reserved**: (MINIMUM 1 year)
  - **Reserved Instances**: long workloads
  - **Convertible Reserved Instances**: long workloads with flexible instances
  - **Highest to lowest discount**: All Upfront payment, Partial Upfront payment, no Upfront
- **Dedicated Instances**: no other customers will share your hardware
- **Dedicated Hosts**: book an entire physical server, control instance placement
  - Great for software licenses that operate at the core, or CPU socket level
  - Can define **host affinity** so that instance reboots are kept on the same host


## EC2 Graviton

- AWS Graviton Processors deliver the best price performance
- Supports many Linux OS, Amazon Linux 2, RedHat, SUSE, Ubuntu
- Not available for Windows instances

- **Graviton2** - 40% better price performance over comparable 5th generation x86-based instances
- **Graviton3** - Up to 3x better performance compared to Graviton2
- Use cases: app servers, microservices, HPC, CPU-based ML, video encoding, gaming, in-memory caches ...

## EC2 included metrics

- _CPU_ : CPU Utilization + Credit Usage / Balance
- _Network_ : Network In/Out
- _Status Check_:
  - Instance status = check the EC2 VM
  - System status = check the underlying hardware
- _Disk_: Read/Write for Ops/Bytes (only for instance store)
- **RAM is NOT included in the AWS EC2 metrics**

## EC2 Instance Recovery

- _Status Check_:
  - Instance status = check the EC2 VM
  - System status = check the underlying hardware
  - **Recovery**: Same Private, Public, Elastic IP, metadata, placement group
  - 

---
 ENA, ENI, EFA 차이점을 물어보는 문제가 많이 나옴
## High Performance Computing (HPC)

- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- Perform genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving

### Then which services help perform HPC?
## Data Management & Transfer

- **AWS Direct Connect**:
  - Move GB/s of data to the cloud, over a private secure network
- **Snowball & Snowmobile**
  - Move PB of data to the cloud
- AWS DataSync
  - Move large amount of data between on-premises and S3, EFS, FSx for Windows

## Compute and Networking

- EC2 Instances:
  - CPU optimized, GPU optimized
  - Spot Instances / Spot Fleets for cost savings + Auto Scaling
- EC2 Placement Groups : **Cluster** for good network performance
- EC2 Enhanced Networking (SR-IOV)
  - Higher bandwidth, higher PPS (packet per second), lower latency
  - Option 1: **Elastic Network Adapter(ENA)** up to 100 Gbps
  - Option 2: Intel 82599 VF up to 10 Gbps - LEGACY
- **Elastic Fabric Adapter (EFA)**
  - Improved ENA for **HPC**, only works for **Linux**
  - Great for inter-node communications, **tightly coupled workloads**
  - Leverages Message Passing Interface (MPI) standard
  - Bypasses the underlying Linux OS to provide low-latency, reliable transport

## Storage

- Instance-attached storage:
  - **EBS**: scale up to 256,000 IOPS with io2 Block Express
  - **Instance Store**: scale to millions of IOPS, linked to EC2 instance, low latency

- Network storage:
  - **Amazon S3**: large blob, not a file system
  - **Amazon EFS**: scale IOPS based on total size, or use provisioned IOPS
  - **Amazon FSx for Lustre**:
    - HPC optimized distributed file system, millions of IOPS
    - Backed by S3
   
## Automation and Orchestration

- AWS Batch
  - **AWS Batch** supports multi-node parallel jobs, which enables you to run single jobs that span multiple **EC2** instances.
  - Easily schedule jobs and launch EC2 instances accordingly

- AWS ParallelCluster
  - Open source cluster management tool to deploy HPC on AWS
  - Configure with text files
  - Automate creation of VPC, Subnet, cluster type and instance types

## Auto Scaling Groups - Dynamic Scaling Policies

- **Target Tracking Scaling**
  - Most simple and easy to set-up
  - Example: I want the average ASG CPU to stay at around 40%
- **Simple / Step Scaling**
  - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
  - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
- Scheduled Actions
  - Anticipate a scaling based on known usage patterns
  - Example: increase the min capacity to 10 at 5 pm on Fridays

## Auto Scaling Groups - Predictive Scaling

- **Predictive scaling**: continuously forecast load and schedule scaling ahead

## Good metrics to scale on

- **CPUUtilization**: Average CPU utilization across your instances
- **RequestCountPerTarget**: to make sure the number of requests per EC2 instances is stable
- **Average Network In/Out**(if you're application is network bound)
- **Any custom metric**(that you push using CloudWatch)

## Auto Scaling - Good to know

- **Spot Fleet support** (mix on Spot and On-Demand instances)
- **Lifecycle Hooks**:
  - Perform actions before an instance is in service, or before it is terminated
  - Examples: cleanup, log extraction, special health checks
- To upgrade an AMI, must update the **launch configuration/template**
  - Then terminate instances manually (CloudFormation can help)
  - Or use EC2 Instance Refresh for Auto Scaling
 
## Auto Scaling - Instance Refresh

- Goal: update launch template and then re-creating all EC2 instances
- For this we can use the native feature of Instance Refresh
- Setting of minimum healthy percentage
- Specify warm-up time (how long until the instance is ready to use)

## Auto Scaling - Scaling Processes

- **Launch**: Add a new EC2 to the group, increasing the capacity
- **Terminate**: Removes an EC2 instance from the group, decreasing its capacity.
- **Healthcheck**: Checks the health of the instances
- **ReplaceUnhealthy**: Termiante unhealthy instances and re-create them
- **AZRebalance**: Balancer the number of EC2 instances across AZ
- **AlarmNotification**: Accept notification from CloudWatch
- **ScheduledActions**: Performs scheduled actions that you create.
- **AddToLoadBalancer**: Adds instances to the load balancer or target group
- **InstanceRefresh**: Perform an instance refresh

## Auto Scaling - Health Checks

- Health checks available:
  - **EC2 Status Checks**
  - **ELB Health Checks (HTTP)**
  - **Custom Health Checks** - send instance's health to an ASG using AWS CLI or AWS SDK (**set-instance-health**)
- ASG will launch a new instance after terminating an unhealthy one
- Make sure the health check is simple and checks the correct thing
