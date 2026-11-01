# Phase 2: Identity and Access Management (IAM)

In this phase I learned how AWS controls who can do what, and I built and tested an IAM structure for a small startup.

## What I Built:

- **10 IAM users in total:** Mycloudlab (my personal admin user, which I use instead of the root account), 1 CEO, 5 developers (developer_test1 to developer_test5), 2 DevOps engineers, and 1 finance manager.
- **5 IAM groups:** CEO_group, developers_group, DevOps_engineers_group, Financemanager_group and Mycloudlab. Each group gets its permissions once, and every user in it inherits them.
- **1 IAM role:** EC2-S3-Access-Role, attached to my EC2 instance so it can reach S3 without access keys.
- **MFA:** I enabled an authenticator app on developer_test1 and tested a login that required the 6-digit code.
- **Access keys:** I created a key for developer_test1, configured the AWS CLI with it, then rotated to a new key and deleted the old one.This particular bit was a bit confusing at first, but i managed to pull through.

## Concepts I Worked Through:

- **Users vs roles:** Users are for people who log in. Roles are for services and applications that need temporary access.
- **Least privilege:** Each group gets only what its job needs. For example, developers can't touch billing or IAM, and the finance manager has read-only access to billing.
- **MFA:** A stolen password isn't enough, because the attacker also needs my phone. That is why every IAM user should enable MFA in their accounts.
- **Access keys:** These are for programs and CLI tools. I store them in a secure place, never in code, and rotate them about every 90 days.
- **Temporary elevated access:** If a developer needs DevOps permissions for a short task, I can give them a role to assume for a limited time, then remove it.

## Key Lessons

1. Give each person a group, not one-off permissions.
2. Users should set up their own MFA. An admin shouldn't hold a user's MFA secret.
3. Don't screenshot access keys or save them in plain text. Use a password manager or Secrets Manager.
4. CloudTrail records who did what in IAM. I used the wrong name for it at first, and I now know the difference between CloudTrail (audit trail) and CloudWatch (monitoring).

## Status

Phase 2 labs and the IAM design challenge are complete. Evidence screenshots are in `phase-2/evidence/` (added in the next step).
