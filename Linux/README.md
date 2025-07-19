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
======================================================================================

### Example of **Restrict SSH Access to Specific Users**
---
[root@Test ~]# useradd user1
[root@Test ~]# mkdir -p /home/user1/.ssh
[root@Test ~]# cp /home/ec2-user/.ssh/authorized_keys /home/user1/.ssh/
[root@Test ~]# chown -R zaki:zaki /home/user1/.ssh
[root@Test ~]# chmod 700 /home/user1/.ssh
[root@Test ~]# chmod 600 /home/user1/.ssh/authorized_keys
[root@Test ~]# ls
[root@Test ~]# pwd
/root
[root@Test ~]# cd /home/
[root@Test home]# ls
abrar  ec2-user  user1  zaki
[root@Test home]# sudo usermod -aG wheel user1
[root@Test /]# passwd user1
Changing password for user user1.
New password:
BAD PASSWORD: The password contains the user name in some form
Retype new password:
passwd: all authentication tokens updated successfully.
[root@Test /]# vi /etc/ssh/sshd_config
[root@Test /]# sudo systemctl restart sshd
[root@Test zaki]# cat ~/.ssh/authorized_keys
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ec2-user\" rather than the user \"root\".';echo;sleep 10;exit 142" ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpEGvF3pke8Ui5BdVzS8L7E4fNDOi95iPq4Xpn/uKKhEhRA/jQ2EPcdHtE3EPBt6te5jD3mW6APHXUSeUUr06KY3JgWC1YT08NDqhj4irYB2JWIstIxsmjDqnYL4dgz6sXXkuOrQ/2GMnHu3Ab4l4OFCyE3XWnLTbHYnoR6qPW9mD2fMqXm8aW4f/1ZlXN6ZJSthKW662aXg2BSB2wlo4PMgS2LiEBlf6bFeiwCcexTpJCbmFVHPHtKWwVH5fF7pHsEF5x49oqRK6qb1eb9dNsGQtDnb8mXY9ic8i17f7+i3+0KP4mflCHnzByAEvCwsYxqJrMVoSb+IwhMtAeF8IB MC 1 Key
[root@Test zaki]# cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCnGVM62v+KlUOaHic1DT7di4WnXEvp4WrbJz8soWjJrobo4tL8GVPgL8qIkxs93EhjRYazTtdF4UjiVbrSEIf8D34WEh/RlWTtxFEeISDU0nXhd82YyI4M6b0cwSlvrl9rohVQm2RCpuHDWEQz6Yt5/DVfb3CrE/uegLSbN3F06FZA1F3MwIhENTNetHKdLs2QRYbaNgFUqR+tXkvN1qE0gtjOlGqR3KYHGwAI8IOBXFSj5EWef3wFcoZ6eBF06TaN091WqJM+AfNNTj7hD9rZELMISg27R3JPrDiIi3If7EpE2SGqJRkzRGrCWOvu8G9yKyfeb6B4NEIUzHcKovdpdPf3QcIQhMpAvhEX/m8d1mo3qtkU8nIZlvM+rZuSDsP7Lj1H3nd76GJQ9vhH6wTpIGlhJzG4euoDfUcBWta8Ze7113ZnG+6X/5hrnFZ3Wbf40yvT0FUKePfzNoMSohnH+SiVUOxXWgLHiQ9UoldALvLa7eGARwVo/UkZcNhdrw8= root@Test
[root@Test zaki]# su - user1
Last login: Fri Jul 18 20:56:53 UTC 2025 on pts/1
[user1@Test ~]$ sudo vi .ssh/authorized_keys
[user1@Test ~]$ sudo systemctl restart sshd
[user1@Test ~]$ exit
logout
[root@Test zaki]# ssh user1@65.1.130.219
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
Last login: Fri Jul 18 21:01:37 2025
[user1@Test ~]$ exit
logout

---

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













