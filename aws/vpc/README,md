# AWS VPC

## 1. What is VPC?

**VPC (Virtual Private Cloud)** is an isolated virtual network in AWS where we launch and manage AWS resources such as EC2 instances.

A VPC allows us to control:
- IP address ranges
- Subnets
- Routing
- Internet connectivity
- Network security

---

## 2. CIDR

**CIDR (Classless Inter-Domain Routing)** is used to define an IP address range for a VPC or subnet.

CIDR notation looks like:

`10.10.0.0/16`

The number after `/` is called the **prefix length**.

### How the prefix length works

The prefix length determines how large the IP address range is.

**Important rule:**

> Lower CIDR prefix number = larger IP range  
> Higher CIDR prefix number = smaller IP range

For example:

| CIDR | Relative size |
|---|---|
| `10.10.0.0/16` | Large range |
| `10.10.0.0/20` | Smaller than /16 |
| `10.10.0.0/24` | Smaller than /20 |
| `10.10.0.0/28` | Much smaller range |

### VPC and subnet example

In the VPC practical, I created:

**VPC:**

`10.10.0.0/16`

This provides the larger address space for the VPC.

**Subnet:**

`10.10.1.0/24`

This is a smaller network carved out from the VPC's address space.

So the relationship is:

`VPC: 10.10.0.0/16`

↓

`Subnet: 10.10.1.0/24`

The subnet must use an IP range that belongs within the VPC CIDR.

### Why CIDR matters in DevOps

CIDR is important when designing AWS networking because it determines:
- How many IP addresses are available
- How VPCs and subnets are divided
- Whether different networks can communicate without overlapping ranges
- How routing is designed
- How resources are organized across subnets

For DevOps interviews, understanding CIDR ranges and the relationship between VPC and subnet is more important initially than performing complex subnetting calculations.

---

## 3. Subnet

A **subnet** is a smaller network created inside a VPC.

A subnet belongs to exactly **one Availability Zone**.

A VPC can contain multiple subnets across multiple Availability Zones.

Example:

`VPC: 10.10.0.0/16`

- Subnet A → `10.10.1.0/24` → AZ-a
- Subnet B → `10.10.2.0/24` → AZ-b

Multiple AZs are commonly used for **high availability and fault tolerance**.

---

## 4. Public Subnet vs Private Subnet

A subnet is considered **public** when its route table contains a route to an **Internet Gateway (IGW)**.

Example:

`0.0.0.0/0 → Internet Gateway`

A private subnet does not have a direct route to an Internet Gateway.

### Important

Having a public IPv4 address on an EC2 instance does **not** by itself make the subnet public.

The subnet's routing determines whether it is public or private.

---

## 5. Internet Gateway

An **Internet Gateway (IGW)** provides internet connectivity for resources in a VPC.

However, simply attaching an IGW to a VPC does not automatically make every subnet public.

The subnet's route table must contain a route to the IGW.

Example:

`0.0.0.0/0 → DevOps-IGW`

---

## 6. Route Table

A **route table** contains routing rules that determine where network traffic should go.

Example:

| Destination | Target |
|---|---|
| `10.10.0.0/16` | Local |
| `0.0.0.0/0` | Internet Gateway |

`0.0.0.0/0` represents all IPv4 destinations that are not matched by a more specific route.

---

## 7. NAT Gateway

A **NAT Gateway** allows resources in a private subnet to initiate outbound connections to the internet without allowing unsolicited inbound internet connections to those resources.

Typical architecture:

`Private EC2`

↓

`Private Route Table`

↓

`NAT Gateway`

↓

`Public Route Table`

↓

`Internet Gateway`

↓

`Internet`

NAT Gateway is a **billable AWS resource**, so it should not be created unnecessarily for practice when working with a limited budget.

---

## 8. Security Group

A **Security Group (SG)** acts as a virtual firewall for an EC2 instance/network interface.

Key characteristics:
- Instance/NIC level
- Stateful
- Allows traffic through allow rules
- No explicit deny rules

**Stateful** means that if an allowed connection is established, the corresponding return traffic is automatically allowed.

---

## 9. Network ACL

A **Network ACL (NACL)** acts at the subnet level.

Key characteristics:
- Subnet level
- Stateless
- Supports allow and deny rules
- Rules are evaluated according to rule number

### SG vs NACL

| Security Group | NACL |
|---|---|
| Instance/NIC level | Subnet level |
| Stateful | Stateless |
| Allow rules | Allow + deny rules |
| Return traffic automatically allowed | Return traffic must be explicitly allowed |

---

## 10. VPC Peering

**VPC Peering** provides private network connectivity between two VPCs.

For communication to work, appropriate routes and security rules must be configured.

VPC Peering is **not transitive**.

Example:

`VPC A ↔ VPC B`

and

`VPC B ↔ VPC C`

does not automatically mean:

`VPC A ↔ VPC C`

---

## 11. VPC Endpoints

VPC Endpoints provide private connectivity between resources in a VPC and supported AWS services without requiring traffic to use the public internet for that service connection.

### Gateway Endpoint

Primarily used for:
- S3
- DynamoDB

It works through route tables.

Mental model:

`Route Table → Gateway Endpoint → AWS Service`

### Interface Endpoint

Uses an **ENI (Elastic Network Interface)** with private IP addresses inside the subnet and AWS PrivateLink.

Mental model:

`EC2 → ENI / Private IP → Interface Endpoint → AWS Service`

Interface endpoints can be billable, so they should not be created unnecessarily for practice.

---

## 12. ENI

**ENI (Elastic Network Interface)** is a virtual network interface in AWS.

It can have:
- Private IP address
- MAC address
- Security Group association
- Connectivity to a VPC

Interface VPC Endpoints use ENIs to provide private connectivity to supported AWS services.

---

## 13. Inbound vs Outbound Traffic

Traffic is data moving between network resources.

From the perspective of an EC2 instance:

**Inbound:**

`Internet → EC2`

**Outbound:**

`EC2 → Internet`

This distinction is important when configuring Security Groups, NACLs, and routing.

---

# VPC Practical

Created a custom VPC instead of relying only on the default VPC.

### VPC

Name: `DevOps-VPC`

CIDR:

`10.10.0.0/16`

### Subnet

Name: `DevOps-Public-Subnet`

CIDR:

`10.10.1.0/24`

Availability Zone:

`ap-south-1a`

### Security Group

Name:

`dev-vpc-sg`

### Internet Gateway

Name:

`DevOps-IGW`

Attached to:

`DevOps-VPC`

### Route Table

Name:

`DevOps-Public-RT`

Associated with:

`DevOps-Public-Subnet`

Route:

`0.0.0.0/0 → DevOps-IGW`

### EC2

The EC2 instance was launched inside the custom VPC and public subnet.

Private IP:

`10.10.1.201`

Public IP:

`13.126.50.177`

After configuring the Internet Gateway and route table, EC2 Instance Connect worked successfully.

Outbound internet connectivity was also verified from the EC2 instance using:

`curl https://www.google.com`

This confirmed the complete networking path:

`EC2 → Public Subnet → Route Table → Internet Gateway → Internet`

---

# VPC Topics Not Covered Yet

The following topics are intentionally postponed for later:

- VPC DNS
- VPC Flow Logs

They should **not** be marked as completed in the roadmap.
