# Phase 3 - Lab Evidence

Screenshots from the Phase 3 network security work: reviewing the default VPC, building a public and private subnet, checking route tables, and configuring security groups and the network ACL.

Sensitive values (account ID, VPC and subnet IDs, security group IDs, rule IDs, and IP addresses) are blacked out.

---

## Screenshot Index

### 1. Default VPC (`phase-3-01-default-vpc.jpg`)
- **Shows:** The default VPC with CIDR 172.31.0.0/16 in the Available state.
- **Proves:** I can identify the network my instance runs in.

### 2. Default VPC Subnets (`phase-3-02-default-subnets.jpg`)
- **Shows:** Six subnets, one in each us-east-1 Availability Zone.
- **Proves:** I understand that subnets are split by zone.

### 3. Instance Networking (`phase-3-03-instance-networking.jpg`)
- **Shows:** The EC2 instance's Networking tab, with its VPC, subnet, and us-east-1c zone.
- **Proves:** I can trace an instance to its network and subnet.

### 4. SSH Security Group Rule (`phase-3-04-ssh-sg-rule.jpg`)
- **Shows:** The `launch-wizard-1` group with a single SSH (port 22) inbound rule.
- **Proves:** I checked which ports are open to which sources.

### 5. Lab VPC Details (`phase-3-05-lab-vpc-details.jpg`)
- **Shows:** `lab-vpc` with CIDR 10.0.0.0/16, state Available, and Default VPC set to No.
- **Proves:** I built a custom VPC instead of relying on the default one.

### 6. Resource Map (`phase-3-06-resource-map.jpg`)
- **Shows:** The public and private subnets, their route tables, and the internet gateway and S3 endpoint connections.
- **Proves:** I can read how the parts of a VPC connect.

### 7. Private Route Table (`phase-3-07-private-route-table.jpg`)
- **Shows:** `lab-rtb-private1-us-east-1a` with a local route and an S3 endpoint route, and no internet route.
- **Proves:** The private subnet cannot reach the internet.

### 8. Public Route Table (`phase-3-08-public-route-table.jpg`)
- **Shows:** `lab-rtb-public` with a local route and a `0.0.0.0/0` route to the internet gateway.
- **Proves:** Resources in the public subnet can reach the internet.

### 9. Web Security Group (`phase-3-09-web-sg-rules.jpg`)
- **Shows:** `lab-web-sg` allowing HTTPS (443) from 0.0.0.0/0.
- **Proves:** The public web tier accepts HTTPS from anywhere.

### 10. Database Security Group (`phase-3-10-db-sg-rules.jpg`)
- **Shows:** `lab-db-sg` allowing MySQL/Aurora (3306) with the source set to `lab-web-sg`, not an IP address.
- **Proves:** Only the web tier can reach the database.

### 11. Network ACL (`phase-3-11-network-acl.jpg`)
- **Shows:** The `lab-vpc` network ACL with an allow-all inbound rule and a deny-all catch-all.
- **Proves:** The NACL is open for now, and the security groups do the filtering. This is noted as a known gap in the overview.

---

**Status:** Phase 3 evidence complete
