# Cloud Architecture Models: IaaS, PaaS, SaaS

Complete documentation of cloud service models and how to choose between them.

## The Three Cloud Models

Cloud computing is organized into three main models based on **WHO manages WHAT**:

SaaS
            (Software)
          User just uses it
         (Gmail, Slack, Teams)
                
               PaaS
            (Platform)
        User writes code
      AWS manages platform
       (RDS, Lambda, Heroku)
       
              IaaS
         (Infrastructure)
      User manages everything
    AWS provides raw resources
       (EC2, S3, VPC, EBS)
       
          On-Premises
       User owns everything
       (Old data centers)

## IaaS (Infrastructure as a Service)

### Definition
AWS provides the **infrastructure** (servers, storage, networking).  
**You manage everything else.**

### what AWS provides 

- Physical servers (you don't see them)
- Data center (location)
- Network infrastructure
- Storage systems
- Power and cooling

### What YOU Provide

- Operating system (you install Linux/Windows)
- Database engine (you install MySQL/PostgreSQL)
- Application code (you write it)
- Security patches (you apply them)
- Backups (you configure them)
- Monitoring (you set it up)
- Disaster recovery (you design it)

### Real AWS IaaS Examples

EC2 (Elastic Compute Cloud):

Servers you can launch
You control the OS
You install everything

S3 (Simple Storage Service):

Raw storage buckets
You manage what goes in
You set permissions

VPC (Virtual Private Cloud):

Virtual network you create
You design the architecture
You configure security

EBS (Elastic Block Store):

Hard drives for your instances
You manage them


### IaaS Analogy
Renting an EMPTY apartment:

Landlord provides: Building, walls, doors, utilities
You provide: Furniture, decoration, maintenance
You're responsible for: Everything inside!


### Responsibility Matrix - IaaS

| Component | AWS | You |
|-----------|-----|-----|
| Physical servers | ✅ | ❌ |
| Network infrastructure | ✅ | ❌ |
| Operating system | ❌ | ✅ |
| Database engine | ❌ | ✅ |
| Application code | ❌ | ✅ |
| Security patches | ❌ | ✅ |
| Backups | ❌ | ✅ |
| Monitoring | ❌ | ✅ |
| **Total responsibility** | **20%** | **80%** |

### Security in IaaS

**YOU are responsible for:**
✅ OS security patches (keep updated!)

✅ Application security (write secure code)

✅ Firewall rules (security groups)

✅ Encryption (encrypt data)

✅ Access control (IAM + OS users)

✅ Monitoring (detect breaches)

✅ Backups (disaster recovery)

**If you fail:**
❌ Unpatched OS → Hacker exploits vulnerability

❌ Bad firewall → Hacker gets in

❌ No encryption → Data stolen

❌ No monitoring → Breach goes undetected

❌ No backups → Data lost forever

### When to Use IaaS

✅ **Use IaaS when:**
- Maximum control needed
- Custom applications
- Unique requirements
- Experienced ops team
- Complex infrastructure

❌ **Don't use IaaS when:**
- Small startup, small team
- Standard applications
- Want minimal security burden
- Don't have DevOps expertise

### IaaS Cost Reality
EC2 t3.micro (smallest):

Free tier: 750 hours/month
After free tier: ~$0.01/hour
Monthly cost: $7-10/month

But if you launch 5 instances by accident:

$35-50/month
Still low, but adds up!

Production-grade instance:

m5.xlarge: ~$0.20/hour
Monthly: ~$150/month
For real workloads!


---

## PaaS (Platform as a Service)

### Definition
AWS provides the **platform** (infrastructure + OS + runtime + database engine).  
**You provide the application and data.**

### What AWS Provides
✅ Physical servers

✅ Network infrastructure

✅ Operating system

✅ Database engine (running)

✅ Runtime environment

✅ Security patches (automatic!)

✅ Backups (automatic!)

### What YOU Provide
❌ Your application code (you write it)

❌ Your data (you manage it)

❌ Application configuration (you set it)

❌ Access control (you define who accesses)

❌ Sometimes: Data encryption (depends)

### Real AWS PaaS Examples
RDS (Relational Database Service):

MySQL, PostgreSQL, Oracle running
You don't manage the engine
Just use it like a service

Lambda (Serverless Functions):

Write code, upload it
AWS runs it
You don't manage servers

Elastic Beanstalk:

Upload your code
AWS deploys it
Auto-scaling included

DynamoDB:

NoSQL database
Fully managed
You store data, AWS manages it


### PaaS Analogy
Renting a FURNISHED apartment:

Landlord provides: Building, furniture, utilities
You provide: Your belongings, decoration
You don't maintain the furniture
Landlord handles repairs


### Responsibility Matrix - PaaS

| Component | AWS | You |
|-----------|-----|-----|
| Physical servers | ✅ | ❌ |
| Network infrastructure | ✅ | ❌ |
| Operating system | ✅ | ❌ |
| Database engine | ✅ | ❌ |
| Application code | ❌ | ✅ |
| Security patches | ✅ | ❌ |
| Backups | ✅ | ❌ |
| Monitoring | ✅ | ❌ |
| Your data security | ❌ | ✅ |
| Access control | ❌ | ✅ |
| **Total responsibility** | **50%** | **50%** |

### Security in PaaS

**AWS handles:**
- OS security patches (automatic)

- Database engine security (automatic)

- Infrastructure security

- Physical security

- Automatic backups

**You handle:**
- Application security (write secure code)

- Data encryption (sometimes)

- Access control (IAM roles)

- Monitoring application health

**Risk:**
If AWS has vulnerability:

You're affected (but usually patch fast)
Limited control on timeline

If your app has vulnerability:

You're at fault
You must fix it


### When to Use PaaS

✅ **Use PaaS when:**
- Focus on code, not infrastructure
- Standard database needs
- Want AWS to handle operations
- Startup with small team
- Need to move fast

❌ **Don't use PaaS when:**
- Need complete custom control
- Have legacy systems
- Need specific OS kernel access
- Enterprise with strict standards

### PaaS Cost Reality
RDS (MySQL, smallest):

Free tier: 750 hours db.t3.micro
After free tier: ~$0.02/hour
Monthly: $15-20/month

Better than IaaS because:

AWS handles backups (you don't pay extra)
AWS handles patches (no downtime)
AWS handles failover (automatic)


---

## SaaS (Software as a Service)

### Definition
Company provides the **complete application**.  
**You just use it.**

### What Company Provides
✅ Everything (servers, OS, app, security, backups, updates)

✅ Just works

✅ No management needed

### What YOU Provide
❌ Just use it

❌ Provide your data

❌ Pay the bill

### Real SaaS Examples
Gmail:

Google provides: Everything
You provide: Emails to write
You manage: Your password, your contacts

Slack:

Slack provides: Chat infrastructure, security, uptime
You provide: Team members, messages
You manage: Your password, channel membership

Microsoft 365 (Office):

Microsoft provides: Word, Excel, Office
You provide: Your documents
You manage: Your password, sharing settings

Salesforce:

Salesforce provides: CRM platform
You provide: Customer data
You manage: Your password, data


### SaaS Analogy
Eating at a RESTAURANT:

Restaurant provides: Building, kitchen, chef, food, service
You provide: Money, appetite, plates back
You don't manage: Anything


### Responsibility Matrix - SaaS

| Component | Company | You |
|-----------|---------|-----|
| Physical servers | ✅ | ❌ |
| Network | ✅ | ❌ |
| Operating system | ✅ | ❌ |
| Application | ✅ | ❌ |
| Security patches | ✅ | ❌ |
| Backups | ✅ | ❌ |
| Monitoring | ✅ | ❌ |
| Your password | ❌ | ✅ |
| Your data | ❌ | ✅ |
| Access control | ❌ | ✅ |
| **Total responsibility** | **90%** | **10%** |

### Security in SaaS

**Company handles:**
- Everything (literally everything)

- Security patches (automatic)

- Backups (automatic)

- Uptime (99.9%+ guaranteed)

- Encryption (at rest and in transit)

**You handle:**
- Your password (don't share it!)

- Your data (what you upload)

- Permissions (who sees what in your account)

**Risk:**
If company gets hacked:

Your data exposed
You trust them, but you're at their mercy

If you use weak password:

Hacker gets in, sees your data
Company's security doesn't matter if password is "123456"


### When to Use SaaS

✅ **Use SaaS when:**
- Standard needs (email, chat, docs)
- No special requirements
- Want zero infrastructure management
- Trust the company
- Want rapid deployment

❌ **Don't use SaaS when:**
- Need data on-premises (sovereignty)
- Have extreme compliance needs
- Need custom functionality
- Can't trust third-party with data
- Company might go out of business

---

## Comparison: IaaS vs PaaS vs SaaS

### Control vs Responsibility Tradeoff
IaaS:

Control: 100% (you manage everything)
Responsibility: 80% (you do most work)
Complexity: HIGH

PaaS:

Control: 50% (shared responsibility)
Responsibility: 50% (you manage app/data)
Complexity: MEDIUM

SaaS:

Control: 10% (company controls)
Responsibility: 10% (you just use)
Complexity: LOW


### Quick Decision Matrix
Question: Do you need to install an OS?

YES → IaaS (EC2)
NO, but need database → PaaS (RDS)
NO, just need app → SaaS (Gmail, Slack)

Question: Who manages security patches?

You → IaaS
AWS → PaaS
Company → SaaS

Question: Can you customize the platform?

Fully → IaaS
Somewhat → PaaS
Not at all → SaaS


---

## Real Startup Architecture Decision

### Your Startup Needs

**Team:**
- 5 developers (write code)
- 2 DevOps (manage infrastructure)
- 1 security engineer (you!)
- Limited budget (startup)

**Requirements:**
- Web application
- Database for user data
- File storage
- Team communication

### Architecture Decisions You Made
Code storage:

GitHub (SaaS)

Why? Focus on code, not version control
Cost? Free tier available
Control? Limited (but don't need it)

Production database:

RDS (PaaS)

Why? Don't want to manage MySQL
Cost? Reasonable ($15-20/month)
Control? Enough (can access, but AWS handles patches)

File storage:

S3 (IaaS)

Why? Simple, reliable, cheap
Cost? Pay for what you use
Control? Full control of permissions

Team communication:

Slack (SaaS)

Why? Professional, reliable
Cost? $150/month for team
Control? Limited (but don't need it)

Servers for app:

EC2 (IaaS)

Why? Custom application
Cost? Free tier while learning
Control? Full control

Monitoring:

CloudWatch (PaaS)

Why? AWS integrated
Cost? Included
Control? Configured by you


### The Pattern
Use SaaS for: Generic needs (communication, email)

Use PaaS for: Managed services (database, hosting)

Use IaaS for: Custom needs (your application)
Result: Maximum control where needed

Minimum burden where not needed

Balanced cost

---

## On-Premises vs Cloud

### On-Premises (Old Way)
Company buys servers, installs in office:

✅ Full control

✅ Data stays local

✅ No internet dependency

❌ Huge upfront cost ($100k+)

❌ Need IT team to manage

❌ Expensive to scale

❌ Disaster recovery complex

❌ Maintenance burden

### Cloud (New Way)
Rent from AWS:

✅ Pay as you go

✅ Easy to scale

✅ AWS handles infrastructure

✅ Disaster recovery easy (geographic redundancy)

❌ Recurring costs

❌ Internet dependency

❌ Less control

❌ Trust third-party

### The Trend
2000s: Most companies used on-premises

2010s: Shift to cloud beginning

2020s: Cloud is default

2025: On-premises is exception (special cases only)

---

## Your Learning: Cloud Architecture Models

### What You Understand Now

- IaaS: Full control, full responsibility (EC2, S3)  
- PaaS: Shared responsibility (RDS, Lambda)  
- SaaS: Minimal responsibility (GitHub, Slack)  
- Trade-offs: Control vs Simplicity  
- When to use each: Based on needs  

### How CIA Triad Applies
IaaS (You manage security):

Confidentiality: YOU encrypt + YOU set access
Integrity: YOU set up audit logs
Availability: YOU set up backups + redundancy

PaaS (Shared responsibility):

Confidentiality: AWS handles infrastructure, you handle access
Integrity: AWS logs infrastructure, you verify data
Availability: AWS handles server uptime, you handle app

SaaS (Company manages):

Confidentiality: Company handles everything
Integrity: Company proves with compliance certs
Availability: Company guarantees with SLA


---

## Summary: Cloud Architecture Models

| Aspect | IaaS | PaaS | SaaS |
|--------|------|------|------|
| **You manage** | OS, DB, App, Security, Backups | App, Data, Access | Password only |
| **AWS/Company manages** | Hardware, Network | Infrastructure, Patches | Everything |
| **Control level** | Maximum | Medium | Minimum |
| **Complexity** | High | Medium | Low |
| **Cost predictability** | Variable | Medium | Fixed |
| **Best for** | Custom apps | Managed databases | Standard tools |
| **Examples** | EC2, S3, VPC | RDS, Lambda | Gmail, Slack |

---

## Real-World Application

### Your EC2 Instance = IaaS
You launched EC2 (raw server)

You installed Linux

You manage everything on it

AWS manages: Physical server, power, cooling

### If you used RDS = PaaS
You'd create database

Database just works

AWS manages: Database engine, patches, backups

You manage: Data, access control

### If you used Gmail = SaaS
You sign up

It works

Google manages: Everything

You manage: Password, your emails

---

## Key Insights

1. **Cloud isn't one thing** - It's a spectrum (IaaS → PaaS → SaaS)
2. **More managed = Less control but less work**
3. **More control = More power but more responsibility**
4. **Choose based on needs** - Not all applications fit one model
5. **Startups usually mix** - SaaS for communication, PaaS for database, IaaS for app
6. **Security applies to all** - But who's responsible varies

---

## Status

**Cloud Architecture Models: MASTERED** ✅

You can now:
-  Understand any cloud service
-  Know who's responsible for security
-  Design appropriate architecture
-  Make informed tool choices

---
