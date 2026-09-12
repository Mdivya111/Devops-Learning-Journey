# AWS Auto Scaling

## What is Auto Scaling?

Amazon EC2 Auto Scaling automatically adjusts the number of EC2 instances based on application demand and defined scaling rules.

It helps maintain application availability while providing the required capacity during changing workloads.

## Auto Scaling Group (ASG)

An Auto Scaling Group manages a group of EC2 instances.

It defines:

- **Minimum capacity** – lowest number of instances the ASG should maintain.
- **Desired capacity** – normal number of instances the ASG tries to maintain.
- **Maximum capacity** – highest number of instances the ASG can launch.

Example:

- Minimum: 2
- Desired: 3
- Maximum: 5

Normally, the ASG maintains 3 instances and can scale between 2 and 5.

## Launch Template

A Launch Template defines how an EC2 instance should be launched.

It can contain:

- AMI
- Instance type
- Key pair
- Security group
- Storage configuration
- Network configuration

### Mental Model

**Launch Template = how an EC2 instance should be created**

**Auto Scaling Group = how many EC2 instances should be running**

If the ASG needs another instance, it uses the Launch Template to determine how that instance should be launched.

## Scale Out and Scale In

### Scale Out

Scale out means increasing the number of EC2 instances when demand increases.

```text
2 instances → 4 instances
```

### Scale In

Scale in means decreasing the number of EC2 instances when demand decreases.

```text
4 instances → 2 instances
```

## Horizontal vs Vertical Scaling

### Horizontal Scaling

Changing the number of EC2 instances.

```text
2 instances → 4 instances
```

### Vertical Scaling

Changing the resources of an existing instance, such as moving from a smaller instance type to a larger one.

Auto Scaling primarily uses **horizontal scaling**.

## Scaling Policies

A scaling policy defines when and how an Auto Scaling Group should scale.

For example, a target tracking policy can maintain a target CPU utilization.

If CPU utilization remains above the configured target, the ASG can scale out.

If CPU utilization remains below the target, the ASG can scale in.

## Scaling Triggers

A scaling trigger is the condition that causes a scaling policy to take action.

Examples of metrics that can be used include:

- CPU utilization
- Network traffic
- Application Load Balancer request count

A simplified relationship is:

```text
CloudWatch Metric
       ↓
Scaling Policy
       ↓
Auto Scaling Group
       ↓
EC2 Instances
```

## Health Checks

An Auto Scaling Group can monitor the health of its EC2 instances.

If an instance becomes unhealthy, the ASG can terminate it and launch a replacement to maintain the desired capacity.

### ASG vs ALB Health Check

**ASG health check:** Determines whether an EC2 instance should remain in the Auto Scaling Group.

**ALB health check:** Determines whether an instance is healthy enough to receive application traffic.

## Instance Replacement

Suppose an ASG has a desired capacity of 2.

If one instance becomes unhealthy:

```text
2 instances
     ↓
1 healthy instance
     ↓
ASG detects unhealthy instance
     ↓
Replacement instance launched
     ↓
2 healthy instances
```

The purpose is to maintain the desired capacity.

## Auto Scaling with Load Balancer

A common architecture is:

```text
User
  ↓
Application Load Balancer
  ↓
Target Group
  ↓
Auto Scaling Group
  ↓
EC2 Instances
```

When demand increases:

1. The ASG launches additional EC2 instances.
2. Healthy instances become available to the target group.
3. The ALB distributes traffic across healthy instances.

When demand decreases:

1. The ASG scales in.
2. Unnecessary instances are removed.
3. The ALB continues sending traffic to the remaining healthy instances.

### ALB vs ASG

**ALB:** Decides where incoming traffic should go.

**ASG:** Decides how many EC2 instances should be running.

## Availability Zones

An Auto Scaling Group can distribute EC2 instances across multiple Availability Zones.

This improves availability and reduces dependence on a single Availability Zone.

## Instance Warm-up

After launching new instances, Auto Scaling can allow time for the instances to initialize and become ready before making further scaling decisions.

This helps prevent rapid or unnecessary scaling actions.

## Termination Policy

When an Auto Scaling Group needs to scale in, it needs a method for determining which instance should be terminated.

Auto Scaling termination policies help determine which instance is selected.

## ASG Lifecycle

A simplified Auto Scaling lifecycle is:

```text
Launch
  ↓
In Service
  ↓
Healthy
  ↓
Unhealthy
  ↓
Replacement
  ↓
New instance In Service
```

The ASG continuously works toward maintaining the desired state defined by its configuration.

## Hands-on Practice

Practiced creating and configuring an Auto Scaling environment using:

- Launch Template
- Auto Scaling Group
- Minimum, desired, and maximum capacity
- EC2 instance configuration
- Availability Zone selection
- Health check configuration
- Instance replacement concepts
- Launch Template versions
- Public IP configuration
- ASG instance lifecycle

The lab resources were cleaned up after practice.

## Key Interview Statement

> A Launch Template defines how an EC2 instance should be launched, while an Auto Scaling Group manages how many EC2 instances should be running.
