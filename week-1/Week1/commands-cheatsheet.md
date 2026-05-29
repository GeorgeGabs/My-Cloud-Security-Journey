# Linux Commands Cheatsheet - Week 1

## Command #1: pwd (Print Working Directory)

### What It Does
Shows you which folder you're currently in.

### Syntax 
pwd

### Output Example
/home/ec2-user

### Why You Need It
Before doing ANYTHING with files (delete, create, edit), you MUST know where you are.

### When To Use It
- First thing every session
- After moving between folders
- When you're confused about location

### Real Example
ec2-user@instance:~$ pwd
/home/ec2-user
ec2-user@instance:~$ cd /
ec2-user@instance:/$ pwd
/
ec2-user@instance:/$ cd ~
ec2-user@instance:~$ pwd
/home/ec2-user

### Security Reason
In cloud security, you need to know EXACTLY where files are before deleting them. A hacker might put malicious files in /var/www/html. If YOU know where you are, you can spot them.

---

## Coming Soon
- ls (list files)
- cd (change directory)
- cat (read files)
