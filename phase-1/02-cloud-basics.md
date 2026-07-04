# Cloud Basics: Phase 1

Complete documentation of AWS setup, EC2 instance creation, and cloud fundamentals learned.

## AWS Account Creation & Setup

### The Beginning
**Date:** June 2, 2026 
**Task:** Create AWS account with security best practices

### Steps Completed

#### 1. Root Account Creation
- Email: Personal email address
- Password: Strong password created
- Phone number: Added for recovery
- Payment method: Added (for free tier)

**Security Setup:**
-  Created root account
-  **IMPORTANT:** Did NOT use root for daily work (best practice!)
-  Root account is dangerous (has ALL permissions)

### 2. Multi-Factor Authentication (MFA) Setup

**Why MFA is Critical:**
Without MFA:

Password stolen → Hacker has full access
One layer of security = easy target

With MFA:

Password stolen → Still can't login (needs phone)
Hacker has password BUT no phone
Two layers = much safer!

**Setup Process:**
1. Logged into root account
2. Went to IAM console
3. Enabled MFA
4. Used Authenticator app (time based tokens)
5. Generated backup codes (saved securely)

**Result:** Root account now protected with 2FA 

### 3. IAM User Creation

**Why not use root?**
- Root = Too much power
- Root = Can't track individual actions
- Root = Can't revoke access easily
- Best practice = Create IAM users for everything

**User Created:**
- **Username:** Mycloudlab
- **Access type:** Console + Programmatic
- **Permissions:** Administrator (for learning)
- **Password:** Created during setup
- **MFA:** Enabled (best practice!)

**Key Insight:**
Root account flow:

Root email + password

↓

Creates IAM user "Mycloudlab"

↓

Use Mycloudlab for daily work

↓

Keep root locked away (emergency only)

### 4. Billing Alerts Setup

**Why billing alerts?**

Free tier: $0

But if I accidentally:

Launch expensive instance
Transfer data across regions
Leave running 24/7

Then: Bill = $1000+
Solution: Alerts = warning before damage

**Alerts Configured:**
- Alert when estimated charges exceed $10
- Monthly budget limit set
- Email notifications enabled

**Real Lesson:** Free tier is free ONLY if you're careful! 

## EC2 Instance Creation

### The Mission
**Goal:** Launch my first cloud server (EC2 instance)

### Instance Details

**Instance Specifications:**

AMI (Operating System): Amazon Linux 2

Instance Type: t3.micro (smallest, free tier eligible)

Region: eu-west-1 (Europe;Ireland)

Availability Zone: eu-west-1a

Instance ID: i-0655e524004d40cb9

Public IPv4: 19.180.254.27

Status: Running (connected via SSH)

**Why t3.micro?**
-  Free tier eligible
-  Enough for learning
-  Would pay if running 24/7, but okay for practice
-  Can always upgrade later

**Why Amazon Linux?**
-  Popular in AWS ecosystem
-  Pre-optimized for EC2
-  Good for learning
-  Different from Ubuntu (good to know both!)

### Key Pair Creation

**What is a key pair?**

Like a house key:

Public key = The lock (on server)
Private key = Your key (keep secret!)
Without key = Can't enter house (server)

**Process:**
1. Created key pair: "my-first-key"
2. Downloaded .pem file
3. Saved in: Downloads

**Critical Security:**

- NEVER share your .pem file

- NEVER commit .pem to GitHub

- NEVER post .pem in chat

- Treat like password (actually more important!)

- If someone gets it, they have SSH access

**In .gitignore:**

*.pem

This prevents accidental commits!

### Security Group Setup (The Learning Moment!)

**What is a security group?**
It's a firewall:
- Defines what traffic is allowed IN/OUT
- Default: DENY ALL (nothing allowed)
- You explicitly allow what you need

**The Problem:**
1. Created EC2 instance
2. Created security group (blocked SSH by default)
3. Tried to connect: **"Connection refused"** 

**Investigation:**
- Checked instance status: Running 
- Checked IP address: Correct 
- Checked security group: SSH (port 22) blocked 

**The Fix:**
Added inbound rule to security group:
Type: SSH

Protocol: TCP

Port: 22

Source: My IP (0.0.0.0/0 for testing, not production)
**Result:** SSH access restored 

**Security Lesson:**
This is a GOOD design

Default deny = secure

Explicitly allow = controlled

If hacker tries SSH:

Security group blocks it
They can't get in

### SSH Connection Setup (Windows PowerShell)

**Challenge:** Windows doesn't have native SSH like Mac/Linux

**Solution:** PowerShell with SSH capability

**Steps:**

#### 1. Fix Key Permissions
```powershell
icacls "my-first-key.pem" /inheritance:r /grant:r "$env:username`:F"
```

**Why?** Windows is paranoid about .pem file permissions. This restricts it to ONLY me.

#### 2. Connect to Server
```powershell
ssh -i "my-first-key.pem" ec2-user@19.180.254.27
```

**Breaking it down:**
- `ssh` = SSH protocol
- `-i` = Identity file (your key)
- `"my-first-key.pem"` = Your key file
- `ec2-user` = Username (default for Amazon Linux)
- `19.180.254.27` = Server IP address

#### 3. First SSH Connection
The authenticity of host can't be established.

RSA key fingerprint is...

Are you sure you want to continue? (yes/no)
Type: `yes`

**Result:** Connected! 
ec2-user@ip-172-31-0-1:~$

**I am now inside my cloud server!**

## What I Can Do Now

### From my Windows Computer
```powershell
# Connect to cloud server
ssh -i "my-first-key.pem" ec2-user@19.180.254.27

# I am now in Linux!
# Run any Linux command
pwd
ls
cd /var
cat /etc/hostname
```

### Power Realization
My laptop in Nigeria/UK

↓ (SSH over internet)

AWS server in Ireland

↓ (I control it!)

Run commands instantly

## GitHub Setup

### Why GitHub?

Local files:
- Lost if computer dies
- No history
- Can't share

GitHub:
- Cloud backup
- Full history
- Professional portfolio
- Version control

### Repository Created
- **Name:** My-Cloud-Security-Journey
- **Description:** 16-week cloud security engineering journey
- **Visibility:** Public (portfolio for employers)
- **URL:** github.com/GeorgeGabs/My-Cloud-Security-Journey

### Security: .gitignore File

**Created to block:**

*.pem              # SSH keys

.aws/              # AWS credentials

.env               # Environment variables

credentials        # Any credential files

secrets/           # Secret files

**Why?** Prevent accidental commits of credentials. GitHub will alert you, but prevention is better.

### Professional README

```markdown
# My Cloud Security Engineering Journey

## Goal
Become a Cloud Security Engineer in 16 weeks

## What I'm Learning
- Cloud infrastructure (AWS)
- Linux fundamentals
- Security principles (CIA Triad)
- Identity & Access Management (IAM)
- Network security
- Data protection

## Progress
Week 1: Foundations 
Week 2-3: IAM 
Week 4-16: Advanced security

## Projects
- Real incident investigation (found hackers!)
- IAM structure design
- Security architecture
- Hands-on labs
```
## Free Tier Understanding

### The Reality of "Free"

**Free Tier includes:**
- 750 hours EC2 per month (≈1 instance running 24/7)
- 5 GB S3 storage
- 20 GB database
- BUT: Only for 6 months

**Critical Understanding:**

You have 6 months free (not 12!)

After 6 months or after 750 hours/month:

You start paying
EC2 costs ~$0.01/hour (small)
But T3.micro: ~$7-10/month

Strategy:

- Stop instances when not using

- Delete unused resources

- Monitor billing weekly

- Set alerts

  **Cost Lesson:**

  Running instance 24/7 for 1 month:

750 hours × $0.01 = $7.50
Running 2 instances 24/7:

~$15/month
Running 10 instances carelessly:

Could be $100+/month!

## Security Lessons Learned

### Lesson 1: Default Deny is Secure

Root account:
- Can do anything
- Can't track actions
- Can't revoke easily
- If hacked, full compromise
Solution:
- Use IAM users for daily work
- Keep root locked away

### Lesson 3: MFA Saves You

Password only:

Hacker steals password → Full access
Password + MFA:

Hacker steals password → Still can't login

Needs your phone → Has to steal that too

Much harder!

### Lesson 4: Cloud is Real Infrastructure

- This isn't simulation

- This is REAL server in Ireland

- REAL hackers try to break in (I found evidence!)

## AWS Console Navigation

### Main Dashboard

AWS Management Console

├── EC2 (Compute)

│   ├── Instances

│   ├── Security Groups

│   └── Key Pairs

├── S3 (Storage)

├── RDS (Database)

├── IAM (Users/Permissions)

└── CloudTrail (Audit logs)

### Finding My Instance
1. Go to AWS Console
2. Go to EC2
3. Click "Instances"
4. Find my instance (Running status)
5. Copy Public IPv4 address
6. Use with SSH command

## Summary: Cloud Basics Completed

| Task | Status |
|------|--------|
| AWS account creation | 
| MFA setup |  | 9/10 |
| IAM user creation |  
| EC2 instance launch |  
| Security group setup |  
| SSH connection |  
| GitHub setup |  
| Understanding free tier |  

## Key Takeaways

1. **Cloud = Renting servers instead of owning them**
2. **AWS = One of the biggest cloud providers**
3. **Security starts with setup** (MFA, IAM users, security groups)
4. **SSH = Secure way to connect to servers**
5. **GitHub = Version control + portfolio**
6. **Free tier is free IF you're careful**
7. **Real hackers attack real servers** (I have evidence!)

## What This Enables

With this foundation, i can:
-  Launch cloud infrastructure
-  Connect securely to servers
-  Understand cloud security basics
-  Document your work professionally
-  Investigate security incidents

## Status

**Cloud Basics: MASTERED** 

Ready for: Phase 2 (IAM Deep Dive)
