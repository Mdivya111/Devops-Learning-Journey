# AWS EC2

## What I Learned

Amazon EC2 (Elastic Compute Cloud) provides resizable virtual servers in the AWS cloud.

EC2 is one of the main AWS services used to run applications, web servers, automation tools, and DevOps workloads.

## Topics Covered

- What is EC2
- EC2 Instance
- Instance Type
- AMI
- Key Pair
- EBS
- Elastic IP
- Launching an EC2 instance
- Connecting to an EC2 instance
- IAM Role with EC2
- Basic EC2 configuration
- EC2 security concepts

## Key Concepts

### EC2 Instance

An EC2 instance is a virtual server running in AWS.

It provides compute resources such as CPU, memory, networking, and storage.

### Instance Type

The instance type determines the resources available to an EC2 instance, including:

- vCPU
- Memory
- Network performance
- Instance capabilities

Common instance families include:

- T — Burstable general-purpose workloads
- M — General-purpose workloads
- C — Compute-optimized workloads
- R — Memory-optimized workloads

### AMI

An AMI (Amazon Machine Image) is a template used to launch EC2 instances.

It can contain the operating system, software, configuration, and storage information required to create an instance.

### EBS

Amazon EBS (Elastic Block Store) provides persistent block storage for EC2 instances.

The root EBS volume normally contains the operating system, while additional EBS volumes can be used for application data.

### Key Pair

A key pair is used to securely connect to an EC2 instance.

The public key is associated with the instance, while the private key is kept by the user.

### Elastic IP

An Elastic IP is a static public IPv4 address that can be associated with an EC2 instance.

It can be moved between instances when required.

### IAM Role with EC2

An IAM role can be attached to an EC2 instance to provide AWS permissions without storing long-term AWS access keys on the server.

For example:

**EC2 → IAM Role → AWS Service**

This is commonly used when an EC2 instance needs to access services such as Amazon S3.

## Hands-On Practice

- Launched EC2 instances using different configurations
- Worked with Amazon Linux and Ubuntu instances
- Created and used AMIs
- Launched an instance from a custom AMI
- Worked with EBS volumes and snapshots
- Practiced attaching and mounting EBS volumes
- Allocated and associated an Elastic IP
- Practiced stopping and restarting instances
- Worked with EC2 security configuration
- Practiced connecting to EC2 instances
- Worked with IAM roles for EC2

## DevOps Relevance

EC2 is commonly used by DevOps engineers for:

- Hosting applications
- Running web servers
- Building CI/CD environments
- Running automation tools
- Hosting monitoring tools
- Running Docker workloads
- Creating development and testing environments
- Deploying applications

EC2 can also be combined with other AWS services such as:

**EC2 + EBS + IAM + Load Balancer + Auto Scaling**

to build scalable and highly available applications.

## Cost Awareness

EC2 resources can generate AWS charges depending on the instance type, storage, public IP resources, and usage.

For learning purposes, I prioritize Free Tier-eligible resources and terminate or release resources after practical exercises when they are no longer required.
