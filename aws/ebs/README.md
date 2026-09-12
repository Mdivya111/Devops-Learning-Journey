# AWS EBS

## What I Learned

Amazon EBS (Elastic Block Store) provides persistent block storage for EC2 instances.

EBS works like a virtual hard disk attached to an EC2 instance.

## Topics Covered

- What is EBS
- EBS and EC2 relationship
- Root EBS volume
- Additional EBS volumes
- EBS volume types
- Attaching and detaching EBS volumes
- Mounting an EBS volume
- EBS persistence
- Delete on termination
- EBS Snapshots
- Restoring a volume from a snapshot

## Key Concepts

### EBS

EBS provides block-level storage that can be attached to EC2 instances.

An EC2 instance provides compute, while EBS provides persistent storage.

**EC2 = Server**

**EBS = Disk**

### Root Volume

The root EBS volume normally contains the operating system of an EC2 instance.

For example:

**EC2 → Root EBS Volume → Operating System**

### Additional EBS Volume

Additional EBS volumes can be attached to an EC2 instance to store application data, logs, or other files separately from the root volume.

### EBS Volume Types

Common EBS volume types include:

- **gp3** — General-purpose SSD
- **gp2** — Older general-purpose SSD
- **io2** — High-performance SSD for I/O-intensive workloads
- **st1** — Throughput-optimized HDD
- **sc1** — Cold HDD for infrequently accessed data

For general-purpose workloads, gp3 is commonly used.

### Attach and Detach

An EBS volume can be attached to an EC2 instance when storage is required.

It can also be detached and attached to another compatible EC2 instance, subject to AWS availability-zone requirements.

### Mounting an EBS Volume

After attaching a volume to a Linux EC2 instance, the operating system must recognize and mount the volume before it can be used as a filesystem.

A typical process is:

**Attach → Identify device → Format if required → Mount → Use**

### Persistence

EBS storage is separate from the EC2 compute resource.

The behavior of an EBS volume when an EC2 instance is terminated depends on its **Delete on termination** setting.

### EBS Snapshot

An EBS Snapshot is a point-in-time backup of an EBS volume.

Snapshots can be used to:

- Back up data
- Restore data
- Create new EBS volumes
- Support disaster recovery

### Snapshot and EBS Relationship

The relationship can be represented as:

**EBS Volume → Snapshot → New EBS Volume**

A new EBS volume can be created from a snapshot and attached to an EC2 instance.

## Hands-On Practice

- Created an additional EBS volume
- Attached the volume to an EC2 instance
- Identified the attached EBS device
- Formatted the volume with the ext4 filesystem
- Mounted the EBS volume
- Created a test file on the mounted volume
- Unmounted the volume
- Created an EBS snapshot
- Created a new EBS volume from the snapshot
- Attached and mounted the restored volume
- Verified that the test file was recovered from the snapshot
- Cleaned up the resources after the practical

## DevOps Relevance

EBS is important in DevOps for providing persistent storage for EC2-based workloads.

It can be used for:

- Application data
- Logs
- Databases
- Configuration files
- Persistent application storage
- Backup and recovery

EBS snapshots are especially useful for backup, recovery, and creating reusable storage states.

## Cost Awareness

EBS volumes and snapshots can incur charges depending on their size, type, and storage usage.

For learning purposes, unnecessary volumes and snapshots should be deleted after practical exercises to avoid unexpected charges.
