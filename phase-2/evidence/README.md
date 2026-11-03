# Phase 2 - Lab Evidence

Screenshots from the Phase 2 IAM work: the group structure I designed, the role I created and attached to EC2, MFA on a developer user, and access key creation.

Sensitive values (account ID, key IDs, instance ID, public IP, Windows username, and ARNs) are blurred.

---

## Screenshot Index

### 1. IAM User Groups (`phase-2-01-iam-user-groups.jpg`)
- **Shows:** The 5 IAM groups: CEO_group, developers_group, DevOps_engineers_group, Financemanager_group, and Mycloudlab, with 10 user counts and permissions status.
- **Proves:** I assigned permissions to groups, not to individual users. Each person has their own identity, so their actions can be traced.

### 2. Role Created (`phase-2-02-role-created.jpg`)
- **Shows:** The EC2-S3-Access-Role permissions tab with the AWS managed policy AmazonS3FullAccess attached.
- **Proves:** I can create a role that gives a service S3 access without access keys.

### 3. Role Attached to EC2 (`phase-2-03-role-attached-ec2.jpg`)
- **Shows:** The EC2 instance's Security tab with IAM role EC2-S3-Access-Role assigned.
- **Proves:** The instance receives temporary credentials through the role.

### 4. MFA Enabled (`phase-2-04-mfa-enabled.jpg`)
- **Shows:** The developer_test1 user summary with console access marked "Enabled with MFA."
- **Proves:** This user must enter an MFA code to sign in, so a password alone is not enough.

### 5. Access Key Rotation (`phase-2-05-access-key-rotation.jpg`)
- **Shows:** An access key set to Inactive after creation.
- **Proves:** I deactivate access keys when it is not needed, then rotate after 90 days.

---

**Status:** Phase 2 complete
