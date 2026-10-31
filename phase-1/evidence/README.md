# Phase 1: Lab Evidence 

These screenshots are the proof for the Phase 1 hands on labs: launching an EC2 instance, connecting over SSH, practicing Linux commands, and investigating real login attempts in the system logs.

Each screenshot is listed in the order I took it, with what it shows and what it proves. Sensitive values (account ID, public IP, key file name, instance ID, and Windows username) are blurred.

## Screenshot Index

### 1. Security Group: SSH Rule (`phase-1-01-security-group-ssh.jpg`)
- **Shows:** The security group `launch-wizard-1` with one inbound rule: SSH, TCP, port 22, IPv4.
- **Proves:** I understood that a security group acts as a firewall, and I checked which port was open.

### 2. Home Directory Listing (`phase-1-02-ls-la-home.jpg`)
- **Shows:** `pwd` and `ls -la` in `/home/ec2-user`, including hidden files such as `.bashrc`, `.ssh`, and `.aws`.
- **Proves:** I used `ls -la` to see hidden files and read permissions.

### 3. Current Directory Check (`phase-1-03-pwd-ls.jpg`)
- **Shows:** `pwd` returning `/home/ec2-user`, followed by `ls`.
- **Proves:** I checked my location before running other commands.

### 4. Logged In to Amazon Linux 2023 (`phase-1-04-ssh-login-pwd.jpg`)
- **Shows:** The Amazon Linux 2023 login banner and the prompt `ec2-user@ip-172-31-29-45`.
- **Proves:** I connected to the EC2 instance over SSH.

### 5. Update Notice After Login (`phase-1-05-login-banner.jpg`)
- **Shows:** The login banner with the notice that a newer Amazon Linux release is available.
- **Proves:** The SSH session connected to a real, running server.

### 6. SSH Connection From Windows PowerShell (`phase-1-06-powershell-ssh.jpg`)
- **Shows:** `icacls` restricting the key file permissions, followed by `ssh -i <keyfile> ec2-user@<public-dns>`.
- **Proves:** I set key file permissions before connecting, and I connected from PowerShell.

### 7. EC2 Instance Summary (`phase-1-07-ec2-instance-summary.jpg`)
- **Shows:** The instance summary page: state "Running", instance type t3.micro, and a private DNS name.
- **Proves:** The instance was running and I could read its details in the console.

### 8. Brute-Force Investigation (`phase-1-08-journalctl-admin-attacks.jpg`)
- **Shows:** The output of `journalctl | grep -i "admin" | head -20`. It lists failed login attempts for `admin`, `mailadmin`, `ansadmin`, `drcomadmin`, and `ftpadmin` from several public IP addresses.
- **Proves:** I used `journalctl`, `grep`, and a pipe to find real login attempts against my server.

### 9. Hostname and OS Release (`phase-1-09-hostname-os-release.jpg`)
- **Shows:** `cat /etc/hostname` and `cat /etc/os-release`, which identifies the system as Amazon Linux 2023.
- **Proves:** I can identify the system I'm working on.

### 10. Navigation Practice (`phase-1-10-cd-pwd-navigation.jpg`)
- **Shows:** `cd /var`, `pwd`, `cd ~`, `pwd`, and `cat /etc/hostname`.
- **Proves:** I can move between absolute and home paths and confirm where I am.

### 11. Navigation Practice, Continued (`phase-1-11-cd-navigation-2.jpg`)
- **Shows:** The same `cd /var` and `cd ~` sequence, with the file listing above it.
- **Proves:** I repeated the navigation commands until they were comfortable.

### 12. Directory Listing and Navigation (`phase-1-12-ls-la-cd-var.jpg`)
- **Shows:** `ls -la` of the home directory, then `cd /var` and `pwd`.
- **Proves:** I can read a full directory listing and move into system directories.

## Instance Note

The screenshots in this folder come from the EC2 instance labelled "third instance" in the console (Amazon Linux 2023). Some earlier Phase 1 notes refer to a different instance and to Amazon Linux 2. The screenshots are the accurate record of the labs.There might be a slight difference in my phase 1 notes and the screenshots of the labs that would be presented as evidence here.This is because it did not occur to me to take screenshot of them at the time, so this screenshots are now taken to show clarification and the fact that i can actually do what is i wrote in my documentation. 

**Status:** Phase 1 evidence complete
