# AWS Elastic IP

## What I Learned

An Elastic IP address (EIP) is a static public IPv4 address provided by AWS.

It can be associated with an EC2 instance when a stable public IP address is required.

## Topics Covered

- What is an Elastic IP
- Public IP vs Elastic IP
- Allocating an Elastic IP
- Associating an Elastic IP with an EC2 instance
- Disassociating an Elastic IP
- Releasing an Elastic IP
- Moving an Elastic IP between EC2 instances
- Elastic IP and EC2 lifecycle

## Key Concepts

### Elastic IP

An Elastic IP is a static public IPv4 address that can be allocated to an AWS account and associated with an EC2 instance.

Unlike a normal automatically assigned public IPv4 address, an Elastic IP can remain associated with the account until it is released.

### Public IP vs Elastic IP

| Public IPv4 | Elastic IP |
|---|---|
| Automatically assigned to an instance | Explicitly allocated |
| Can change when the instance is stopped and started | Remains the same while allocated |
| Mainly used for temporary public access | Used when a stable public IP is required |
| Not independently managed like an EIP | Can be disassociated and reassociated |

### Allocate

Before using an Elastic IP, it must be allocated from AWS.

**Allocate → EIP created**

### Associate

An allocated Elastic IP can be associated with an EC2 instance.

**Elastic IP → EC2 Instance**

The instance can then use that static public IPv4 address.

### Disassociate

Disassociating an Elastic IP removes its connection to the EC2 instance.

The Elastic IP remains allocated to the AWS account and can be associated with another instance.

### Release

Releasing an Elastic IP returns the address to AWS.

Once released, the same Elastic IP cannot be assumed to remain available for reuse.

### Moving an Elastic IP

An Elastic IP can be moved between EC2 instances in the same AWS Region.

The process is:

**Disassociate → Associate with another EC2 instance**

Only one resource can use the Elastic IP association at a time.

## Hands-On Practice

- Allocated an Elastic IP
- Associated the Elastic IP with an EC2 instance
- Verified the public IP address
- Stopped and restarted the EC2 instance
- Verified that the Elastic IP remained associated
- Disassociated the Elastic IP
- Released the Elastic IP after completing the practical

## DevOps Relevance

Elastic IPs can be useful when a service requires a stable public IPv4 address.

For example, they can be used for:

- Public-facing servers
- Network appliances
- Specific infrastructure configurations
- Situations where a stable IP address is required

However, DNS-based solutions are often preferred when applications need a stable endpoint rather than depending directly on an IP address.

## Cost Awareness

Elastic IP addresses can incur charges depending on AWS's current public IPv4 pricing and how they are used.

For learning purposes, unused Elastic IPs should be released immediately after the practical to avoid unnecessary charges.
