# AWS Elastic Load Balancing

## What I Learned

Elastic Load Balancing (ELB) is an AWS service that distributes incoming application traffic across multiple targets such as EC2 instances.

Load balancing helps improve application availability, reliability, and scalability.

## Topics Covered

- What is Elastic Load Balancing
- Why load balancing is required
- ELB types
- Application Load Balancer (ALB)
- Target Groups
- Health Checks
- Listeners
- ALB traffic flow
- ALB vs Classic Load Balancer
- Handling unhealthy EC2 instances

## Key Concepts

### Elastic Load Balancing

ELB distributes incoming traffic across multiple backend targets.

A basic flow is:

**User → Load Balancer → Target Group → EC2**

The user sends the request to the load balancer instead of directly connecting to a specific backend server.

### Why Use a Load Balancer?

A load balancer can help:

- Distribute traffic across multiple servers
- Improve application availability
- Prevent a single server from handling all traffic
- Detect unhealthy targets
- Support scalable applications

### Types of Elastic Load Balancers

AWS provides different types of load balancers:

- **Application Load Balancer (ALB)** — HTTP/HTTPS traffic and Layer 7 application-level routing
- **Network Load Balancer (NLB)** — high-performance TCP/UDP/TLS traffic
- **Gateway Load Balancer (GWLB)** — deployment and scaling of network/security appliances

### Application Load Balancer

An ALB operates at the application layer and is designed primarily for HTTP and HTTPS applications.

It can distribute requests to multiple targets and supports application-level routing.

### Target Group

A target group is a collection of backend targets that receive traffic from the load balancer.

For an EC2-based application:

**ALB → Target Group → EC2 Instances**

### Health Check

A target group's health check determines whether a backend target is healthy enough to receive new traffic.

For example, an HTTP health check can use:

**Protocol:** HTTP  
**Port:** 80  
**Path:** `/`

If a target becomes unhealthy, the load balancer stops sending new traffic to that target.

### Listener

A listener checks for incoming connection requests on a specified protocol and port.

Examples:

- HTTP : 80
- HTTPS : 443

The listener receives the request and forwards it according to its configured rules.

### ALB Traffic Flow

A basic ALB request flow is:

**User → ALB → Listener → Target Group → Healthy EC2**

The ALB selects a healthy target from the target group and forwards the request.

### What Happens When an EC2 Instance Fails?

The target group's health check detects that the instance is unhealthy.

The ALB then stops sending new traffic to that unhealthy target and continues forwarding traffic to healthy targets.

### ALB vs Classic Load Balancer

ALB is the modern Layer 7 load balancer designed for HTTP/HTTPS applications and advanced application-level routing.

Classic Load Balancer is an older generation load balancer.

## Hands-On Practice

- Created EC2 instances in different Availability Zones
- Installed and configured a web server on the instances
- Created a target group
- Registered EC2 instances as targets
- Configured HTTP health checks
- Created an Application Load Balancer
- Configured an HTTP listener on port 80
- Configured the listener to forward traffic to a target group
- Verified that target health could become healthy
- Practiced the relationship between the ALB, listener, target group, health check, and EC2 instances
- Cleaned up the load balancer and related resources after the practical

## DevOps Relevance

Load balancing is important in DevOps for building highly available and scalable applications.

A common architecture is:

**Users → ALB → Target Group → EC2 Instances**

ALB can be combined with Auto Scaling so that the number of backend EC2 instances can change according to application demand.

## Cost Awareness

Elastic Load Balancers are billable AWS resources.

For learning purposes, the ALB practical was kept short and the load balancer and related resources were deleted after the exercise to avoid unnecessary charges.

## Practical Limitation

The ALB practical successfully demonstrated target registration, health checks, listener configuration, and healthy target status.

Direct browser access through the ALB DNS name was not successfully completed, and the ALB resources were deleted after the troubleshooting attempt.
