# Linux Commands Mastery - Phase 1

All 10 essential Linux commands learned with deep understanding using Feynman technique.

---

## Command #1: pwd (Print Working Directory)

### What It Does
Shows the full path of the folder you're currently in.

### Syntax
```bash
pwd
```

### Real Example Output### Why You Need It
Before doing ANYTHING with files, you must know where you are. Prevents accidental deletion or modification of wrong files.

### When To Use It
- First thing when you SSH into a server
- After moving between folders (verify location)
- When you're confused about where you are

### Security Reason
Knowing your exact location prevents catastrophic mistakes. "Delete this file" is safe in /tmp/ but DANGEROUS in /etc/

### My Learning Journey
**Week 1:** First command learned. Simple but critical.  
**Usage:** Used constantly. Habit now!  
**Confidence:** 9/10 

---

## Command #2: ls (List)

### What It Does
Shows all files and folders in the current directory.

### Syntax
```bash
ls              # Basic listing
ls -l           # Long format with details
ls -la          # Shows hidden files too
ls -lh          # Human-readable file sizes
```

### Real Example Output
```bash
# Basic ls
Desktop  Documents  Downloads  test.txt

# ls -l (shows details)
drwxr-xr-x  3 ec2-user ec2-user  4096 May 28 10:45 Desktop
-rw-r--r--  1 ec2-user ec2-user 12345 May 27 15:20 myfile.txt
```

### What Each Part Means
- `d` at start = directory (folder)
- `-` at start = regular file
- `rwxr-xr-x` = permissions (read, write, execute)
- `ec2-user` = owner of file
- `4096` = size in bytes
- `May 28 10:45` = last modified date/time

### Why You Need It
See what's on the system. Spot suspicious files. Understand directory structure. CRITICAL for security investigations.

### When To Use It
- Check what's in a folder
- Verify file sizes
- See file permissions
- Spot unexpected files (security audit!)
- Track file modifications

### Security Reason
Hackers might hide files. `ls -la` shows EVERYTHING including hidden files (starting with `.`). One of your first tools in incident response.

### Real Security Use
**Found real hackers:** Used `journalctl | grep` (combining with grep) to discover 18 failed login attempts from 5 different IP addresses!

### My Learning Journey
**Week 1:** Learned after pwd. Opened my eyes to security.  
**Week 2:** Discovered real attack using this command!  
**Confidence:** 9/10 

---

## Command #3: cd (Change Directory)

### What It Does
Moves you from one folder to another. Navigation tool.

### Syntax
```bash
cd /path/to/folder      # Go to specific folder (absolute path)
cd folder-name          # Go to folder in current location (relative)
cd ~                    # Go to home directory
cd ..                   # Go up to parent folder
cd -                    # Go back to previous location
```

### Real Example Journey
```bash
ec2-user@instance:~$ pwd
/home/ec2-user

ec2-user@instance:~$ cd /var
ec2-user@instance:/var$ pwd
/var

ec2-user@instance:/var$ cd log
ec2-user@instance:/var/log$ pwd
/var/log

ec2-user@instance:/var/log$ cd ..
ec2-user@instance:/var$ pwd
/var

ec2-user@instance:/var$ cd -
ec2-user@instance:/var/log$ pwd
/var/log
(You're back where you were!)
```

### Key Differences
- `cd /var` = Go to /var from ANYWHERE
- `cd var` = Go to var folder IN YOUR CURRENT LOCATION
- `cd ..` = Go UP one level to parent
- `cd -` = Go BACK to previous location (like browser back button!)

### Why You Need It
Navigate the file system. Find configuration files. Investigate security issues. Explore logs.

### When To Use It
- Move between folders constantly
- Access system files for investigation
- Navigate to log directories
- Security incident response

### Security Reason
Wrong location = wrong file modified = disaster! Navigation precision is critical.

### My Learning Journey
**Week 1:** Learned the concept (moving between rooms).  
**Discovery:** Understood `cd ..` vs `cd -` difference (both look similar but totally different!)  
**Real use:** Navigated to /var/log to find hacker evidence.  
**Confidence:** 8/10 

---

## Command #4: echo (Print Text)

### What It Does
Writes text to terminal OR creates/modifies files with content.

### Syntax
```bash
echo "text"              # Print to screen
echo "text" > file.txt   # Create file with content
echo "text" >> file.txt  # Add to existing file (append)
```

### Real Example
```bash
ec2-user@instance:~$ echo "Hello, Gabriel!"
Hello, Gabriel!

ec2-user@instance:~$ echo "This is line 1" > myfile.txt
ec2-user@instance:~$ cat myfile.txt
This is line 1

ec2-user@instance:~$ echo "This is line 2" >> myfile.txt
ec2-user@instance:~$ cat myfile.txt
This is line 1
This is line 2
```

### Critical Symbol Differences
- `>` = CREATE or OVERWRITE file (dangerous!)
- `>>` = APPEND to file (safe, adds without deleting)

### Why You Need It
Quick file creation. Writing content without editor. Automating tasks. Creating test data.

### When To Use It
- Create simple files quickly
- Write configuration values
- Generate test data
- Automate repetitive writing tasks

### Security Warning
❌ **NEVER use `echo` for passwords!**
- Passwords visible in shell history
- Anyone can read your command history
- Use files or environment variables instead

### My Learning Journey
**Week 1:** Basic learning - seemed simple.  
**Week 2:** Realized power of `>` vs `>>` difference!  
**Experiment:** Discovered if you use `>` twice, first content is LOST!  
**Security lesson:** This is why careful commands matter.  
**Confidence:** 8/10 

---

## Command #5: cat (Concatenate/Read)

### What It Does
Shows the entire contents of a file.

### Syntax
```bash
cat filename            # Show file contents
cat -n filename         # Show with line numbers
cat -A filename         # Show hidden characters
cat file1 file2         # Show multiple files
```

### Real Example
```bash
ec2-user@instance:~$ cat myfile.txt
Line 1: Cloud security is fun
Line 2: I love learning
Line 3: No giving up!

ec2-user@instance:~$ cat -n myfile.txt
     1  Line 1: Cloud security is fun
     2  Line 2: I love learning
     3  Line 3: No giving up!
```

### Why You Need It
Read configuration files. Check log files. Understand system settings. Spot malicious changes.

### When To Use It
- Read config files (`/etc/ssh/sshd_config`)
- Check log files (`/var/log/auth.log`)
- Verify file contents
- Security investigations

### Real Security Examples
```bash
# Check SSH configuration
cat /etc/ssh/sshd_config

# Check user accounts
cat /etc/passwd

# Check system information
cat /etc/os-release

# Check failed logins
cat /var/log/auth.log | grep Failed
```

### Security Reason
Hackers modify configuration files. `cat` lets you spot changes and misconfigurations.

### My Learning Journey
**Week 1:** Learned to read files - foundation skill.  
**Week 2:** Combined with grep to find real hacker activity!  
**Discovery:** Used `cat` on `.bashrc` - saw system configuration files.  
**Real use:** Essential for every investigation.  
**Confidence:** 9/10 

---

## Command #6: touch (Create Empty Files)

### What It Does
Creates an empty file OR updates a file's timestamp.

### Syntax
```bash
touch filename                  # Create empty file
touch file1 file2 file3        # Create multiple files
touch -t 202305011000 file.txt # Set specific timestamp
```

### Real Example
```bash
ec2-user@instance:~$ touch myfile.txt
ec2-user@instance:~$ ls -l myfile.txt
-rw-r--r-- 1 ec2-user ec2-user 0 Jun 6 13:20 myfile.txt
(Size is 0 - it's empty!)

ec2-user@instance:~$ touch file1.txt file2.txt file3.txt
ec2-user@instance:~$ ls
file1.txt  file2.txt  file3.txt  myfile.txt
```

### Why You Need It
Create placeholder files. Set up file structure. Create files you'll edit later.

### When To Use It
- Create empty configuration files
- Create script files before editing
- Create log files before applications use them
- Prepare file structure

### Security Concern 
**Hackers can change timestamps to hide!**

```bash
# Hacker modifies file at 2:30 PM
# Then changes timestamp to old date:
touch -t 202305011000 /etc/passwd
# Now it LOOKS like it hasn't been modified!
```

**Defense:**
- Use immutable logs (CloudTrail)
- Check file hashes (fingerprints)
- Monitor files in REAL-TIME
- Don't trust timestamps alone!

### My Learning Journey
**Week 2:** Learned touch command.  
**Discovery:** Understood timestamp manipulation vulnerability!  
**Security insight:** This is why CloudTrail is critical (can't fake those logs).  
**Confidence:** 8/10 

---

## Command #7: grep (Search/Filter)

### What It Does
Searches through file contents and shows only matching lines.

### Syntax
```bash
grep "word" filename            # Find lines with word
grep -i "word" filename         # Case-insensitive search
grep -n "word" filename         # Show line numbers
grep -c "word" filename         # Count matches
grep -v "word" filename         # Show lines WITHOUT word
```

### Real Example
```bash
ec2-user@instance:~$ cat auth.log
Failed password for user john from 192.168.1.100
Successful login for admin from 10.0.0.1
Failed password for user mary from 192.168.1.100
User alice logged in successfully

ec2-user@instance:~$ grep "Failed" auth.log
Failed password for user john from 192.168.1.100
Failed password for user mary from 192.168.1.100

ec2-user@instance:~$ grep -c "Failed" auth.log
2
(There are 2 failed attempts!)
```

### Why You Need It
Search massive log files instantly. Find security incidents. Spot patterns. Analyze system activity.

### When To Use It
- Search logs for errors
- Find failed login attempts
- Look for specific users
- Spot suspicious activity
- Analyze large files

### Real Security Use
**Finding real hackers:**
```bash
journalctl | grep -i "admin"
# Found 18 failed admin login attempts!
# Identified 5 attacking IP addresses!
```

### Security Reason
Large files are overwhelming. `grep` finds needles in haystacks. Essential for incident response.

### My Learning Journey
**Week 2:** Learned grep.  
**MAJOR DISCOVERY:** Used grep to find REAL brute-force attack!  
  - 18 failed admin attempts
  - 5 different attacking IPs
  - Real security incident!
**Confidence:** 9/10 

---

## Command #8: nano (Text Editor)

### What It Does
Opens an interactive text editor in the terminal. Create and edit files easily.

### Syntax
```bash
nano filename           # Open/create file
```

### How to Use It
```bash
# Open nano
nano myfile.txt

# Inside nano:
- Type your text
- Ctrl+O = Save (WriteOut)
- Ctrl+X = Exit
- Ctrl+W = Search
- Ctrl+K = Delete line
```

### Real Example
```bash
ec2-user@instance:~$ nano myconfig.conf

(nano opens - you type your content)

# My config file
server=localhost
port=22
encryption=AES256

(Press Ctrl+O to save)
(Press Ctrl+X to exit)
```

### Why You Need It
Edit configuration files. Create scripts. Modify system settings. Better than `echo` for multi-line content.

### When To Use It
- Edit SSH configuration
- Create shell scripts
- Modify application config
- Write documentation
- Any time you need interactive editing

### vs `echo`
- `echo` = Quick single line
- `nano` = Multi-line, interactive editing
- `nano` = Professional approach

### My Learning Journey
**Week 2:** Learned nano.  
**Realization:** So much easier than echo for multiple lines!  
**Real use:** Perfect for config file editing.  
**Confidence:** 8/10 

---

## Command #9: chmod (Change Permissions)

### What It Does
Controls who can read, write, and execute files.

### Syntax
```bash
chmod 755 filename      # Owner: rwx, Others: r-x
chmod 644 filename      # Owner: rw-, Others: r--
chmod 600 filename      # Owner: rw-, Others: none
chmod 700 filename      # Owner: rwx, Others: none
```

### Permission Number Meanings

7 = 4+2+1 = rwx (read + write + execute)

6 = 4+2   = rw- (read + write)

5 = 4+1   = r-x (read + execute)

4 = 4     = r-- (read only)

0 = ---   (no permissions)

### Real Example
```bash
# SSH key must be private (owner only)
ec2-user@instance:~$ chmod 600 mykey.pem
ec2-user@instance:~$ ls -l mykey.pem
-rw------- ec2-user ec2-user mykey.pem
(Only owner can read!)

# Script should be executable
ec2-user@instance:~$ chmod 755 myscript.sh
ec2-user@instance:~$ ls -l myscript.sh
-rwxr-xr-x ec2-user ec2-user myscript.sh
(Everyone can read/execute, owner can write)
```

### Why You Need It
Control access to sensitive files. Prevent accidental modification. Security enforcement.

### When To Use It
- SSH keys must be 600 (owner only!)
- Config files often 644 (owner edit, others read)
- Scripts often 755 (everyone can run)
- Backups often 600 (secure)

### Security Critical
 **NEVER use 777** (everyone can do anything!)

```bash
# DON'T DO THIS:
chmod 777 anything
# This means: rwxrwxrwx (everyone can read, write, execute!)
# SECURITY DISASTER!
```

 **ALWAYS use minimal:**
- SSH keys: 600
- Config files: 644
- Scripts: 755
- Sensitive files: 600

### My Learning Journey
**Week 2:** Learned chmod.  
**Discovery:** Understood rwx system deeply!  
**Security insight:** File permissions prevent attacks!  
**Real use:** Essential for security hardening.  
**Confidence:** 8/10 

---

## Command #10: sudo (Super User Do)

### What It Does
Runs a command with administrator (root) privileges.

### Syntax
```bash
sudo command
sudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### When You Use It
```bash
# Edit system file (needs admin)
sudo nano /etc/ssh/sshd_config

# Install software (needs admin)
sudo apt-get install nmap

# View restricted files (needs admin)
sudo cat /var/log/auth.log

# Change ownership (needs admin)
sudo chown root:root myfile.txt
```

### Why You Need It
Edit system files. Install software. Manage services. Change critical settings.

### When To Use It
- Edit system configuration
- Install packages
- Restart services
- Manage users/groups
- View restricted logs

###  DANGER 
**`sudo` is POWERFUL and DANGEROUS!**

```bash
# This deletes your ENTIRE system:
sudo rm -rf /
# With sudo, it ACTUALLY WORKS!
# DISASTER!
```

**Rule:** Think 3 times before using sudo with rm, mkfs, or deletion!

### Security Considerations
- Only use sudo when necessary
- Think before you type
- `sudo` can break your system
- Never give sudo to untrusted people
- Monitor who uses sudo (CloudTrail logs it!)

### My Learning Journey
**Week 2:** Learned sudo.  
**Realization:** Power comes with responsibility!  
**Security lesson:** One wrong sudo command = system down.  
**Confidence:** 8/10 ✅

---

## Bonus: The PIPE Operator `|`

### What It Does
Takes output from one command and feeds it to another.

### Syntax
```bash
command1 | command2
```

### Real Example
```bash
# Without pipe:
journalctl
(Shows 10,000 lines - overwhelming!)

# With pipe:
journalctl | grep -i "failed"
(Shows only relevant lines!)
```

### Why It's Powerful
- Combine commands
- Filter large outputs
- Process data step-by-step

### Real Security Use
```bash
# Find all failed logins from specific IP:
cat /var/log/auth.log | grep "Failed" | grep "192.168.1.100"

# Count errors:
journalctl | grep "error" | wc -l

# Show last 10 errors:
journalctl | grep "error" | tail -10
```

### My Learning Journey
**Week 2:** Discovered pipe power!  
**Real use:** Used to find 18 failed login attempts!  
**Confidence:** 8/10 

---

## Summary: Commands Learned

| Command | Purpose | Confidence |
|---------|---------|-----------|
| pwd | Know location | 9/10 |
| ls | See files | 9/10 |
| cd | Navigate | 8/10 |
| echo | Write text | 8/10 |
| cat | Read files | 9/10 |
| touch | Create files | 8/10 |
| grep | Search | 9/10 |
| nano | Edit | 8/10 |
| chmod | Permissions | 8/10 |
| sudo | Admin access | 8/10 |
| \| (pipe) | Combine commands | 8/10 |

**Overall:** 8/10 confidence with all commands! 

---

## Key Insights

1. **Commands work together** - ls, cd, cat all complement each other
2. **Security is built-in** - chmod, sudo, grep all have security implications
3. **Thinking like hacker** - Using these commands to find real attacks
4. **Real experience** - Used these commands to discover actual brute-force!
5. **Foundation for everything** - These 10 commands unlock the entire Linux system

---

## Next Phase

These commands are the foundation for:
- Phase 2: IAM management
- Phase 3: Network security
- Phases 4-7: Advanced security
- Real job: Daily tools!

**Status:** Phase 1 Commands MASTERED 

