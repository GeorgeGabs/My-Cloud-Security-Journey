# Linux Commands Cheatsheet - Week 1

All commands mastered by me, Gabriel. Feynman-style explanations included.

---

## Command #1: pwd (Print Working Directory)

### What It Does
Shows you which folder you're currently in.

### Syntax
```bash
pwd
```

### Output Example

/home/ec2-user 

### Why You Need It
Before doing ANYTHING with files, you MUST know where you are. Prevents accidents.

### When To Use It
- First thing every session
- After moving between folders
- When you're confused about location

### Real Example
```bash
ec2-user@instance:~$ pwd
/home/ec2-user

ec2-user@instance:~$ cd /var
ec2-user@instance:/var$ pwd
/var
```

### Security Reason
In cloud security, knowing your exact location prevents accidental deletion of critical files.

---

## Command #2: ls (List)

### What It Does
Shows all files and folders in your current location.

### Syntax
```bash
ls              # Basic list
ls -l           # Long format with details
ls -la          # All files (including hidden) with details
ls -n myfile    # With line numbers
```

### Output Example
```bash
# Basic ls
Desktop  Documents  Downloads

# ls -l (long format)
drwxr-xr-x  3 ec2-user ec2-user  4096 May 28 10:45 Desktop
-rw-r--r--  1 ec2-user ec2-user 12345 May 27 15:20 myfile.txt
```

### What It Means
- `d` = directory (folder)
- `-` = file
- `rwxr-xr-x` = permissions
- `ec2-user` = owner
- `4096` = size in bytes
- `May 28 10:45` = date/time modified

### Why You Need It
See what's on the system. Spot suspicious files. Understand structure.

### When To Use It
- See what's in a folder
- Check file sizes
- Verify files exist
- Security audits

### Security Reason
Hackers might hide files. `ls -la` reveals EVERYTHING including hidden files (starting with `.`).

---

## Command #3: cd (Change Directory)

### What It Does
Moves you from one folder to another.

### Syntax
```bash
cd /path/to/folder      # Go to specific folder (absolute path)
cd folder-name          # Go to folder in current location (relative)
cd ~                    # Go home
cd ..                   # Go up to parent folder
cd -                    # Go back to previous location
```

### Output Example
```bash
ec2-user@instance:~$ cd /var
ec2-user@instance:/var$ pwd
/var

ec2-user@instance:/var$ cd ..
ec2-user@instance:/$  pwd
/

ec2-user@instance:/$ cd ~
ec2-user@instance:~$ pwd
/home/ec2-user
```

### Key Differences
- `cd /var` = Go to `/var` from ANYWHERE
- `cd var` = Go to `var` folder in CURRENT location
- `cd ..` = Go UP one level (parent)
- `cd -` = Go BACK to previous location

### Why You Need It
Navigate the file system. Find configuration files. Investigate security issues.

### When To Use It
- Move between folders
- Find system files
- Explore structure
- Security investigations

### Security Reason
You MUST know where files are before modifying them. Wrong location = huge mistake.

---

## Command #4: echo (Print Text)

### What It Does
Writes text to the terminal OR to a file.

### Syntax
```bash
echo "text"              # Print to terminal
echo "text" > file.txt   # Create file with content
echo "text" >> file.txt  # Add to existing file
```

### Output Example
```bash
ec2-user@instance:~$ echo "Hello, Gabriel!"
Hello, Gabriel!

ec2-user@instance:~$ echo "Line 1" > myfile.txt
ec2-user@instance:~$ echo "Line 2" >> myfile.txt
ec2-user@instance:~$ cat myfile.txt
Line 1
Line 2
```

### What the Symbols Mean
- `>` = Create/overwrite file
- `>>` = Append to file (add without deleting)

### Why You Need It
Create files quickly. Add content. Write scripts. Configure systems.

### When To Use It
- Create config files
- Write to log files
- Create test data
- Automate tasks

### Security Reason
Quick way to create files without manual editing. But NEVER use `echo` for passwords (they'd be visible in shell history!).

---

## Command #5: cat (Concatenate/Read Files)

### What It Does
Shows the entire contents of a file.

### Syntax
```bash
cat filename            # Show file contents
cat -n filename         # Show with line numbers
cat -A filename         # Show hidden characters
cat file1 file2         # Show multiple files
```

### Output Example
```bash
ec2-user@instance:~$ cat myfile.txt
Line 1: Cloud security is fun
Line 2: I love learning
Line 3: No giving up!

ec2-user@instance:~$ cat -n myfile.txt
     1  Line 1: Cloud security is fun
     2  I love learning
     3  No giving up!
```

### Why You Need It
Read configuration files. Check log files. Understand system settings. Spot malicious changes.

### When To Use It
- Read config files (`/etc/ssh/sshd_config`)
- Check log files (`/var/log/auth.log`)
- Verify file contents
- Security investigations

### Real Examples (Cloud Security)
```bash
# Check SSH config for security issues
cat /etc/ssh/sshd_config

# Check user accounts
cat /etc/passwd

# Read system info
cat /etc/os-release

# Check for malicious activity
cat /var/log/auth.log
```

### Security Reason
Hackers might modify config files. You use `cat` to spot changes and malicious code.

---

## Command #6: touch (Create/Update Files)

### What It Does
Creates an empty file OR updates a file's timestamp.

### Syntax
```bash
touch filename           # Create empty file
touch file1 file2 file3  # Create multiple files
touch -t 202305011000 file.txt  # Set specific timestamp
```

### Output Example
```bash
ec2-user@instance:~$ touch myfile.txt
ec2-user@instance:~$ ls -l myfile.txt
-rw-r--r-- 1 ec2-user ec2-user 0 Jun 6 13:20 myfile.txt

ec2-user@instance:~$ touch file1.txt file2.txt file3.txt
ec2-user@instance:~$ ls
file1.txt  file2.txt  file3.txt  myfile.txt
```

### Why You Need It
Create placeholder files. Set up file structure. Update access times for tracking.

### When To Use It
- Create config files (then edit later)
- Create script files (then add code)
- Create log files
- Update timestamps

### Security Concern ⚠️
**HACKERS CAN CHANGE TIMESTAMPS!**

```bash
# Hacker modifies a file, then fakes the timestamp
touch -t 202305011000 /etc/passwd
# Now it LOOKS like it hasn't been modified!
```

**Defense:**
- Use immutable logs (CloudTrail)
- Check file hashes (fingerprints)
- Monitor files in REAL TIME
- Never trust timestamps alone

---

## Summary Table

| Command | Creates | Reads | Moves | Lists |
|---------|---------|-------|-------|-------|
| pwd | ❌ | ❌ | ❌ | ❌ |
| ls | ❌ | ❌ | ❌ | ✅ |
| cd | ❌ | ❌ | ✅ | ❌ |
| echo | ✅ | ❌ | ❌ | ❌ |
| cat | ❌ | ✅ | ❌ | ❌ |
| touch | ✅ | ❌ | ❌ | ❌ |

---

## Week 1 Mastery Checklist

- [x] pwd - Understand location
- [x] ls - See file system
- [x] cd - Navigate
- [x] echo - Write text
- [x] cat - Read files
- [x] touch - Create files

**All 6 commands mastered!** 🏆

---

**Last Updated:** June 6, 2025

**Week 1 COMPLETE! 🎉
