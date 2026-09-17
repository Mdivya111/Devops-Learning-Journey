# AWS S3 — Complete Notes & CLI Reference

## 1. What is Amazon S3?

**Amazon S3 (Simple Storage Service)** is an AWS object storage service used to store and retrieve data.

S3 is commonly used for:

- Application backups
- Log storage
- Build artifacts
- Static website files
- Configuration files
- Data storage
- CI/CD deployment artifacts
- Terraform state
- Archiving

### Basic S3 structure

```text
S3
 └── Bucket
      └── Object
```

- **Bucket** → Container for storing objects
- **Object** → Actual file/data stored in S3
- **Object Key** → Name/identifier of the object inside the bucket

Example:

```text
Bucket:
divya-devops-s3-learning-2026

Object:
AWS My Notes.pdf

Object Key:
AWS My Notes.pdf
```

---

# 2. S3 Object Storage

S3 is **object storage**, not a traditional filesystem.

An object consists of:

- Data
- Object key
- Metadata
- Storage class
- Encryption information

Example object key:

```text
logs/2026/september/application.log
```

S3 does not have real directories like a traditional Linux filesystem. The `/` in an object key provides a folder-like organization in the console.

---

# 3. S3 Bucket

A bucket is a container for objects.

Example:

```text
divya-devops-s3-learning-2026
```

### Bucket naming rules

Traditional S3 general-purpose bucket names are:

- Globally unique
- 3–63 characters
- Lowercase letters
- Numbers
- Hyphens
- Periods
- Cannot contain spaces or uppercase letters

Example:

```text
divya-devops-s3-learning-2026
```

A bucket has a home AWS Region.

Example:

```text
Mumbai → ap-south-1
Ohio → us-east-2
```

---

# 4. S3 URI

An S3 object can be identified using an S3 URI.

Example:

```text
s3://divya-devops-s3-learning-2026/AWS My Notes.pdf
```

General format:

```text
s3://bucket-name/object-key
```

---

# 5. S3 ARN

An S3 object can also have an ARN.

Example:

```text
arn:aws:s3:::divya-devops-s3-learning-2026/AWS My Notes.pdf
```

General format:

```text
arn:aws:s3:::bucket-name/object-key
```

---

# 6. S3 Storage Classes

S3 provides different storage classes based on access frequency and cost requirements.

## S3 Standard

Used for frequently accessed data.

Examples:

- Active application files
- Frequently accessed build artifacts
- Frequently accessed data

---

## S3 Intelligent-Tiering

Used when access patterns are unpredictable or changing.

S3 automatically moves objects between access tiers based on usage.

---

## S3 Standard-IA

IA = Infrequent Access.

Used for data that is accessed less frequently but still requires rapid access when needed.

---

## S3 One Zone-IA

Stores data in a single Availability Zone.

It is suitable for data that can tolerate the loss of the Availability Zone.

---

## S3 Glacier Instant Retrieval

For archive data that still needs very fast retrieval.

---

## S3 Glacier Flexible Retrieval

For archive data where retrieval can take longer.

---

## S3 Glacier Deep Archive

For long-term archival data that is rarely accessed.

---

## Storage class comparison

| Storage Class | Typical Use |
|---|---|
| S3 Standard | Frequently accessed data |
| Intelligent-Tiering | Unknown/changing access patterns |
| Standard-IA | Infrequently accessed data |
| One Zone-IA | Infrequent data that can tolerate single-AZ storage |
| Glacier Instant Retrieval | Archive with fast retrieval |
| Glacier Flexible Retrieval | Archive with slower retrieval |
| Glacier Deep Archive | Long-term rarely accessed archive |

### Main trade-off

Generally:

```text
Lower storage cost
        ↓
Potentially higher retrieval cost / slower retrieval
```

Some storage classes also have minimum storage-duration considerations.

---

# 7. S3 Versioning

Versioning allows S3 to keep multiple versions of an object with the same object key.

Without versioning:

```text
file.txt
   ↓
new file.txt
   ↓
previous content replaced
```

With versioning:

```text
file.txt
 ├── Version 1
 ├── Version 2
 └── Version 3
```

### Benefits

- Protects against accidental overwrites
- Helps recover previous versions
- Protects against accidental deletion

### Delete Marker

When versioning is enabled, deleting an object normally creates a **Delete Marker** instead of immediately removing all previous versions.

Removing the Delete Marker can make the previous version visible again.

### Important

Versioning is not the same as a complete backup strategy.

Previous versions also consume storage.

---

# 8. S3 Bucket Policies

An S3 bucket policy is a **resource-based policy** attached to an S3 bucket.

It controls who can perform which actions on the bucket or its objects.

Important elements:

```text
Effect
Principal
Action
Resource
```

Example concepts:

```text
Effect     → Allow / Deny
Principal  → Who
Action     → What action
Resource   → Which bucket/object
```

### IAM policy vs Bucket policy

**IAM policy**

```text
Identity → What can this identity do?
```

**Bucket policy**

```text
Resource → Who can access this resource and what can they do?
```

An explicit `Deny` overrides an `Allow`.

---

# 9. Public vs Private S3 Access

S3 buckets are normally kept private.

### Private bucket

Only authorized identities/resources can access the objects.

Typical private data:

- Backups
- Logs
- Build artifacts
- Terraform state
- Internal application files

### Public bucket

Objects can potentially be accessed from the internet if the required permissions allow it.

### Block Public Access

S3 provides **Block Public Access** settings to help prevent accidental public exposure.

For normal DevOps workloads:

```text
Keep S3 private by default.
```

---

# 10. S3 Encryption

S3 supports encryption at rest.

## SSE-S3

Server-Side Encryption with S3-managed encryption keys.

Simple option for many workloads.

---

## SSE-KMS

Server-Side Encryption using AWS KMS keys.

Provides more control over encryption keys and auditing.

---

## Client-Side Encryption

The application encrypts the data before uploading it to S3.

---

# 11. S3 Lifecycle Rules

Lifecycle rules automatically perform actions on objects based on conditions such as object age.

Example:

```text
S3 Standard
     ↓
Standard-IA
     ↓
Glacier
     ↓
Delete
```

Lifecycle rules are useful for:

- Cost optimization
- Automatic archival
- Automatic cleanup
- Log retention

### Storage Class vs Lifecycle Rule

**Storage Class**

Defines how an object is stored.

**Lifecycle Rule**

Defines automatic actions that happen to an object over time.

---

# 12. S3 Cross-Region Replication

S3 Cross-Region Replication (CRR) automatically replicates objects from a source bucket to a bucket in another AWS Region.

Example:

```text
Mumbai
ap-south-1
     |
     | S3 Cross-Region Replication
     ↓
Ohio
us-east-2
```

Example:

```text
Source:
bucket-region-cross1
Mumbai

Destination:
bucket-region-desti
Ohio
```

### Why use CRR?

Possible use cases:

- Disaster recovery
- Business continuity
- Geographic redundancy
- Compliance requirements
- Lower-latency access from another region

### Important

CRR can introduce:

- Destination storage costs
- Inter-region data transfer costs

Therefore, use it carefully when working with a limited AWS budget.

---

# 13. AWS CLI and S3

AWS CLI allows us to manage S3 from the command line.

This is important for DevOps because S3 operations can be automated.

Typical use cases:

```text
Jenkins
   ↓
AWS CLI
   ↓
S3
```

For example, Jenkins can upload a build artifact to S3 automatically.

---

# 14. AWS CLI Authentication

Check AWS CLI installation:

```bash
aws --version
```

Configure credentials:

```bash
aws configure
```

The CLI asks for:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

Example:

```text
ap-south-1
json
```

Verify the currently authenticated identity:

```bash
aws sts get-caller-identity
```

This is useful for confirming which IAM user or role the CLI is using.

---

# 15. List S3 Buckets

```bash
aws s3 ls
```

Example:

```text
2026-09-01  divya-devops-bucket
2026-09-02  my-test-bucket
```

This lists buckets accessible to the authenticated identity.

---

# 16. Create an S3 Bucket

Basic command:

```bash
aws s3 mb s3://bucket-name
```

Example:

```bash
aws s3 mb s3://divya-s3-cli-learning-2026
```

### Create bucket in a specific region

```bash
aws s3 mb s3://bucket-name --region ap-south-1
```

Example:

```bash
aws s3 mb s3://my-bucket --region us-east-1
```

### Important

The bucket name must be globally unique when using the traditional S3 general-purpose namespace.

If another account already owns the name:

```text
BucketAlreadyExists
```

can occur.

---

# 17. List Objects Inside a Bucket

```bash
aws s3 ls s3://bucket-name
```

Example:

```bash
aws s3 ls s3://divya-s3-cli-learning-2026
```

---

# 18. List Objects Recursively

```bash
aws s3 ls s3://bucket-name --recursive
```

This lists objects under the bucket, including objects under folder-like prefixes.

Example:

```bash
aws s3 ls s3://my-bucket --recursive
```

---

# 19. Upload an Object

Upload a local file:

```bash
aws s3 cp file.txt s3://bucket-name/
```

Example:

```bash
aws s3 cp notes.txt s3://divya-s3-cli-learning-2026/
```

Upload to a specific object key:

```bash
aws s3 cp notes.txt s3://bucket-name/documents/notes.txt
```

---

# 20. Upload a Directory

```bash
aws s3 cp ./folder s3://bucket-name/folder --recursive
```

Example:

```bash
aws s3 cp ./logs s3://my-bucket/logs --recursive
```

`--recursive` tells the CLI to process the directory contents recursively.

---

# 21. Download an Object

```bash
aws s3 cp s3://bucket-name/file.txt .
```

The `.` means the current directory.

Example:

```bash
aws s3 cp s3://my-bucket/notes.txt .
```

Download with a different local filename:

```bash
aws s3 cp s3://my-bucket/notes.txt downloaded-notes.txt
```

---

# 22. Download an Entire Directory

```bash
aws s3 cp s3://bucket-name/folder ./folder --recursive
```

Example:

```bash
aws s3 cp s3://my-bucket/logs ./logs --recursive
```

---

# 23. Copy an Object Between S3 Locations

```bash
aws s3 cp s3://source-bucket/file.txt s3://destination-bucket/
```

Example:

```bash
aws s3 cp s3://source-bucket/file.txt s3://destination-bucket/
```

This copies the object without first downloading it to the local machine.

---

# 24. Copy an Entire S3 Prefix

```bash
aws s3 cp s3://source-bucket/source-folder s3://destination-bucket/destination-folder --recursive
```

Useful when copying multiple objects.

---

# 25. Copy Between Regions

The S3 CLI can copy objects between buckets in different regions.

Example:

```bash
aws s3 cp s3://source-bucket/file.txt s3://destination-bucket/ --source-region ap-south-1 --region us-east-2
```

This is different from S3 Cross-Region Replication.

### Important distinction

```text
aws s3 cp
```

→ Manual/command-driven copy.

```text
aws s3 sync
```

→ Synchronizes differences between locations.

```text
S3 Cross-Region Replication
```

→ AWS-managed replication based on configured replication rules.

---

# 26. Move an Object

The AWS CLI supports moving S3 objects using:

```bash
aws s3 mv
```

Example:

```bash
aws s3 mv s3://bucket-name/old/file.txt s3://bucket-name/new/file.txt
```

This effectively moves the object from the source location to the destination.

---

# 27. Move a Local File to S3

```bash
aws s3 mv file.txt s3://bucket-name/
```

Example:

```bash
aws s3 mv notes.txt s3://my-bucket/
```

---

# 28. Move an S3 Object to Local Machine

```bash
aws s3 mv s3://bucket-name/file.txt .
```

---

# 29. Move a Directory

```bash
aws s3 mv ./folder s3://bucket-name/folder --recursive
```

---

# 30. Delete an Object

```bash
aws s3 rm s3://bucket-name/file.txt
```

Example:

```bash
aws s3 rm s3://my-bucket/notes.txt
```

---

# 31. Delete Multiple Objects

Delete objects under a prefix:

```bash
aws s3 rm s3://bucket-name/folder/ --recursive
```

Example:

```bash
aws s3 rm s3://my-bucket/logs/ --recursive
```

---

# 32. Forceful Removal of a Bucket

An empty bucket can be deleted using:

```bash
aws s3 rb s3://bucket-name
```

If the bucket contains objects, normal removal can fail because the bucket is not empty.

To remove the bucket and its objects:

```bash
aws s3 rb s3://bucket-name --force
```

Example:

```bash
aws s3 rb s3://my-bucket --force
```

### What `--force` does

It removes objects from the bucket and then removes the bucket.

Conceptually:

```text
Bucket
 ├── object1
 ├── object2
 └── object3

        ↓

delete objects

        ↓

delete bucket
```

Use this carefully because it is destructive.

---

# 33. Delete an Empty Bucket

```bash
aws s3 rb s3://bucket-name
```

Example:

```bash
aws s3 rb s3://my-bucket
```

---

# 34. S3 Sync

`aws s3 sync` synchronizes files/objects between two locations.

It is **not limited to cross-region synchronization**.

It can synchronize:

```text
Local directory ↔ S3 bucket
S3 bucket ↔ Local directory
S3 bucket ↔ S3 bucket
```

---

## Local → S3

```bash
aws s3 sync ./website s3://my-bucket
```

Useful for:

- Static website deployment
- Build artifact uploads
- Backup synchronization

---

## S3 → Local

```bash
aws s3 sync s3://my-bucket ./backup
```

Useful for:

- Downloading backups
- Recovering files
- Local copies of S3 data

---

## S3 → S3

```bash
aws s3 sync s3://source-bucket s3://destination-bucket
```

This can synchronize objects between buckets.

---

# 35. Sync Between Different Regions

Example:

```bash
aws s3 sync s3://source-bucket s3://destination-bucket --source-region ap-south-1 --region us-east-2
```

This is command-driven synchronization between two S3 locations.

It should not be confused with configured S3 Cross-Region Replication.

---

# 36. Excluding Files During Sync

You can exclude matching files:

```bash
aws s3 sync ./folder s3://my-bucket --exclude "*.tmp"
```

Example:

```bash
aws s3 sync ./website s3://my-bucket --exclude "*.log"
```

---

# 37. Including Specific Files During Sync

```bash
aws s3 sync ./folder s3://my-bucket --exclude "*" --include "*.html"
```

This can be useful when only specific file types should be synchronized.

---

# 38. Delete Extra Destination Objects During Sync

The `--delete` option removes files from the destination that are not present in the source.

Example:

```bash
aws s3 sync ./website s3://my-bucket --delete
```

Conceptually:

```text
Local:
A
B

S3:
A
B
C

sync --delete

S3:
A
B
```

### Warning

`--delete` is destructive.

It can remove destination objects that are not present in the source.

Always understand the source and destination before using it.

---

# 39. Dry Run

Before performing a potentially destructive operation, use:

```bash
--dryrun
```

Example:

```bash
aws s3 sync ./website s3://my-bucket --delete --dryrun
```

This shows what would happen without actually making the changes.

Useful for:

- Testing commands
- Checking synchronization
- Avoiding accidental deletion
- Troubleshooting automation

---

# 40. Specify Region for S3 Commands

A region can be supplied using:

```bash
--region REGION
```

Example:

```bash
aws s3 ls --region ap-south-1
```

or:

```bash
aws s3 mb s3://my-bucket --region us-east-1
```

### Important command-line rule

`--region` is an **option/parameter**, not a standalone command.

Incorrect:

```bash
--region us-east-1
```

Correct:

```bash
aws s3 ls --region us-east-1
```

or:

```bash
aws s3 mb s3://my-bucket --region us-east-1
```

---

# 41. Changing the Region of an Existing S3 Bucket

An S3 bucket's Region is not normally changed in place.

If you need the data in another Region, the usual approach is:

```text
Source bucket
     ↓
Copy / Sync
     ↓
New bucket in target Region
```

Example:

```bash
aws s3 mb s3://my-new-bucket --region us-east-2
```

Then:

```bash
aws s3 sync s3://old-bucket s3://my-new-bucket --source-region ap-south-1 --region us-east-2
```

This creates a new bucket in the target Region and synchronizes the objects.

---

# 42. Check a Bucket's Region

Using AWS CLI:

```bash
aws s3api get-bucket-location --bucket bucket-name
```

Example:

```bash
aws s3api get-bucket-location --bucket my-bucket
```

For some AWS APIs, `us-east-1` can be represented differently/returned as a null location value, so don't interpret a null result as "no region."

---

# 43. Useful S3 `s3api` Commands

The high-level:

```bash
aws s3
```

commands are generally easier for day-to-day file operations.

The lower-level:

```bash
aws s3api
```

commands provide more direct access to S3 APIs.

### Get bucket location

```bash
aws s3api get-bucket-location --bucket my-bucket
```

### Check bucket versioning

```bash
aws s3api get-bucket-versioning --bucket my-bucket
```

### Enable versioning

```bash
aws s3api put-bucket-versioning \
    --bucket my-bucket \
    --versioning-configuration Status=Enabled
```

### Check bucket encryption

```bash
aws s3api get-bucket-encryption --bucket my-bucket
```

### Check bucket tagging

```bash
aws s3api get-bucket-tagging --bucket my-bucket
```

These commands are useful when troubleshooting or scripting S3 configuration.

---

# 44. High-Level S3 CLI Command Cheat Sheet

## AWS CLI setup

```bash
aws --version
aws configure
aws sts get-caller-identity
```

## Bucket operations

```bash
aws s3 ls
aws s3 mb s3://bucket-name
aws s3 mb s3://bucket-name --region ap-south-1
aws s3 rb s3://bucket-name
aws s3 rb s3://bucket-name --force
```

## Object listing

```bash
aws s3 ls s3://bucket-name
aws s3 ls s3://bucket-name --recursive
```

## Upload

```bash
aws s3 cp file.txt s3://bucket-name/
aws s3 cp ./folder s3://bucket-name/folder --recursive
```

## Download

```bash
aws s3 cp s3://bucket-name/file.txt .
aws s3 cp s3://bucket-name/folder ./folder --recursive
```

## Copy

```bash
aws s3 cp s3://source-bucket/file.txt s3://destination-bucket/
aws s3 cp s3://source-bucket/folder s3://destination-bucket/folder --recursive
```

## Move

```bash
aws s3 mv file.txt s3://bucket-name/
aws s3 mv s3://bucket-name/file.txt .
aws s3 mv s3://bucket-name/old/file.txt s3://bucket-name/new/file.txt
aws s3 mv ./folder s3://bucket-name/folder --recursive
```

## Delete

```bash
aws s3 rm s3://bucket-name/file.txt
aws s3 rm s3://bucket-name/folder/ --recursive
```

## Synchronize

```bash
aws s3 sync ./folder s3://bucket-name
aws s3 sync s3://bucket-name ./folder
aws s3 sync s3://source-bucket s3://destination-bucket
```

## Sync with deletion

```bash
aws s3 sync ./folder s3://bucket-name --delete
```

## Dry run

```bash
aws s3 sync ./folder s3://bucket-name --delete --dryrun
```

## Region

```bash
aws s3 ls --region ap-south-1
aws s3 mb s3://bucket-name --region us-east-1
```

## Bucket region

```bash
aws s3api get-bucket-location --bucket bucket-name
```

---

# 45. `cp` vs `mv` vs `sync`

| Command | Purpose |
|---|---|
| `aws s3 cp` | Copy/upload/download |
| `aws s3 mv` | Move data |
| `aws s3 sync` | Synchronize differences |
| `aws s3 rm` | Delete objects |
| `aws s3 mb` | Create bucket |
| `aws s3 rb` | Delete bucket |
| `aws s3 ls` | List buckets/objects |

### Easy mental model

```text
cp    → Copy
mv    → Move
sync  → Synchronize
rm    → Remove
mb    → Make bucket
rb    → Remove bucket
ls    → List
```

---

# 46. Important Difference: `cp` vs `sync`

### `cp`

Copies the specified file/object.

```bash
aws s3 cp file.txt s3://my-bucket/
```

### `sync`

Compares source and destination and synchronizes the differences.

```bash
aws s3 sync ./folder s3://my-bucket
```

For a DevOps deployment, `sync` is often useful when a complete directory needs to be kept synchronized with an S3 location.

---

# 47. Important Difference: `sync` vs Cross-Region Replication

These are not the same.

### AWS CLI Sync

```bash
aws s3 sync s3://source-bucket s3://destination-bucket
```

Command-driven synchronization.

### S3 Cross-Region Replication

```text
Source bucket
     ↓
S3 replication rule
     ↓
Destination bucket
```

AWS handles replication according to the configured rule.

---

# 48. S3 DevOps Use Cases

## Build artifacts

A CI/CD pipeline can upload build artifacts to S3.

```text
Developer
    ↓
Git
    ↓
Jenkins
    ↓
Build
    ↓
AWS CLI
    ↓
S3
```

---

## Static website deployment

Website files can be synchronized to S3.

```bash
aws s3 sync ./website s3://my-website-bucket
```

---

## Backups

Application or generated files can be uploaded to S3.

```bash
aws s3 sync ./backup s3://my-backup-bucket
```

---

## Logs

Applications can store logs in S3 for long-term retention.

Lifecycle rules can later move old logs to cheaper storage classes.

---

## Terraform State

S3 can be used as remote storage for Terraform state.

For production environments, state management also requires appropriate security, locking/concurrency considerations, and access control.

---

# 49. Common CLI Mistakes

## Mistake 1: Using `md` instead of `mb`

Incorrect:

```bash
aws s3 md s3://my-bucket
```

Correct:

```bash
aws s3 mb s3://my-bucket
```

`mb` means **make bucket**.

---

## Mistake 2: Wrong bucket name

If the bucket does not exist:

```bash
aws s3 rb s3://wrong-bucket
```

may return:

```text
NoSuchBucket
```

Always verify the bucket name with:

```bash
aws s3 ls
```

---

## Mistake 3: Treating `--region` as a command

Incorrect:

```bash
--region us-east-1
```

Correct:

```bash
aws s3 mb s3://my-bucket --region us-east-1
```

---

## Mistake 4: Forgetting `s3://`

Correct:

```bash
aws s3 ls s3://my-bucket
```

---

## Mistake 5: Trying to delete a non-empty bucket

A bucket containing objects cannot normally be removed with:

```bash
aws s3 rb s3://my-bucket
```

Use:

```bash
aws s3 rb s3://my-bucket --force
```

only when you intentionally want to delete the objects and bucket.

---

# 50. AWS S3 Cost Awareness

S3 is usage-based.

Potential cost areas include:

- Storage
- Requests
- Data transfer
- Retrieval
- Replication
- Certain storage classes
- Some advanced S3 features

Cross-region replication can create additional costs.

Archive and infrequent-access storage classes can have retrieval/minimum-duration considerations.

For small learning labs:

```text
Create only what is necessary
        ↓
Use tiny files
        ↓
Complete the practical
        ↓
Delete temporary resources
```

Always check the pricing implications before using advanced features.

---

# 51. S3 Practical Checklist

The S3 hands-on work covered:

- [x] Create S3 bucket
- [x] Select bucket Region
- [x] Upload object
- [x] List bucket
- [x] List objects
- [x] Copy object
- [x] Download object
- [x] Move object
- [x] Delete object
- [x] Delete bucket
- [x] Force-delete bucket containing objects
- [x] Synchronize local directory and S3
- [x] Synchronize S3 locations
- [x] Use different Regions
- [x] S3 storage classes
- [x] Versioning
- [x] Delete Marker
- [x] Bucket policies
- [x] Public/private access
- [x] Encryption
- [x] Lifecycle concepts
- [x] Cross-Region Replication
- [x] AWS CLI authentication
- [x] `aws sts get-caller-identity`
- [x] Cleanup of learning resources

---

# 52. Interview Questions — Quick Answers

### What is S3?

S3 is AWS's object storage service used to store and retrieve data such as backups, logs, build artifacts, and application files.

### What is an S3 bucket?

A bucket is a container used to store S3 objects.

### What is an S3 object?

An object is the actual data stored in S3 along with its metadata and object key.

### What is an object key?

An object key is the unique name/identifier of an object within a bucket.

### Is S3 a filesystem?

No. S3 is an object storage service.

### What is S3 versioning?

Versioning allows S3 to maintain multiple versions of an object with the same key.

### What is a Delete Marker?

With versioning enabled, a normal delete creates a Delete Marker, while previous object versions remain.

### What is a bucket policy?

A bucket policy is a resource-based policy attached to an S3 bucket that controls access to the bucket and its objects.

### What is S3 encryption?

Encryption protects S3 data at rest. Common approaches include SSE-S3, SSE-KMS, and client-side encryption.

### What is a lifecycle rule?

A lifecycle rule automatically transitions or deletes objects based on conditions such as object age.

### What is Cross-Region Replication?

CRR automatically replicates S3 objects from a source bucket to a destination bucket in another AWS Region.

### What is `aws s3 sync`?

`aws s3 sync` synchronizes objects between local directories and S3 or between S3 locations. It is useful for automation, backups, and deployments.

### Why is AWS CLI important for DevOps?

AWS CLI allows S3 and other AWS operations to be performed from scripts and CI/CD pipelines instead of manually using the AWS Console.

### `cp` vs `sync`?

`cp` copies specified files or objects, while `sync` compares source and destination and synchronizes their differences.

### `cp` vs `mv`?

`cp` copies data while `mv` moves data from one location to another.

### How do you delete a bucket containing objects?

```bash
aws s3 rb s3://bucket-name --force
```

### How do you create a bucket in a specific Region?

```bash
aws s3 mb s3://bucket-name --region region-name
```

### How do you upload a file to S3?

```bash
aws s3 cp file.txt s3://bucket-name/
```

### How do you download a file from S3?

```bash
aws s3 cp s3://bucket-name/file.txt .
```

### How do you synchronize a local directory with S3?

```bash
aws s3 sync ./folder s3://bucket-name
```

---

# 53. Final S3 Mental Model

```text
                         Amazon S3
                            |
                     +------+------+
                     |             |
                  Bucket        Bucket
                     |             |
                  Objects        Objects
                     |
              +------+------+
              |             |
          Object Key     Metadata
```

DevOps interaction:

```text
Developer
    |
    ↓
Git
    |
    ↓
CI/CD Pipeline
    |
    ↓
AWS CLI
    |
    ↓
S3
    |
    +── Upload
    +── Download
    +── Copy
    +── Move
    +── Delete
    +── Sync
    +── Backup
    +── Build Artifacts
    +── Logs
```

### Core commands to remember

```bash
aws s3 ls
aws s3 mb s3://bucket-name
aws s3 cp source destination
aws s3 mv source destination
aws s3 sync source destination
aws s3 rm s3://bucket-name/object
aws s3 rb s3://bucket-name
aws s3 rb s3://bucket-name --force
aws s3api get-bucket-location --bucket bucket-name
aws sts get-caller-identity
```

**S3 status: COMPLETE**
