# AWS Route 53 & DNS

## 1. What is DNS?

### Simple Explanation
DNS (Domain Name System) is like the **phonebook of the internet**.

Humans use domain names such as:

```text
google.com
myapp.com
api.myapp.com
```

Computers communicate using IP addresses.

DNS translates a domain name into the appropriate IP address or destination.

### Technical Definition
DNS is a **distributed naming system** that translates human-readable domain names into IP addresses and other DNS information.

### Example

```text
www.example.com
        ↓
      DNS
        ↓
   93.184.216.x
```

Without DNS, users would have to remember IP addresses instead of domain names.

---

# 2. What is Amazon Route 53?

### Simple Explanation

Amazon Route 53 is AWS's **managed DNS service**.

It helps convert domain names into destinations such as:

```text
www.myapp.com
       ↓
     Route 53
       ↓
      ALB
       ↓
      EC2
```

### Technical Definition

Amazon Route 53 is a highly available and scalable AWS-managed service used for:

1. DNS management
2. Domain registration
3. DNS-based traffic routing

### Important

Route 53 does **not** run your application.

For example:

- **Route 53** → DNS/name resolution
- **ALB** → distributes application traffic
- **EC2** → runs the application

---

# 3. Route 53 Hosted Zone

### Simple Explanation

A Hosted Zone is like a **container/folder that stores DNS records for a domain**.

Example:

```text
myapp.com
   │
   └── Hosted Zone
         ├── A Record
         ├── CNAME
         ├── MX
         ├── TXT
         ├── NS
         └── SOA
```

### Technical Definition

A Route 53 Hosted Zone is a container for DNS records that defines how DNS queries for a domain are answered.

There are two types:

- Public Hosted Zone
- Private Hosted Zone

---

# 4. Public vs Private Hosted Zone

| Public Hosted Zone | Private Hosted Zone |
|---|---|
| Used for public DNS | Used for internal/private DNS |
| Resolvable from the public internet | Resolvable within associated VPCs |
| Used for public websites/APIs | Used for internal applications/services |
| Example: `www.myapp.com` | Example: `db.internal` |
| Not tied to a private VPC for resolution | Associated with one or more VPCs |

### Interview Definition

A **public hosted zone** contains DNS records that can be resolved through the public DNS system.

A **private hosted zone** is associated with VPCs and is used for DNS resolution within private AWS environments.

---

# 5. DNS Records

DNS records tell DNS servers **what to do with a domain or hostname**.

Common DNS records:

- A
- AAAA
- CNAME
- MX
- TXT
- NS
- SOA

---

# 6. A Record

### Simple Explanation

An A record maps a hostname/domain to an **IPv4 address**.

```text
app.myapp.com
      ↓
   A Record
      ↓
  192.168.1.10
```

### Technical Definition

An A (Address) record maps a domain name or hostname to an IPv4 address.

### Interview

**A = IPv4**

---

# 7. AAAA Record

An AAAA record maps a hostname to an **IPv6 address**.

```text
app.myapp.com
      ↓
   AAAA Record
      ↓
   IPv6 address
```

### Interview

**AAAA = IPv6**

---

# 8. CNAME Record

### Simple Explanation

CNAME points one hostname to **another hostname**.

```text
www.myapp.com
      ↓
    CNAME
      ↓
app.myapp.com
```

### Technical Definition

A CNAME (Canonical Name) record maps one hostname to another hostname.

It does **not directly map to an IP address**.

### Important

CNAME cannot normally be used at the **zone apex/root domain**.

Example:

```text
myapp.com
```

For AWS resources, Route 53 Alias records are commonly used instead.

---

# 9. MX Record

MX stands for **Mail Exchange**.

It specifies the mail servers responsible for receiving email for a domain.

Example:

```text
myapp.com
    ↓
   MX
    ↓
mail server
```

### Example Use

If you use:

```text
user@myapp.com
```

the MX record helps determine where incoming email should be delivered.

---

# 10. TXT Record

TXT records store **text-based DNS information**.

Common uses include:

- Domain verification
- Email security
- SPF
- DKIM-related information
- Other service verification

Example:

```text
myapp.com
    ↓
   TXT
    ↓
verification information
```

---

# 11. Alias Record

Alias is a **Route 53-specific feature**.

### Simple Explanation

An Alias record can point a domain/hostname to supported AWS resources.

Example:

```text
www.myapp.com
      ↓
    Alias
      ↓
     ALB
```

### Supported AWS resources can include

- Application Load Balancer
- Network Load Balancer
- CloudFront distribution
- S3 website endpoint
- Other supported AWS resources

### A vs CNAME vs Alias

| Record | Points to |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Another hostname |
| Alias | Supported AWS resource |

### Important Interview Point

A Route 53 Alias can be used at the **zone apex/root domain**, whereas a traditional CNAME cannot.

Example:

```text
myapp.com
    ↓
 Alias
    ↓
  ALB
```

---

# 12. NS Record

NS stands for **Name Server**.

### Simple Explanation

It tells the internet **which DNS servers are authoritative for the domain**.

### Technical Definition

An NS record identifies the authoritative name servers responsible for a DNS zone.

### Common Scenario

Suppose:

```text
Domain registered somewhere else
              ↓
       Route 53 manages DNS
```

The domain's DNS delegation is configured to use the Route 53 name servers.

The NS records identify those authoritative servers.

---

# 13. SOA Record

SOA stands for **Start of Authority**.

### Simple Explanation

It contains important administrative/authoritative information about a DNS zone.

### Technical Definition

An SOA record identifies authoritative information for a DNS zone and contains information such as:

- Primary name server
- Responsible contact
- Serial number
- Refresh timing
- Retry timing
- Expiration timing
- Minimum/default TTL-related information

In normal Route 53 usage, you generally don't need to manually configure the SOA record.

---

# 14. Route 53 + AWS Architecture

A common AWS architecture is:

```text
                 User
                   │
                   ▼
             Route 53
              DNS lookup
                   │
                   ▼
                  ALB
          Load Balancer
                   │
          ┌────────┴────────┐
          ▼                 ▼
        EC2-1             EC2-2
     Application         Application
```

### Responsibilities

**Route 53**

Resolves the domain name and determines the DNS destination.

**ALB**

Receives application traffic and distributes it between healthy backend instances.

**EC2**

Runs the application.

### Interview Question

**How would you point a domain to an AWS Load Balancer?**

Use a **Route 53 Alias record** pointing the domain/hostname to the Load Balancer.

---

# 15. Route 53 Routing Policies

Routing policies determine **how Route 53 responds to DNS queries**.

Important policies:

1. Simple
2. Weighted
3. Failover
4. Latency-based
5. Geolocation
6. Geoproximity
7. Multi-value answer

---

## 15.1 Simple Routing

Used for basic DNS routing.

Example:

```text
app.myapp.com
      ↓
     ALB
```

Useful when there is a single primary destination.

---

## 15.2 Weighted Routing

Distributes DNS responses according to assigned weights.

Example:

```text
                 Route 53
                    │
             ┌──────┴──────┐
             ▼             ▼
          Version 1      Version 2
            90%            10%
```

### Common Use

Canary or gradual deployments.

Example:

```text
90% → existing version
10% → new version
```

---

## 15.3 Failover Routing

Used for **primary and secondary/backup resources**.

Example:

```text
             Route 53
                 │
          Is primary healthy?
             /        \
           Yes         No
            │           │
            ▼           ▼
         Primary     Secondary
```

If the primary is unhealthy, Route 53 can return the secondary resource.

### Common Use

- High availability
- Disaster recovery

---

## 15.4 Latency-Based Routing

Routes users to the AWS resource associated with the **lowest network latency** from the user.

Example:

```text
User in India
     ↓
Route 53
     ↓
Low-latency AWS region
```

Useful for applications deployed across multiple AWS Regions.

---

## 15.5 Geolocation Routing

Routes users based on their **geographic location**.

Example:

```text
India users
     ↓
India/Asia resource

US users
     ↓
US resource
```

Useful when traffic needs to be handled differently based on user location.

---

## 15.6 Geoproximity Routing

Routes traffic based on the geographic location of:

- Users
- AWS resources

It also supports a **bias** that can expand or shrink the geographic area from which traffic is routed to a resource.

This is a more advanced routing policy; for DevOps interviews, understand the concept rather than memorizing implementation details.

---

## 15.7 Multi-Value Answer Routing

Route 53 can return multiple healthy resource values in response to a DNS query.

Example:

```text
Route 53
   │
   ├── Server 1
   ├── Server 2
   └── Server 3
```

Route 53 can return multiple healthy IP addresses.

### Important

Multi-value answer routing is **not a replacement for a load balancer**.

Route 53 performs DNS-based routing; a load balancer distributes application traffic.

---

# 16. Route 53 Health Checks

### Simple Explanation

A Route 53 health check checks whether an endpoint/resource is healthy.

Example:

```text
Route 53 Health Check
         │
         ▼
      Endpoint
         │
    Healthy?
     /     \
   Yes      No
```

### Technical Definition

Route 53 health checks monitor the health of endpoints or can evaluate the state of a CloudWatch alarm. Health-check status can be used by supported routing policies to influence which DNS response is returned.

Health checks can perform checks using protocols such as:

- HTTP
- HTTPS
- TCP

### Common Use

Health checks are commonly used with **failover routing**.

```text
Health Check
     ↓
Is Primary Healthy?
     ↓
   Route 53
   /      \
Primary  Secondary
```

### Important

A health check does **not repair the server**.

It only determines health status, which Route 53 can use when making DNS routing decisions.

---

# 17. Route 53 vs Load Balancer

This distinction is important for DevOps interviews.

| Route 53 | Load Balancer |
|---|---|
| DNS service | Traffic distribution service |
| Resolves domain names | Receives application traffic |
| Makes DNS routing decisions | Distributes requests |
| Works at DNS level | Works with network/application traffic |
| Can use routing policies | Uses load-balancing algorithms |
| Can perform health checks | Performs target health checks |

### Example

```text
User
 ↓
Route 53
 ↓
ALB
 ↓
EC2
```

Route 53 answers:

> "Where should this domain resolve?"

ALB answers:

> "Which healthy backend should receive this request?"

---

# 18. Important Route 53 Interview Questions

### Q1. What is Route 53?

Amazon Route 53 is AWS's managed DNS service used for DNS management, domain registration, and DNS-based traffic routing.

### Q2. What is a Hosted Zone?

A hosted zone is a container for DNS records for a domain.

### Q3. Difference between Public and Private Hosted Zone?

Public hosted zones are used for publicly resolvable DNS, while private hosted zones are associated with VPCs and used for internal DNS resolution.

### Q4. What is an A record?

An A record maps a hostname to an IPv4 address.

### Q5. What is an AAAA record?

An AAAA record maps a hostname to an IPv6 address.

### Q6. What is a CNAME?

A CNAME maps one hostname to another hostname.

### Q7. What is an Alias record?

An Alias is a Route 53-specific record that can point a hostname to supported AWS resources such as an ALB.

### Q8. Why use Alias instead of CNAME for an ALB?

Route 53 Alias records can point directly to supported AWS resources and can also be used at the zone apex.

### Q9. What is an NS record?

An NS record identifies the authoritative DNS name servers for a domain/zone.

### Q10. What is an SOA record?

An SOA record contains authoritative and administrative information about a DNS zone.

### Q11. What is weighted routing?

Weighted routing distributes DNS responses according to configured weights.

### Q12. What is failover routing?

Failover routing sends DNS responses to a secondary resource when the primary resource is considered unhealthy.

### Q13. What is latency-based routing?

It routes users to the resource associated with the lowest latency.

### Q14. What is geolocation routing?

It routes DNS queries based on the geographic location of the user.

### Q15. What are Route 53 health checks?

They monitor endpoint health and can influence DNS routing decisions, especially with failover routing.

---

# 19. Practical

Perform a Route 53/DNS practical covering:

1. Explore the Route 53 console.
2. Understand Hosted Zones.
3. Examine DNS records.
4. Identify A, AAAA, CNAME, MX, TXT, NS and SOA records.
5. Understand Alias records.
6. Examine Route 53 routing policies.
7. Understand health-check configuration.
8. Verify DNS resolution using DNS lookup tools such as `nslookup` or `dig`.
9. Relate Route 53 to an AWS architecture:

```text
User
 ↓
Route 53
 ↓
ALB
 ↓
EC2
```

---

# 20. DevOps Engineer Takeaway

For a DevOps/Cloud Engineer, the most important concepts to remember are:

```text
DNS
 ↓
Route 53
 ↓
Hosted Zone
 ↓
DNS Records
 ↓
Routing Policies
 ↓
Health Checks
```

The key architecture to remember is:

```text
Domain
   ↓
Route 53
   ↓
ALB
   ↓
EC2
```

And the most important record distinction is:

```text
A       → IPv4
AAAA    → IPv6
CNAME   → hostname
MX      → mail server
TXT     → text/verification/security information
NS      → authoritative name servers
SOA     → zone authority information
Alias   → supported AWS resource
```

For interviews, focus especially on **Hosted Zones, A/CNAME/Alias records, Route 53 routing policies, health checks, and Route 53 vs Load Balancer**.
