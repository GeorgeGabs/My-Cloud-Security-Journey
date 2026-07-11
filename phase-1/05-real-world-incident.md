# Real-World Security Incident Investigation

Documentation of discovering and analyzing a REAL brute-force attack on MY EC2 instance.

# The Discovery

### When
**Date:** June 8, 2026  
**Time:** While learning `grep` command  

### How It Happened
Learning: grep command (searching in files)

Exercise: Search system logs for patterns

Command: journalctl | grep -i "admin"
Result: Found REAL attack activity!

### The Realization
Expected: Theoretical understanding of logs

Actual: Real hackers attacking RIGHT NOW

Feeling: "This is actually happening!"

---

## The Evidence

### Command Used
```bash
journalctl | grep -i "admin" | head -20
```

**Breaking it down:**
- `journalctl` = Show system logs
- `|` = Pipe (send to next command)
- `grep -i "admin"` = Find lines with "admin" (case-insensitive)
- `| head -20` = Show only first 20 lines

### Raw Logs Found
Invalid user admin from 34.128.95.203 port 52412

Connection closed by invalid user admin 34.128.95.203 port 52412
Invalid user mailadmin from 34.128.95.203 port 52002

Connection closed by invalid user mailadmin 34.128.95.203 port 52002
Invalid user ansadmin from 34.128.95.203 port 45654
Invalid user admin from 34.128.95.203 port 36288

Connection closed by invalid user admin 34.128.95.203 port 36288
Invalid user mailadmin from 61.3.195.40 port 48841

Connection closed by invalid user mailadmin 61.3.195.40 port 48841
Invalid user admin from 118.145.131.27 port 47242

Connection closed by invalid user admin 118.145.131.27 port 47242
Invalid user admin from 118.145.131.27 port 45090

Connection closed by invalid user admin 118.145.131.27 port 45090
Invalid user drcomadmin from 102.37.156.238 port 53222
Invalid user ftpadmin from 101.79.165.43 port 48600

---

## Attack Analysis

### Attacking IP Addresses

#### IP #1: 34.128.95.203 (PRIMARY ATTACKER)

**Activity:**
Failed attempts: 8 times

Attempted usernames:

admin (multiple times)
mailadmin (multiple times)
ansadmin (once)

Pattern:

1st attempt: admin on port 52412

2nd attempt: mailadmin on port 52002

3rd attempt: ansadmin on port 45654

4th attempt: admin again on port 36288
Strategy: Trying different usernames, cycling through ports

**Assessment:**
Behavior: Classic brute-force attack

Sophistication: Medium

Tools used: Probably automated script

Persistence: Multiple attempts (determined)

#### IP #2: 61.3.195.40

**Activity:**
Failed attempts: 1-2 times

Attempted usernames:

mailadmin (trying admin accounts)


**Assessment:**
Less active than primary attacker

Possibly separate group or automated scan

Testing credentials

#### IP #3: 118.145.131.27

**Activity:**
Failed attempts: 2-3 times

Attempted usernames:

admin (multiple times)


**Assessment:**
Consistent targeting of admin accounts

Methodical approach

#### IP #4: 102.37.156.238

**Activity:**
Failed attempts: 1 time

Attempted usernames:

drcomadmin


**Assessment:**
Minimal activity

Possible mass scanner (trying many targets)

#### IP #5: 101.79.165.43

**Activity:**
Failed attempts: 1 time

Attempted usernames:

ftpadmin


**Assessment:**
Single attempt

Mass vulnerability scanner

### Overall Pattern Summary
Total failed admin login attempts: 18

Unique attacking IPs: 5

Unique usernames tried: 6 (admin, mailadmin, ansadmin, drcomadmin, ftpadmin)

Date range: June 4, 2025 (multiple attempts over time)

Primary attacker: 34.128.95.203
---

## Attack Type Classification

### This is a BRUTE-FORCE ATTACK

#### What is brute-force?
Attacker tries MANY combinations:

Many usernames
Many passwords
Many ports
Goal: Eventually guess correctly


#### Why does it work?
If system allows unlimited attempts:

Username: "admin" + Password: "123456" → Fails

Username: "admin" + Password: "password" → Fails

Username: "admin" + Password: "admin" → Success!

(Eventually finds weak passwords)

#### Why does it fail against MY instance?

I don't have "admin" user (AWS uses ec2-user)
SSH only allows key-based auth (not passwords)
Security group limits connections
Strong password policy if enabled

Result: Attacker can't get in ✅

---

## Geographic Analysis

### Where Are These IPs?
34.128.95.203 - Likely cloud provider (Google Cloud or AWS)

61.3.195.40   - Asia-Pacific region

118.145.131.27 - Asia-Pacific region

102.37.156.238 - Europe or North Africa

101.79.165.43  - South Asia region

**Interpretation:**
IPs from cloud providers suggest:

Attackers using cloud to launch attack
Can hide real location
Can scale attack easily
Can rotate IPs if blocked


---

## Security Implications

### Threat Assessment: MEDIUM-HIGH

#### Confidentiality Risk
If attacker succeeds:

❌ SSH access to server

❌ Access to all files

❌ Access to environment variables

❌ Possible access to AWS credentials
Current status: PROTECTED ✅

(Attackers failed to gain access)

#### Integrity Risk
If attacker succeeds:

❌ Modify system files

❌ Plant malware

❌ Change configurations

❌ Alter log files
Current status: PROTECTED ✅

(Attack unsuccessful)

#### Availability Risk
If attacker succeeds:

❌ Shut down instance

❌ Delete data

❌ Disrupt services

❌ Launch attacks from this server
Current status: PROTECTED ✅

(Attack unsuccessful)

### Why I Were Protected

1. **No password authentication**
   - SSH key-based auth only
   - Brute-force password guessing doesn't work

2. **No "admin" user**
   - AWS uses "ec2-user" by default
   - Attacker trying wrong usernames

3. **Limited attempts**
   - Can't try infinite combinations
   - SSH closes connection after failures

4. **Security group**
   - Restricts who can attempt connection
   - Rate limiting possible

---

## What This Means

### Real vs Theoretical
Before: "Security is important"

After: "People are ACTIVELY trying to break in RIGHT NOW"
Mindset shift: Security is not optional

### Scale of Attack
1 instance = 18 failed attempts

1000 instances = 18,000 failed attempts

1 million instances = 18 million attempts
Attackers don't target you specifically

They scan randomly, attack broadly

### This Happens to EVERYONE
Every AWS instance receives attacks

Within hours of launch

Attackers use automated tools

This is the internet reality

---

## Response: What You SHOULD Do

### Immediate (What i Did)
✅ Discovered attack (investigation)  
✅ Analyzed patterns (understanding)  
✅ Documented findings (proof)  

### Short-term (Best Practice)

Enable CloudTrail logging (tracks all AWS API calls)
Set up CloudWatch alarms (alert on suspicious activity)
Review security group rules (verify they're restrictive)
Check EC2 key pair security (verify .pem is secure)
Consider fail2ban (blocks repeated failed attempts)


### Long-term (Production)

Implement IDS/IPS (intrusion detection/prevention)
Use VPN for SSH access (not open to internet)
Change default SSH port (obscurity helps slightly)
Implement 2FA for console access
Regular security audits
Automated threat detection


---

## Learning Outcomes

### Skills Demonstrated
✅ Using `journalctl` to access logs  
✅ Using `grep` to search large datasets  
✅ Identifying attack patterns  
✅ Analyzing log data  
✅ Understanding SSH security  
✅ Recognizing brute-force attacks  
✅ Assessing risk  
✅ Documenting findings  

### Knowledge Gained
✅ Attacks are real and constant  
✅ Logs contain evidence  
✅ Pattern recognition is critical  
✅ Security is proactive, not reactive  
✅ Monitoring is essential  
✅ Documentation proves incidents  

### Confidence Boost
Moment: Discovered real attack

Feeling: "I can do this!"

Realization: Not just theory anymore

Motivation: This matters!

---

## Tools Used

### journalctl
```bash
journalctl              # Show all system logs
journalctl -u sshd      # Show SSH logs
journalctl -n 50        # Show last 50 entries
journalctl --since "2025-06-04"  # Since date
```

### grep
```bash
grep "pattern" file     # Find matching lines
grep -i "pattern"       # Case-insensitive
grep -c "pattern"       # Count matches
grep "a\|b" file        # Multiple patterns
```

### Combination (PIPE)
```bash
journalctl | grep "admin" | wc -l    # Count admin attempts
journalctl | grep "Failed" | head -5 # Show first 5 failures
```

---

## CIA Triad Application

### How This Incident Shows CIA

**Confidentiality:**
Attack goal: Access instance (break confidentiality)

Defense: SSH key auth protects it ✅

Result: Attacker couldn't see data

**Integrity:**
Risk: If successful, could modify logs

Defense: CloudTrail (separate, immutable log) ✅

Result: Evidence preserved

**Availability:**
Risk: If successful, could shut down server

Defense: Security group + SSH key limits attack ✅

Result: Server running normally

---

## Real-World Security Lesson

### The Big Picture
You don't need to BUILD perfect security

You need to UNDERSTAND attacks

Then you can DEFEND against them
This incident taught me REAL attack patterns

Better than 100 tutorials!

### Security Mindset
Before: "Hope no one attacks"

After: "Someone is attacking RIGHT NOW"

Healthy paranoia ✅

Proactive defense ✅

---

## Documentation Value

### Why Document This?

1. **Proof of skills**
   - Employers see: Can investigate incidents
   - Portfolio shows: Real incident analysis

2. **Learning reinforcement**
   - Writing it down = memory lock
   - Explaining it = deep understanding

3. **Reference material**
   - I can refer back
   - Help others learn

4. **Career development**
   - This is a few weeks experience
   - Real job experience = interviews love this

---

## Summary

| Aspect | Finding |
|--------|---------|
| **Attack type** | Brute-force SSH attempts |
| **Number of attempts** | 18 failed admin logins |
| **Attacking IPs** | 5 unique addresses |
| **Primary attacker** | 34.128.95.203 |
| **Success rate** | 0% (all blocked) ✅ |
| **Discovery method** | journalctl + grep |
| **Investigation time** | ~2 hours |
| **Documentation** | Complete ✅ |

---

## Key Realization

### This Is Real
Not a simulation ❌

Not a test environment ❌

Not theoretical ❌
REAL hackers ✅

REAL attack attempts ✅

REAL security incident ✅

REAL defense success ✅

### You Protected Yourself
Did i intentionally defend against attack? No

Did security architecture protect me? YES ✅
This proves: Good design works automatically!

---

## Status

**Real-World Incident Investigation: COMPLETE** ✅

I can now:
- ✅ Access and understand system logs
- ✅ Identify attack patterns
- ✅ Analyze security incidents
- ✅ Understand real threats
- ✅ Document findings professionally
