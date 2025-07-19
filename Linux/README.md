# Linux System Administration & Automation

# User & Group Management:

### Learn about Linux users, groups, and permissions (/etc/passwd, /etc/group).

Create a user devops_user and add them to a group devops_team.

Here's a **step-by-step guide** to create a user `devops_user` and add them to a group `devops_team` on a **Linux system** (works on most distros like Ubuntu, CentOS, Debian, etc.):

---

### ✅ Step 1: Open Terminal

Make sure you have **sudo/root access**.

---

### ✅ Step 2: Create the Group `devops_team`

```bash
sudo groupadd devops_team
```

> 🔹 This command creates a new group named `devops_team`.

---

### ✅ Step 3: Create the User `devops_user` and Add to the Group

```bash
sudo useradd -m -G devops_team devops_user
```

> 🔹 Explanation:
>
> * `-m`: Creates a home directory (`/home/devops_user`)
> * `-G devops_team`: Adds the user to the group `devops_team`

---

### ✅ Step 4: Set a Password for the User

```bash
sudo passwd devops_user
```

> 🔹 You'll be prompted to enter and confirm the password for `devops_user`.

---

### ✅ Step 5: Verify the User and Group Membership

```bash
id devops_user
```

> Output should show something like:
>
> ```bash
> uid=1001(devops_user) gid=1001(devops_user) groups=1001(devops_user),1002(devops_team)
> ```

---

### ✅ Optional: Switch to the New User

```bash
su - devops_user
```

### Set a password and grant sudo acces:

Here’s a **step-by-step guide** to set a password for a user and **grant them `sudo` access** on a **Linux system**:

---

## ✅ Part 1: Set a Password for the User

### 🔹 Step 1: Set/Change Password

```bash
sudo passwd devops_user
```

> You'll be prompted to enter a new password for `devops_user`.

---

## ✅ Part 2: Grant `sudo` Access

You can grant `sudo` access in **two main ways**. We'll use the **safest and most common one** — adding the user to the `sudo` group.

### 🔹 Step 2: Add User to `sudo` Group

```bash
sudo usermod -aG sudo devops_user
```

> ✅ `-aG`: Appends the user to the group without removing existing ones
> ✅ `sudo`: This is the admin group on **Ubuntu/Debian-based** systems
> 🔁 On **CentOS/RHEL**, the group is usually `wheel`, so you’d use:

```bash
sudo usermod -aG wheel devops_user
```

---

## ✅ Part 3: Verify `sudo` Access

### 🔹 Step 3: Switch to the User

```bash
su - devops_user
```

### 🔹 Step 4: Try a `sudo` Command

```bash
sudo whoami
```

> It should prompt for the password and return `root` if successful.

---

## ✅ Summary

| Task                            | Command                              |
| ------------------------------- | ------------------------------------ |
| Set password                    | `sudo passwd devops_user`            |
| Add to sudo group (Ubuntu)      | `sudo usermod -aG sudo devops_user`  |
| Add to sudo group (CentOS/RHEL) | `sudo usermod -aG wheel devops_user` |
| Switch user                     | `su - devops_user`                   |
| Test sudo                       | `sudo whoami`                        |

---

### Restrict SSH login for certain users in /etc/ssh/sshd_config

To **restrict SSH login for certain users**, you can edit the `/etc/ssh/sshd_config` file on your Linux server. Here is a step-by-step guide:

---

### 🔒 **Step-by-Step: Restrict SSH Access to Specific Users**

#### **1. Open the SSH config file**

```bash
sudo nano /etc/ssh/sshd_config
```

#### **2. Use one of the following directives:**

✅ **To allow only specific users:**

```bash
AllowUsers devops_user user2
```

> Only `devops_user` and `user2` can access SSH. Everyone else is denied.

❌ **To deny specific users (allow all others):**

```bash
DenyUsers baduser testuser
```

> `baduser` and `testuser` cannot SSH into the system.

> ⚠️ **Do not use both `AllowUsers` and `DenyUsers` together** unless you understand precedence — `AllowUsers` takes priority.

---

### 🖊️ Optional: Restrict by Group

To restrict login based on group membership:

✅ **Allow only users in a specific group:**

```bash
AllowGroups sshusers
```

❌ **Deny users in a specific group:**

```bash
DenyGroups blockedgroup
```

---

### 🔄 **3. Save and Exit**

In `nano`, press:

```
Ctrl + O → Enter → Ctrl + X
```

---

### 🔁 **4. Restart SSH Service**

```bash
sudo systemctl restart sshd
```

---

### ✅ **Verify**

Try logging in with a restricted user from another terminal:

```bash
ssh baduser@your_server_ip
```

It should deny access with a message like:

```
Permission denied, please try again.
```
======================================================================================
### If you get error: Permission denied (publickey,gssapi-keyex,gssapi-with-mic)

mkdir -p /home/zaki/.ssh
cp /tmp/zaki-key.pub /home/zaki/.ssh/authorized_keys
chown -R zaki:zaki /home/zaki/.ssh
chmod 700 /home/zaki/.ssh
chmod 600 /home/zaki/.ssh/authorized_keys
sudo vi /etc/ssh/sshd_config

AllowUsers user1
:wq (save and exit)

AllowGroups sshusers

DenyGroups blockedgroup

sudo systemctl restart sshd

### ✅ **Verify**

ssh baduser@your_server_ip
    
### Here is a **well-structured documentation** in Markdown format that documents how to **create a user and restrict SSH access to specific users** on an Amazon Linux 2023 EC2 instance.

# 🔒 Restrict SSH Access to Specific Users on Amazon Linux (EC2)

This guide documents the procedure to:
- Create a new user (`user1`)
- Configure SSH key-based access
- Grant sudo privileges
- Restrict SSH access to only specific users
---
## 🧱 Prerequisites

- EC2 instance running **Amazon Linux 2023**
- SSH access to the instance as `ec2-user` or a user with `sudo` privileges
- A public SSH key already available (e.g., from `~/.ssh/id_rsa.pub`)
---

## 👤 Step 1: Create a New User

```bash
sudo useradd user1
````
---
## 📁 Step 2: Set Up `.ssh` Directory for the New User

```bash
sudo mkdir -p /home/user1/.ssh
sudo cp /home/ec2-user/.ssh/authorized_keys /home/user1/.ssh/
sudo chown -R user1:user1 /home/user1/.ssh
sudo chmod 700 /home/user1/.ssh
sudo chmod 600 /home/user1/.ssh/authorized_keys
```
---
## 🔑 Step 3: Grant Sudo Privileges

```bash
sudo usermod -aG wheel user1
```
---
## 🔐 Step 4: (Optional) Set a Password for `user1`

```bash
sudo passwd user1
```
> ⚠️ Avoid including the username in the password. Use a strong and secure password.

---
## 🚫 Step 5: Restrict SSH Access to Specific Users

Open the SSH daemon configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```
Add the following line at the end of the file to allow only specific users to SSH:

```conf
AllowUsers ec2-user user1
```

> Replace `ec2-user` and `user1` with the usernames you want to allow.
> This **blocks SSH access for all other users**, including `root`.
---
## 🔁 Step 6: Restart SSH Service

```bash
sudo systemctl restart sshd
```
---
## 🧪 Step 7: Verify SSH Login with New User

Try logging in with `user1` from your local terminal:

```bash
ssh user1@<your-ec2-public-ip>
```

If successful, you'll see:

```text
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
```
---
## ✅ Summary of What We Did

| Task                                   | Status |
| -------------------------------------- | ------ |
| Created `user1`                        | ✅      |
| Set up SSH key-based login             | ✅      |
| Configured correct file permissions    | ✅      |
| Granted sudo access                    | ✅      |
| Restricted SSH access via `AllowUsers` | ✅      |
| Tested successful login                | ✅      |

---
## 📄 SSH Public Key Reference
Example key used (from `~/.ssh/id_rsa.pub`):

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCnGVM62v+KlUOaHic1DT7di4WnXEvp4WrbJz8soWjJrobo4tL8GVP...
```

> You can use your own key or generate one using:
>
> ```bash
> ssh-keygen -t rsa -b 4096
> ```

---

## 🔐 Tip: Remove AWS Default `authorized_keys` Restriction (Optional)

If `/home/ec2-user/.ssh/authorized_keys` contains a line like this:

```text
no-port-forwarding,no-agent-forwarding,... command="echo 'Please login as ec2-user'..."
```

You can remove it for full shell access. Edit `~/.ssh/authorized_keys` and remove or replace the restricted line with your actual public key.

---

## 📦 Files Modified

* `/etc/ssh/sshd_config`
* `/home/user1/.ssh/authorized_keys`

---

**Author:** `YourName`
**Date:** July 18, 2025
**Platform:** Amazon Linux 2023 (EC2)
```

# File & Directory Permissions:

### Create /devops_workspace and a file project_notes.txt.

# Step 1: Create the directory
sudo mkdir /devops_workspace

# Step 2: Create the file inside the directory
sudo touch /devops_workspace/project_notes.txt

# Step 3: Set ownership to current user (optional, replace 'your_username' if needed)
sudo chown $USER:$USER /devops_workspace /devops_workspace/project_notes.txt

# Step 4: Set permissions
# Owner: read/write (rw-), Group: read-only (r--), Others: no access (---)
sudo chmod 740 /devops_workspace/project_notes.txt

# Use ls -l to verify permissions.


#  Log File Analysis with AWK, Grep & Sed:

### Logs are crucial in DevOps! You’ll analyze logs using the Linux_2k.log file from LogHub ([GitHub Repo](https://github.com/logpai/loghub/blob/master/Linux/Linux_2k.log).

### Grep: 

To find **all occurrences of the word `error`** in a file using `grep`, use the following command:

---

### ✅ **Basic Command**

```bash
grep "error" filename.txt
```

Replace `filename.txt` with your actual file name (e.g., `application.txt`).

---

### 🔍 **Case-insensitive Search**

To match `Error`, `ERROR`, `eRrOr`, etc.:

```bash
grep -i "error" filename.txt
```

---

### 📄 **Search in All Files in Current Directory**

```bash
grep -i "error" *
```

---

### 📁 **Search Recursively in All Files and Subdirectories**

```bash
grep -ri "error" .
```

---

### 📌 Example with Log File:

```bash
grep -i "error" /var/log/messages
```

### awk:

To use `awk` to extract **timestamps** and **log levels** from a log file, we first need to assume a **typical log format**. Here's a general example log line:

```
Jul 18 12:45:02 server-name application-name: [ERROR] Something failed here.
```

---

### ✅ **AWK Command to Extract Timestamp and Log Level**

```bash
awk '{print $1, $2, $3, $NF}' filename.txt
```

* `$1, $2, $3` → `Jul 18 12:45:02` (timestamp)
* `$NF` → the **last field**, which is often the log level in square brackets like `[ERROR]`

---

### 💡 Better: Extract `[LOGLEVEL]` from anywhere in the line

If the log level (e.g., `[INFO]`, `[ERROR]`, `[WARN]`) is not always the last field, use a **pattern match**:

```bash
awk '{for(i=1;i<=NF;i++) if($i ~ /^\[[A-Z]+\]$/) print $1, $2, $3, $i}' filename.txt
```

✔️ This will:

* Loop through each word in the line
* Print the **timestamp** + **log level** if the log level is in the format `[LEVEL]`

---

### 📌 Example Output:

```
Jul 18 12:45:02 [ERROR]
Jul 18 12:47:10 [INFO]
Jul 18 12:49:33 [WARN]
```
### sed:

Here's a **step-by-step guide to `sed` commands** with examples. `sed` (Stream Editor) is used in Linux/Unix to **search**, **replace**, **insert**, and **delete** lines in text files.

---

## 🧱 1. **Basic Syntax**

```bash
sed [options] 'command' filename
```

---

## 🔄 2. **Substitute/Replace (`s`)**

### 🔹 Replace first occurrence on each line:

```bash
sed 's/apple/orange/' fruits.txt
```

**Explanation:** Replaces the first **"apple"** with **"orange"** on each line.

---

### 🔹 Replace all occurrences:

```bash
sed 's/apple/orange/g' fruits.txt
```

**`g`** means global (replace all matches in a line).

---

### 🔹 Replace only on a specific line:

```bash
sed '2s/apple/orange/' fruits.txt
```

Replaces **"apple" with "orange"** only on **line 2**.

---

### 🔹 Case-insensitive replace:

```bash
sed 's/apple/orange/Ig' fruits.txt
```

**`I`** makes the search case-insensitive (matches `apple`, `Apple`, `APPLE`, etc.)

---

## 🧹 3. **Delete Lines**

### 🔹 Delete a specific line:

```bash
sed '3d' fruits.txt
```

Deletes **line 3**.

### 🔹 Delete a range of lines:

```bash
sed '5,10d' fruits.txt
```

Deletes lines **5 to 10**.

### 🔹 Delete lines matching a pattern:

```bash
sed '/error/d' logs.txt
```

Deletes lines that **contain the word "error"**.

---

## ➕ 4. **Insert Text**

### 🔹 Insert before a specific line:

```bash
sed '3i\This is a new line' file.txt
```

Inserts **a new line before line 3**.

---

## ➕ 5. **Append Text**

### 🔹 Append after a specific line:

```bash
sed '3a\This is added after line 3' file.txt
```

---

## ✏️ 6. **Change/Replace Entire Line**

### 🔹 Change content of a specific line:

```bash
sed '2c\Replaced entire line 2' file.txt
```

---

## 🔁 7. **In-place Editing (Modify File Directly)**

> ⚠️ Use with caution!

```bash
sed -i 's/old/new/g' file.txt
```

**`-i`** edits the file directly without showing output.

---

## 📁 8. **Multiple Commands**

### 🔹 Execute multiple commands:

```bash
sed -e 's/apple/orange/g' -e '/banana/d' fruits.txt
```

Replaces **apple → orange** and deletes lines with **banana**.

---

## 🔍 9. **Print Matching Lines (Filter)**

```bash
sed -n '/error/p' logs.txt
```

**`-n`** suppresses default output; `p` prints only matching lines.

---

## 🔐 10. **Replace IP Addresses (Security Use Case)**

```bash
sed -E 's/([0-9]{1,3}\.){3}[0-9]{1,3}/[REDACTED]/g' logs.txt
```

Replaces all **IPv4 addresses** with `[REDACTED]`.

---

## 📌 Summary Table

| Task                 | Command Example                  |
| -------------------- | -------------------------------- |
| Replace text         | `sed 's/foo/bar/' file.txt`      |
| Global replace       | `sed 's/foo/bar/g' file.txt`     |
| Delete line          | `sed '5d' file.txt`              |
| Delete pattern match | `sed '/error/d' logs.txt`        |
| Insert before line   | `sed '2i\Hello' file.txt`        |
| Append after line    | `sed '2a\Hello' file.txt`        |
| Replace full line    | `sed '2c\New content' file.txt`  |
| In-place edit        | `sed -i 's/yes/no/g' config.txt` |
| Print match only     | `sed -n '/fail/p' logs.txt`      |

---










