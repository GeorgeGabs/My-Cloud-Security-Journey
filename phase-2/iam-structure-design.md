# IAM Structure Design

How I would set up IAM for a small startup with 5 developers, 2 DevOps engineers, 1 finance manager, and a CEO, using least privilege.

---

## 1. Groups

I created 5 groups so permissions are assigned once per role, not once per person:

| Group | Members | Why it exists |
|---|---|---|
| CEO_group | CEO | The owner has full access, but only uses it when necessary |
| developers_group | 5 developers | Everyone doing development gets the same access. New hires just join the group |
| DevOps_engineers_group | 2 DevOps engineers | Infrastructure and deployment access, kept separate from billing and IAM |
| Financemanager_group | 1 finance manager | Billing and cost visibility only |
| Mycloudlab | Me (personal admin) | My day-to-day admin account, used instead of the root account |

---

## 2. Users vs Roles

- **IAM users** are for people who log in: the CEO, developers, DevOps engineers, and the finance manager. Each person gets a user, so their actions are traceable in CloudTrail.
- **IAM roles** are for services and applications. For example, EC2 uses a role to reach S3, and Lambda uses a role to reach DynamoDB. A role gives temporary credentials, so there are no long lived keys to steal.
- **Temporary escalation** also uses roles. If a developer needs DevOps access for a task, they assume a role for a limited time, and the access ends when the session expires.

---

## 3. Least Privilege Per Group

**DevOps_engineers_group**
- Can have access to: EC2, S3, RDS, CodeDeploy, CodePipeline, CloudWatch (full access), IAM read-only, Secrets Manager read/write
- Cannot have access to: full IAM access, billing, creating IAM users, full CloudFormation access
- Why: they run the infrastructure, but they can't grant themselves more permissions or change billing

**developers_group**
- Can have access to: DynamoDB read-only, EC2 read-only, S3 full access, CodeCommit power user
- Cannot have access to: billing, IAM, or full EC2 access
- Why: they can read infrastructure and push code, but they can't change or delete production resources

**Financemanager_group**
- Can have access to: billing read-only, budgets read-only, Cost Optimization Hub read-only
- Cannot have access to: any infrastructure or IAM access
- Why: they can see costs without touching systems. If the company trusts this person more, billing full access can be granted later

**CEO_group**
- Can have access to: administrator access through an IAM user
- Cannot have access to: nothing is restricted by policy, this account is used only when needed by the CEO of that company
- Why: the owner needs full control, and I'd keep this user for emergencies rather than daily work

---

## 4. MFA and Access Keys

- **MFA:** Every IAM user has MFA. If a password leaks, the attacker still needs the person's phone.
- **Access keys:** Only for programs and CLI tools outside AWS. For services inside AWS, I'd use roles instead.
- **Rotation:** Access keys rotate about every 90 days, or immediately if they might be exposed.

---

## 5. Security Incident Response

**Scenario:** A developer's laptop is compromised, and the attacker has the developer's password.

- **What the attacker can do:** Read DynamoDB, read EC2 instances, access S3 broadly, and use CodeCommit at power-user level.
- **What the attacker cannot do:** Access billing, view or create IAM users, delete production databases, or launch EC2 instances.
- **Response:** Immediately disable or delete the affected user, review CloudTrail for what they touched, and reset any credentials they could have seen.
- **How MFA helps:** Without the developer's phone, the attacker can't complete login, even with the password.

---

## 6. Temporary Elevated Access

**Example:** A developer needs to debug production with DevOps permissions.

1. Create a role (for example, `production-deploy-role`) with only the permissions needed.
2. Allow the specific developer to assume that role.
3. The developer assumes the role and receives temporary credentials.
4. The session expires after a set time, which I'd keep to about 24 hours at most.
5. The developer returns to their normal permissions. Actions taken during the session are logged in CloudTrail.

---

## Status

Design complete. Lab evidence for the structure is in `phase-2/evidence/`.
