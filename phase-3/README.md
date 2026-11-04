# Phase 3: Network Security

In this phase I learned how traffic reaches (and is blocked from) cloud resources, and I built a small network that separates a public web tier from a private database tier.

---

## What I Built

- **Reviewed the default VPC:** One VPC (172.31.0.0/16) with six subnets, one per Availability Zone in us-east-1. My EC2 instance runs in us-east-1c.
- **Built `lab-vpc`:** A custom VPC (10.0.0.0/16) with one public subnet and one private subnet in us-east-1a.
- **Route tables:**
  - `lab-rtb-public` sends `0.0.0.0/0` to the internet gateway (`lab-igw`), so the public subnet can reach the internet.
  - `lab-rtb-private1-us-east-1a` has no internet route. It only has a route to S3 through a VPC endpoint, so S3 traffic stays inside AWS.
  - I confirmed the public subnet is associated with `lab-rtb-public`.
- **Security groups:**
  - `lab-web-sg`: allows HTTPS (443) from anywhere, for the public web server.
  - `lab-db-sg`: allows MySQL/Aurora (3306) only from `lab-web-sg`, not from any IP address.
- **Network ACL:** Reviewed the `lab-vpc` NACL. It allows all inbound traffic with a deny-all catch-all, so the security groups do the filtering for now.

---

## Concepts I Worked Through

- **Public vs private subnet:** A subnet is public only if its route table sends internet traffic to an internet gateway. The name doesn't matter.
- **Security group vs NACL:** A security group is attached to a resource and is stateful, so return traffic is allowed automatically. A NACL is attached to a subnet and is stateless, so both directions need rules.
- **Referencing a security group instead of an IP:** The database rule says "allow from the web group," so it still works when web servers are replaced and their IPs change.
- **Defense in depth:** Network rules limit who can reach the database. Least privilege database users, hashed passwords, and encryption at rest limit what an attacker gets if the web server is compromised.

---

## Key Lessons

1. A route table decides whether a subnet is public. Check the routes and the subnet association, not the subnet name.
2. Reference security groups in rules where possible, so rules don't depend on IP addresses.
3. Don't rely on one layer. The NACL here is wide open, so the security groups are carrying the load.
4. Restrict access by source, not just by port. My SSH rule on the instance is limited to my own IP (`/32`), not `0.0.0.0/0`.

---

## Known Gaps

- The `lab-vpc` NACL still allows all inbound traffic. Restricting it is a planned improvement.
- I did not run AWS Reachability Analyzer, because it isn't free-tier and my credits are limited. The rules were verified by reading them.
- The evidence screenshots are in `phase-3/evidence/` (see the index there).

---

## Status

Phase 3 complete, with the gaps above noted.
