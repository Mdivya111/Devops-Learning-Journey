# AWS AMI

## What I Learned

AMI (Amazon Machine Image) is a template used to launch EC2 instances.

An AMI can contain the operating system, installed software, configuration, and information about the storage volumes required to launch an EC2 instance.

## Topics Covered

- What is an AMI
- AMI and EC2 relationship
- Creating a custom AMI
- Launching an EC2 instance from an AMI
- AMI vs EBS Snapshot
- Copying an AMI to another AWS Region
- Sharing an AMI
- Deregistering an AMI
- AMI and EBS relationship

## Key Concepts

### AMI

An AMI is a reusable blueprint for launching EC2 instances.

It helps create multiple EC2 instances with the same operating system, software, and configuration.

### Why Use an AMI?

Custom AMIs can be used to:

- Standardize server configurations
- Quickly launch new instances
- Create identical servers
- Replace failed instances
- Support application scaling
- Maintain a known-good server configuration

### AMI and EC2

The relationship can be represented as:

**AMI → Launch → EC2 Instance**

An AMI is the template, while an EC2 instance is the running virtual server created from that template.

### AMI and EBS

For EBS-backed EC2 instances, creating an AMI involves creating snapshots of the EBS volumes included in the image.

When an EC2 instance is launched from the AMI, new EBS volumes can be created from those snapshots.

The relationship is:

**AMI → EBS Snapshots → EBS Volumes → EC2 Instance**

### AMI vs EBS Snapshot

| AMI | EBS Snapshot |
|---|---|
| Used to launch EC2 instances | Used to back up an EBS volume |
| Contains information required for an instance image | Point-in-time backup of an EBS volume |
| Can include multiple storage volumes | Represents a specific EBS volume |
| Used as an EC2 launch template | Used for backup and recovery |

### Copying an AMI

An AMI can be copied to another AWS Region.

The copied AMI becomes an independent AMI in the destination Region.

This can be useful for:

- Disaster recovery
- Regional deployments
- Launching instances in another Region

### Sharing an AMI

An AMI can be shared with another AWS account.

The original owner continues to own the AMI, while the other account can use the shared AMI to launch instances according to the sharing configuration.

### Deregistering an AMI

Deregistering an AMI removes the AMI from the list of available images.

For EBS-backed AMIs, the underlying snapshots may still exist and may need to be cleaned up separately.

## Hands-On Practice

- Created a custom AMI from an EC2 instance
- Launched an EC2 instance using the custom AMI
- Verified that the new instance was created from the AMI
- Copied a custom AMI from one AWS Region to another Region
- Observed the relationship between AMIs, EBS snapshots, and EBS volumes
- Reviewed AMI sharing concepts
- Practiced AMI lifecycle and cleanup concepts

## DevOps Relevance

AMIs are useful in DevOps for creating standardized server environments.

They can be used for:

- Automated infrastructure provisioning
- Consistent server deployments
- Application environments
- Disaster recovery
- Auto Scaling
- Creating identical EC2 instances
- Maintaining standardized machine configurations

A common workflow is:

**Configure EC2 → Create AMI → Use AMI to Launch Multiple EC2 Instances**

## Cost Awareness

Creating and storing AMIs can involve storage costs because the underlying EBS snapshots consume storage.

For learning purposes, unnecessary AMIs and associated snapshots should be cleaned up after the practical work is completed.
