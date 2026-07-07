# CIA Triad Framework: Security Foundation

Complete documentation of CIA Triad learning and how it applies to cloud security.

## What is CIA Triad?

CIA Triad is the FOUNDATION of all information security.

It answers THREE fundamental questions:

1. **C = Confidentiality:** Who can see this data?
2. **I = Integrity:** Has this data been tampered with?
3. **A = Availability:** Can I access it when I need it?

**Everything in security flows from these three concepts.**

## C = CONFIDENTIALITY (Keeping Secrets)

### Definition
Only authorized people can see or access data.

### Simple Analogy
You have a diary with your secrets and only YOU should read it.
If a stranger reads it:
Confidentiality is BROKEN

### Real World Examples

**Broken Confidentiality:**
- Hacker steals password
- Hacker accesses email
- Your secrets are exposed 

- Company database hacked
- Customer credit cards leaked
- Business reputation destroyed 

- Employee reads another employee's emails
- Private information exposed
- Trust broken 

**Protected Confidentiality:**
- Password protected accounts
- Encryption scrambles data
- IAM permissions limit access
- Only authorized people can see 

### How to Protect Confidentiality

#### 1. Access Control (IAM)

Developer_test can see:
- Dev database
- Dev S3 buckets
Developer_test CANNOT see:
- Production database
- Finance records
- Customer data
Result: Confidentiality maintained through least privilege

#### 2. Encryption
Data at rest (stored): Files encrypted
Hacker gets file: Can't read it (encrypted!)
Data in transit (moving): HTTPS encryption
Hacker intercepts: Can't read it (encrypted!)

#### 3. Passwords & MFA
Password: Only YOU know it, Proves you are who you say
MFA: Password + phone
Even if password stolen: Need phone too
Double protection!

#### 4. Audit Logs (CloudTrail)
Who accessed what?
When did they access it?
What did they do?
If confidentiality broken:
- Can prove: "Admin user at 3:15 PM accessed customer data"
- Can trace: Who, what, when
- Can respond: Delete that user

### Confidentiality in my Work

**IAM Users i Created:**

**developer_test:**
-  Can see: Dev database, dev S3
-  Cannot see: Production, finances, IAM
- Result: Confidentiality maintained!

**auditor_test:**
-  Can see: Everything (ViewOnlyAccess)
-  Cannot modify: Anything (read-only)
- Result: Can audit without exposing secrets to others!

**Mycloudlab (Admin):**
-  Can see: Everything
-  But: Protected by strong password + MFA
- Result: Confidentiality through access control

## I = INTEGRITY (Data Hasn't Been Tampered)

### Definition
Data is accurate and hasn't been modified without authorization.

### Simple Analogy
You sign a contract and someone modifies it secretly, you check and contract doesn't match what you signed.
Integrity is BROKEN

### Real-World Examples

**Broken Integrity:**
- Hacker modifies bank balance
- $100 becomes $10,000
- You notice discrepancy 

- Hacker deletes log files
- Evidence of attack is gone
- You can't prove what happened

- Employee modifies audit report
- False data presented
- Wrong decisions made 

**Protected Integrity:**
- Audit logs record all changes
- Can prove: "User modified database at 3:15 PM"
- Version control tracks changes
- Hash verification detects tampering 

### How to Protect Integrity

#### 1. Audit Logs (CloudTrail)

Every action logged:
- WHO: ec2-user
- WHAT: DeleteObject
- WHEN: June 6, 2:32 PM
- WHERE: s3://production-data
- RESULT: Success

If data modified:
- CloudTrail shows: "Who changed it and when"
- Can prove: Changes made by hacker
- Can restore: From backup before change

#### 2. Version Control

File version 1: Original

File version 2: Modification 1

File version 3: Modification 2
If File 3 is bad:

Can restore File 2

Can track who modified

Can see what changed (diff)

#### 3. Checksums (Hash Fingerprints)

Original file checksum: 4a7f8e3d2b1c9a6f

File after modification: 5b8g9f4e3c2d0b7g
Checksums different!

File was tampered with!

Integrity broken! 

#### 4. Access Controls

Only authorized users can modify:
- Developer_test CANNOT modify production
- Finance manager CANNOT modify code
- Auditor CANNOT modify anything
Result: Only trusted people can change data

#### 5. Digital Signatures

- Signer: "This is my document"
- Signature: Cryptographic proof
- Receiver: Verifies signature
- Result: Proves authenticity and that it hasn't changed

### Integrity in My Work

**CloudTrail Logs I Found:**

journalctl | grep -i "admin"

Shows: 18 failed admin login attempts

Proof: Someone tried to break in
This was seen in real time, so no mistakes were made.

**Audit Trail Benefits:**
-  Proves what happened
-  Shows who did it
-  Timestamps prevent fake claims
-  Protects innocent people (proves innocence!)

**Example Scenario:**
Dtabase was modified, suspect says they did not do it but cloudtrail shows proof of it being that particular person with specific time and date of the incident.

Crime: Database modified

Suspect: "It wasn't me!"

Proof: CloudTrail shows user "alice" at 3:15 PM modified it

Reality: Alice is guilty (has evidence)

Without CloudTrail:
- Could be anyone
- Can't prove guilt
- Can't defend innocent

With CloudTrail:
- Can prove exactly who
- Can track actions
- Can respond appropriately

## A = AVAILABILITY (Access When Needed)

### Definition
Authorized users can access resources when they need them.

### Simple Analogy
You have money in bank and the bank is closed 24/7, you can not access money when needed
Availability is BROKEN
BUT;
When bank is open 24/7, custoer can access anytime
Availability is GOOD 

### Real-World Examples

**Broken Availability:**
- Server crashes
- Website offline
- Customers can't shop
- Business loses money 

- Ransomware encrypts databases
- Can't access patient records
- Hospital can't operate
- Critical situation 

- Data center burns down
- All backups lost
- No recovery possible
- Business shuts down 

**Protected Availability:**
- Multiple servers (redundancy)
- Automatic failover
- Regular backups in different locations
- Disaster recovery plan 

### How to Protect Availability

#### 1. Redundancy (Multiple Copies)

One server fails:

Customer: "This is bad"
Three servers (load balanced): Server 1 fails → Server 2 serves traffic; Server 2 fails → Server 3 serves traffic

Customer: "Back online"
Result: System keeps running even with failures

#### 2. Backups

Production data: Primary

Backup 1: Secondary location

Backup 2: Off-site location

Backup 3: AWS S3 (geo-redundant)

If disaster occurs:
You can always restore from backup, data is recovered and business continues

#### 3. Disaster Recovery Plan

When disaster happens:
- Alert team (monitoring)
- Switch to backup (failover)
- Restore data (recovery)
- Verify integrity (testing)
- Resume normal operations

RTO (Recovery Time Objective): 1 hour

RPO (Recovery Point Objective): 15 minutes

Business can tolerate up to 1 hour downtime

Willing to lose up to 15 min of data

#### 4. Monitoring & Alerts

Server starts failing:

Monitoring detects: CPU 95%, Memory 90%

Alert triggers: Email, SMS, page on-call

Team responds: Within 5 minutes

Problem fixed: Before customer notices

Result: Proactive availability protection

#### 5. Auto-Scaling

Traffic increases:

Load balancer: "Need more servers"

Auto-scaling: "Launch 2 more servers"

Capacity added: Within 1 minute

Availability maintained: No slowdown 

### Availability in my Work

**AWS Free Tier Considerations:**

Instance running 24/7:

750 hours/month × $0.01/hour = $7.50/month
Best practice:

Stop instance when not using

Start when needed

Saves money + keeps free tier valid

**Backup Strategy:**

Your EC2 instance:

- In AWS (protected by AWS)
- But: Single instance = single point of failure

Better:

- Instance + snapshot backup
- Snapshots in multiple regions
- Can restore quickly if needed

## How CIA Triad Works Together

### The Three Pillars

CONFIDENTIALITY
          ▲
          │
Keep      │      Prevent
secrets   │      exposure
          │
────────────────────────
          │
          │      Prove
          ▼      accuracy
      INTEGRITY
      
          │
          │      Enable
          │      access
          ▼
    AVAILABILITY

    ### Real Security Incident Example

**Scenario: DevOps account hacked**

#### Confidentiality Impact

Hacker can:

- Access production database (confidential data exposed)
- Access S3 buckets (files exposed)
- Access customer data (privacy breach)
- Create new admin user (backdoor for future access)
Confidentiality:  BROKEN

#### Integrity Impact

Hacker can:

- Modify database records
- Delete log files (hide tracks!)
- Create fake records
- Plant malware in code
Integrity: BROKEN

#### Availability Impact

Hacker can:

- Delete production database
- Delete backups
- Shutdown servers
- Launch DDoS attack
Availability:  SEVERELY BROKEN

#### Response Using CIA
Detection:

CloudTrail shows unusual actions (Integrity proof!)
Alerts trigger on deletion attempts (Availability monitoring)

Response:

Delete hacker's account (Confidentiality restored)

Review logs (Integrity verification)

Restore from backup (Availability recovery)

## CIA Triad Tradeoffs

### Confidentiality vs Availability

Super secure (no access):

- Confidentiality: Perfect
- Availability: Zero (can't use it!)

Too open (everyone has access):

- Confidentiality: Broken

- Availability: Perfect

Balance:

- Confidentiality: Good (least privilege)

- Availability: Good (users can access what they need)

### Security vs Convenience
Maximum security (10 factor authentication):

- Very secure

- Users frustrated (can't log in easily)

- Productivity drops

No security (password only):

- Easy to use

- Gets hacked constantly

- Disaster

Balance:

- MFA (2 to 3 factors)
- Strong passwords
- Users can work efficiently

### Cost vs Security
No security measures:

Free
Gets hacked = $1,000,000 loss

Enterprise security:

Expensive
Protected = Business survives

Balance:

Investment in security
Proportional to what you're protecting
Startup != Fortune 500 level of security

## CIA Triad in Different Scenarios

### Scenario 1: E-Commerce Website

**Confidentiality:**
- Customer credit cards encrypted
- Only payment processor sees raw numbers
- PCI-DSS compliance (payment standard)

**Integrity:**
- Price can't be secretly modified
- Audit logs track all changes
- Database integrity checks

**Availability:**
- Site up 99.9% (almost always available)
- Multiple servers + load balancing
- Disaster recovery ready

### Scenario 2: Hospital System

**Confidentiality:**
- Patient records encrypted
- HIPAA compliance
- Only authorized doctors see records

**Integrity:**
- Medical records can't be modified
- Audit trails for every access
- Signatures on changes

**Availability:**
- 99.99% uptime (nearly perfect)
- Emergency power backups
- Off-site disaster recovery
- (Lives depend on it!)

### Scenario 3: Startup 

**Confidentiality:**
- Basic encryption
- IAM for access control
- Limited sensitive data initially

**Integrity:**
- CloudTrail logging
- GitHub version control
- Basic backup strategy

**Availability:**
- Single server okay to start
- Grow redundancy as business grows
- Incremental improvements

## CIA Triad and My IAM Design

### How My Structure Protects CIA
CONFIDENTIALITY:

developer_test limited access

auditor_test read only

Finance manager only sees billing

CEO admin access

→ Each person sees ONLY what they need

INTEGRITY:

CloudTrail logs all IAM actions

Policies can't be modified by developers

Audit trail proves who did what

→ Changes are tracked and verifiable

AVAILABILITY:

Users can access what they need

Policies don't block legitimate work

System is responsive and accessible

→ Business operations continue

## Key Insights

### 1. CIA Isn't Perfect
Every security measure has tradeoffs. You balance all three rather than maximizing one.

### 2. CIA Guides All Decisions
When designing security:
- Ask: How do I protect confidentiality?
- Ask: How do I maintain integrity?
- Ask: How do I ensure availability?

### 3. CIA Applies Everywhere
- User passwords: Confidentiality
- Audit logs: Integrity
- Disaster recovery: Availability
- Every security tool maps to CIA

### 4. Defense in Depth
Use multiple layers for each CIA:
- Confidentiality: Encryption + access control + MFA
- Integrity: Logs + versioning + signatures
- Availability: Backups + redundancy + monitoring

### 5. Real Security is CIA
When you see "hacker" news:
- Usually confidentiality broken (data stolen)
- Often integrity broken (data modified)
- Sometimes availability broken (systems down)

## Summary: CIA Triad Framework

| Pillar | What | How | Why |
|--------|------|-----|-----|
| **Confidentiality** | Keep secrets | Access control, encryption | Only authorized people see data |
| **Integrity** | Prevent tampering | Audit logs, versioning | Data is accurate and trustworthy |
| **Availability** | Ensure access | Redundancy, backups | Users can access when needed |

## Status

**CIA Triad Framework: MASTERED** 

**Key Learning:** CIA Triad is THE framework for security engineering.
Every decision you make in security relates back to these three pillars.
