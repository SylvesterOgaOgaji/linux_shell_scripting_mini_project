# Introduction to Linux Shell Scripting and File Permissions

## Navigating the Linux Filesystem

This guide documents a practical journey through essential Linux system administration tasks, focusing on shell scripting and permission management. We begin with fundamental directory operations and progress to advanced script execution scenarios.

### Key Learning Objectives:
1. **Directory Navigation**: Mastering `pwd` and `cd` commands
2. **File System Inspection**: Using `ls` with various flags
3. **Shell Environment**: Examining `/etc/shells` configurations
4. **Script Development**: Creating and executing shell scripts
5. **Permission Management**: Understanding and modifying file access rights.

## Permission Architecture Explained

The examples clearly illustrate Linux's security model:
- **Owner/Group/Others** triad
- **Read-Write-Execute** privilege system
- Numeric (755) vs symbolic (u+x) permission notation

This foundation prepares users for advanced topics like:
- Automated user provisioning
- Group collaboration setups
- Secure script deployment

*Note: All commands were executed on Ubuntu 24.04 LTS. Paths and outputs may vary slightly across distributions.*

```markdown
# Shell Scripting Essentials: From Creation to Execution

## 1. Establishing the Workspace
![Current Directory](./img/1.%20PWD.jpg)

**Command:**  
```bash
pwd
```

**Output:**  
`/home/ubuntu`  

**Explanation:**  
Confirms the current working directory before creating new project folders.

---

## 2. Shell Environment Verification
![Shell Variants](./img/2.%20Confirming%20the.jpg)

**Command:**  
```bash
cat /etc/shells
```

**Output:**  
```
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
...
```

**Key Insight:**  
- Lists all available shells on the system  
- Bash (`/bin/bash`) is the default for Ubuntu  

---

## 3. Project Directory Creation
![Directory Setup](./img/3.%20Directory%20creted%20shell-scripting.jpg)

**Command:**  
```bash
mkdir shell-scripting
ls -lstr
```

**Output:**  
```
drwxrwxr-x 2 ubuntu ubuntu 4096 May 22 shell-scripting
```

**Permissions Breakdown:**  
- `drwxrwxr-x`:  
  - `d`: Directory  
  - `rwx`: Owner (ubuntu) has full access  
  - `rwx`: Group members can read/write/execute  
  - `r-x`: Others can only read/execute  

---

## 4. Script Development Workflow
![Script Creation](./img/4.%20Navigated%20to%20shell-scripting.jpg)

**Process:**  
1. Navigate to directory:  
   ```bash
   cd shell-scripting
   ```
2. Create script:  
   ```bash
   vim my_first_shell_script.sh
   ```

**Default Permissions:**  
![Initial Permissions](./img/7.%20File%20created%20with%20initail%20access%20permission.jpg)
```
-rw-rw-r-- 1 ubuntu ubuntu 153 May 22 my_first_shell_script.sh
```

**Permission Analysis:**  
| User Class | Read | Write | Execute |
|------------|------|-------|---------|
| Owner      | ✓    | ✓     | ✗       |
| Group      | ✓    | ✓     | ✗       |
| Others     | ✓    | ✗     | ✗       |

---

## 5. Execution Permission Workflow
![Permission Denied](./img/8.%20Execute%20Permission%20is%20denied,%20no%20execution.jpg)

**Attempt to Run:**  
```bash
./my_first_shell_script.sh
```

**Error:**  
`-bash: ./my_first_shell_script.sh: Permission denied`

**Solution:**  
```bash
chmod +x my_first_shell_script.sh
```

**Updated Permissions:**  
```
-rwxrwxr-x 1 ubuntu ubuntu 153 May 22 my_first_shell_script.sh
```

---

## 6. Bulk Operations Script
![Automation Script](./img/5.%20Command%20to%20create%20directory%20or%20folders.jpg)

**Script Content:**  
```bash
#!/bin/bash
mkdir Folder{1..3}
sudo useradd user{1..3}
```

**Key Features:**  
- Creates 3 folders: `Folder1`, `Folder2`, `Folder3`  
- Adds 3 users: `user1`, `user2`, `user3`  
- Uses brace expansion for efficiency  

---

## Security Best Practices

1. **Execution Permissions**  
   - Use `chmod +x` judiciously  
   - Never grant `777` to scripts handling sensitive data  

2. **User Management**  
   ```bash
   sudo passwd user1  # Always set passwords for new users
   ```

3. **Directory Permissions**  
   ```bash
   chmod 750 ~/shell-scripting  # Restrict access to owner+group
   ```

---

## Troubleshooting Checklist

✅ **Permission Denied Error**  
   - Verify with `ls -l`  
   - Fix with `chmod +x`  

✅ **Script Not Found**  
   - Confirm path with `pwd`  
   - Use full path: `/home/ubuntu/shell-scripting/script.sh`  

✅ **Editor Issues**  
   - Use `nano` if `vim` isn't available:  
     ```bash
     sudo apt install nano
     ```

---

# Advanced Shell Scripting Operations

## 9. Granting Execution Privileges
![Elevate Permissions](./img/9.%20Command%20to%20elevate%20execution%20permission.jpg)

**Command Correction:**  
```bash
chmod u+x my_first_shell_script.sh  # Fix typo: [cmod → chmod]
```

**Before:**  
`-rw-rw-r--`  
**After:**  
`-rwxrw-r--`  

**Key Changes:**  
- Owner now has execute (`x`) permission  
- File color turns green in terminal (executable indicator)  

---

## 10. Automated Directory Creation
![Folder Creation](./img/10.%20Folders%20Created%20succesfully.jpg)

**Script Output Verification:**  
```bash
./my_first_shell_script.sh
ls
```
**Result:**  
```
Folder1  Folder2  Folder3  my_first_shell_script.sh
```

**Permission Analysis:**  
```bash
ls -ld Folder*
```
```
drwxrwxr-x 2 ubuntu ubuntu 4096 May 22 Folder1
```

---

## 11. Bulk User Provisioning
![User Creation](./img/11.%20Three%20Users%20created.jpg)

**Verification Commands:**  
```bash
id user1
id user2 
id user3
```

**Sample Output:**  
```
uid=1006(user1) gid=1009(user1) groups=1009(user1)
```

**Security Note:**  
Always set passwords for new users:  
```bash
sudo passwd user1
```

---

## 12. System Binary Exploration
![Bin Directory](./img/12.%20Exploring%20the%20slash%20bin.jpg)

**Command:**  
```bash
ls -l /bin
```

**Key Findings:**  
- Symbolic link: `/bin → usr/bin`  
- Essential utilities: `apt`, `bash`, `chmod`  
- System management tools: `adduser`, `systemctl`  

**Permission Pattern:**  
Most binaries have `-rwxr-xr-x` (755) permissions  

---

## 13. Navigation Techniques
![Directory Navigation](./img/13.%20Navigating%20back%20to%20shell-scripting%20folder.jpg)

**Effective Commands:**  
```bash
cd ~           # Return to home
cd -           # Switch to previous directory
pushd/popd     # Stack-based navigation
```

**Path Verification:**  
```bash
pwd  # Output: /home/ubuntu/shell-scripting
```

---

## 14. Script Development Workflow
![Script Editing](./img/14.%20Entered%20the%20script.jpg)

**Sample Script Content:**  
```bash
#!/bin/bash
name="John"
echo "Example: Variable assignment"
echo "My name is $name"
```

**Best Practice:**  
Always include shebang line:  
`#!/usr/bin/env bash`

---

## 15. Script Execution Demo
![Script Output](./img/15.%20Executed%20from%20the%20script.jpg)

**Execution Command:**  
```bash
./my_first_shell_script.sh
```

**Output:**  
```
Example: Variable assignment  
My name is John
```

---

## 16. CLI vs Script Execution
![CLI Execution](./img/16.%20Executed%20from%20CLI.jpg)

**Direct Variable Assignment:**  
```bash
name="Sarah" && echo $name  # Output: Sarah
```

**Key Difference:**  
- Script variables: Contained within execution environment  
- CLI variables: Session-persistent  

---