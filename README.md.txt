# Secure S3 Data Repository

## Overview

A secure Amazon S3 data repository built to demonstrate practical AWS cloud security principles, including encryption, access control, least privilege, versioning, and protection against unauthorized access.

## Architecture

The repository uses AWS IAM to control access to a private Amazon S3 bucket.

**Architecture flow:**

`User → IAM → Amazon S3`

The S3 bucket is protected using multiple security controls:

* Block Public Access
* SSE-S3 encryption
* Versioning
* Bucket owner enforced object ownership
* IAM least-privilege access
* MFA for the restricted IAM user

## Security Controls

### 1. Block Public Access

All public access to the S3 bucket is blocked to prevent unintended exposure of stored data.

### 2. Encryption

SSE-S3 is enabled as the default encryption method, ensuring objects stored in the bucket are encrypted at rest.

### 3. Versioning

S3 Versioning is enabled to preserve previous versions of objects and provide protection against accidental overwrites or deletions.

### 4. IAM Least Privilege

A restricted IAM user named `Arda` was created with only the permissions required to work with objects in the repository.

Allowed actions:

* `s3:ListAllMyBuckets`
* `s3:ListBucket`
* `s3:GetObject`
* `s3:PutObject`

The user was intentionally **not granted `s3:DeleteObject` permission**.

### 5. MFA

MFA was enabled for the restricted IAM user to provide an additional layer of authentication security.

## Security Testing

The configuration was tested using the restricted IAM user.

| Action          | Result          |
| --------------- | --------------- |
| List S3 bucket  | ✅ Allowed       |
| Upload object   | ✅ Allowed       |
| Download object | ✅ Allowed       |
| Delete object   | ❌ Access Denied |
| List IAM users  | ❌ Access Denied |

The failed deletion demonstrated that the least-privilege policy was working as intended.

The denied `iam:ListUsers` request also confirmed that the restricted user could not perform IAM administration.

## Repository Structure

```text
Secure-S3-Data/
│
├── README.md
├── iam-policy.json
│
├── Screenshots/
│   ├── failed to delete.png
│   ├── file structure.png
│   └── successfuly uploaded.png
│
└── Architecture/
    └── architecture-diagram.png
```

## AWS Services Used

* Amazon S3
* AWS IAM

## Key Concepts Demonstrated

* Cloud storage security
* Identity and Access Management
* Least privilege
* Data encryption at rest
* MFA
* S3 Versioning
* Public access prevention
* Access control testing

## What I Learned

This project provided practical experience implementing and testing AWS security controls rather than only studying them theoretically. The most important lesson was that permissions should be intentionally limited to the actions an identity actually needs.

## Future Improvements

* Enforce HTTPS-only access using an S3 bucket policy
* Rebuild the infrastructure using Terraform
* Introduce additional monitoring and security controls
* Automate security configuration using Infrastructure as Code
