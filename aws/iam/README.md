# AWS IAM

## What I Learned

IAM (Identity and Access Management) is used to control access to AWS resources.

I learned and practiced:

- IAM Users
- IAM Groups
- IAM Policies
- AWS Managed Policies
- Customer Managed Policies
- Inline Policies
- IAM Roles
- Trust Policies
- Allow and Explicit Deny
- IAM permission troubleshooting

## Hands-On Practice

- Created IAM users and groups
- Attached policies to groups
- Created and tested IAM policies
- Worked with `iam:ListUsers`
- Practiced IAM Roles
- Created an EC2-to-S3 read-only role
- Investigated Access Denied errors
- Practiced granting permissions through policies

## Key Concepts

### IAM User
An identity representing a person or application that needs long-term AWS credentials.

### IAM Group
A collection of IAM users to which permissions can be assigned collectively.

### IAM Policy
A JSON document that defines what actions are allowed or denied on AWS resources.

### IAM Role
An identity that can be assumed by AWS services or other trusted principals and provides temporary permissions.

### Trust Policy
Defines who or what is allowed to assume an IAM role.

## Important Learning

An IAM permission is action-specific. Giving permission for one action, such as `iam:ListUsers`, does not automatically provide access to other IAM actions.

Explicit Deny overrides an Allow.

## DevOps Relevance

IAM is important in DevOps for securely giving AWS services and automation tools only the permissions they need.

Example:

EC2 → IAM Role → Temporary Credentials → S3
