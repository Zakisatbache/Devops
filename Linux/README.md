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



