# Amazon EFS (Elastic File System)

## 1. What is Amazon EFS?

Amazon EFS (Elastic File System) is a fully managed, elastic network file system that allows multiple EC2 instances to access the same files simultaneously.

EFS uses the **NFS (Network File System)** protocol to provide shared file storage over the network.

### Interview Answer

> Amazon EFS is a fully managed, elastic network file system that allows multiple EC2 instances to access shared files simultaneously.

---

## 2. Why Use EFS?

EFS is useful when multiple servers need to access the same files.

### Example

Suppose we have multiple web servers behind a Load Balancer:

```text
              Load Balancer
              /     |     \
             /      |      \
          EC2-1   EC2-2   EC2-3
             \      |      /
              \     |     /
                  EFS
               /uploads
```

All EC2 instances can access the same `/uploads` directory.

### Common Use Cases

- Shared application files
- User uploads
- Shared directories
- Content that must be accessible from multiple EC2 instances

### Interview Answer

> EFS is used when multiple EC2 instances need to access the same shared files, such as application data or uploaded files.

---

## 3. EFS vs EBS

| EBS | EFS |
|---|---|
| Block storage | File storage |
| Primarily used with EC2 | Can be accessed by multiple EC2 instances |
| Attached as a block device | Mounted over the network |
| Uses block-storage architecture | Uses NFS |
| Capacity is configured for the volume | Elastic filesystem that grows and shrinks with data |

### Interview Answer

> EBS is block storage generally used by an EC2 instance, while EFS is a managed network file system that allows multiple EC2 instances to access shared files simultaneously.

---

## 4. NFS

**NFS = Network File System**

NFS allows a Linux system to access files from a remote filesystem over a network.

EFS uses NFS to allow EC2 instances to access the EFS filesystem.

### Architecture

```text
EC2 Instance
     |
     | NFS
     |
EFS Mount Target
     |
     |
EFS File System
```

### Important Port

**TCP 2049 = NFS**

The security group associated with the EFS mount target must allow inbound TCP port 2049 from the EC2 instance's security group.

### Interview Answer

> NFS is a network file system protocol used to access files over a network. Amazon EFS uses NFS to allow EC2 instances to mount and access the shared filesystem.

---

## 5. EFS Mount Target

A **mount target** provides network access to an EFS filesystem from a VPC.

An EFS filesystem can have mount targets in multiple Availability Zones.

For example:

```text
              EFS
           /   |   \
          /    |    \
       MT-1   MT-2   MT-3
        |      |      |
       AZ-1   AZ-2   AZ-3
```

EC2 instances connect to EFS through the appropriate mount target.

### Interview Answer

> An EFS mount target provides network access to an EFS filesystem from a VPC and allows EC2 instances in the corresponding Availability Zone to connect to it.

---

## 6. EFS Storage Classes

EFS provides different storage classes for different access patterns.

### Standard

Used for frequently accessed data.

### Infrequent Access (IA)

Used for data that is accessed less frequently.

### Archive

Used for long-term, rarely accessed data.

EFS lifecycle management can automatically move files between appropriate storage classes based on access patterns.

### One Zone

EFS also supports **One Zone** storage, where the filesystem data is stored in a single Availability Zone.

One Zone options generally cost less but provide less Availability Zone-level resilience than Regional EFS.

### Mental Shortcut

```text
Regional EFS → Regional / multi-AZ architecture

One Zone EFS → Single AZ

IA → Infrequently accessed data
```

---

# EFS Practical

## Practical Goal

Demonstrate that multiple EC2 instances can access the same EFS filesystem.

### Lab Architecture

```text
          EFS File System
                |
        -----------------
        |               |
      EC2-1           EC2-2
        |               |
    /mnt/efs         /mnt/efs
        |               |
        ------ Shared ------
             filesystem
```

---

## Step 1 — Install EFS Mount Helper

On Amazon Linux:

```bash
sudo dnf install -y amazon-efs-utils
```

The `amazon-efs-utils` package provides utilities for mounting EFS.

---

## Step 2 — Create Mount Directory

```bash
sudo mkdir /mnt/efs
```

---

## Step 3 — Configure Security Group

The security group attached to the EFS mount target must allow:

```text
Protocol: TCP
Port: 2049
Source: EC2 Instance Security Group
```

Port 2049 is required for NFS communication.

Avoid unnecessarily allowing NFS from the entire internet.

---

## Step 4 — Mount EFS

Example:

```bash
sudo mount -t efs fs-xxxxxxxxxxxxxxxxx:/ /mnt/efs
```

Replace the filesystem ID with the actual EFS filesystem ID.

---

## Step 5 — Create a File on EC2-1

```bash
sudo touch /mnt/efs/test.txt
```

Verify:

```bash
ls -l /mnt/efs
```

---

## Step 6 — Mount the Same EFS on EC2-2

Install the mount helper:

```bash
sudo dnf install -y amazon-efs-utils
```

Create the mount directory:

```bash
sudo mkdir /mnt/efs
```

Mount the same filesystem:

```bash
sudo mount -t efs fs-xxxxxxxxxxxxxxxxx:/ /mnt/efs
```

Then:

```bash
ls -l /mnt/efs
```

The `test.txt` file created from EC2-1 should be visible from EC2-2.

---

## Step 7 — Verify Shared Read/Write Access

From EC2-2:

```bash
echo "Hello from EC2-2" | sudo tee /mnt/efs/test.txt
```

From EC2-1:

```bash
cat /mnt/efs/test.txt
```

The updated content should be visible from EC2-1.

This demonstrates that both EC2 instances are accessing the same EFS filesystem.

---

# Key Interview Questions

### 1. What is EFS?

> EFS is a fully managed network file system that allows multiple EC2 instances to access shared files simultaneously.

### 2. What protocol does EFS use?

> EFS uses the NFS protocol.

### 3. What is the NFS port?

> TCP port 2049.

### 4. What is the difference between EBS and EFS?

> EBS is block storage generally used by EC2, while EFS is a network file system designed for shared file access from multiple EC2 instances.

### 5. Can multiple EC2 instances access the same EFS filesystem?

> Yes. Multiple EC2 instances can mount and access the same EFS filesystem simultaneously.

### 6. What is an EFS mount target?

> A mount target provides network access to an EFS filesystem from a VPC.

### 7. Give a real-world EFS use case.

> Multiple web servers can use EFS as shared storage for application files or user uploads.

---

# Important Mental Model

Remember:

```text
EFS
 |
 | NFS
 |
Mount Target
 |
EC2
 |
/mnt/efs
```

The main reason to use EFS:

> **Multiple EC2 instances need access to the same shared files.**

---

# Lab Cleanup

After completing an EFS lab, remove the resources that were created for testing.

Check and clean up:

- EC2 instances created specifically for the lab
- EFS filesystem
- EFS mount targets
- EFS security group
- Any other temporary resources

Avoid leaving EFS resources running unnecessarily because EFS is usage-based.
