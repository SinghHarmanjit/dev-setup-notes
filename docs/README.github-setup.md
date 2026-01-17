
# 📘 Managing Multiple GitHub Accounts with SSH
*A beginner‑friendly guide for setting up SSH keys for personal and work GitHub accounts.*

This guide explains how to:

- Generate separate SSH keys for each GitHub account  
- Configure your laptop to use the correct key automatically  
- Clone and manage repositories with different identities  
- Avoid common authentication problems  

Perfect for developers who switch between **personal** and **work** GitHub accounts on the same machine.

---

## 🧩 Why You Need Separate SSH Keys
GitHub uses SSH keys to authenticate you.  
If you have two different accounts (for example: **personal** and **work**):

- Each account must have **its own SSH key**
- Your local machine must be configured to use the **right key for the right repo**
- Git must use the correct **username + email** for commits

This README shows you exactly how to do that.

---

# 🔐 1. Generate SSH Keys for Each Account

### Personal Account
```bash
ssh-keygen -t ed25519 -C "your_personal_email@example.com" -f ~/.ssh/id_ed25519_personal
```

### Work Account
```bash
ssh-keygen -t ed25519 -C "your_work_email@example.com" -f ~/.ssh/id_ed25519_work
```

You will end up with:
```
~/.ssh/id_ed25519_personal
~/.ssh/id_ed25519_personal.pub
~/.ssh/id_ed25519_work
~/.ssh/id_ed25519_work.pub
```

### Add each key to your SSH agent
macOS:
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_personal
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_work
```

Linux:
```bash
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

Windows PowerShell:
```powershell
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519_personal
ssh-add $env:USERPROFILE\.ssh\id_ed25519_work
```

---

# 🔑 2. Add Public Keys to GitHub
For each account, copy the public key:

macOS:
```bash
pbcopy < ~/.ssh/id_ed25519_personal.pub
```

Linux:
```bash
cat ~/.ssh/id_ed25519_personal.pub
```

Windows:
```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519_personal.pub
```

Then:
1. Go to **GitHub → Settings → SSH and GPG keys**  
2. Click **New SSH Key**  
3. Paste the key  
4. Name them clearly: *Laptop Personal*, *Laptop Work*  

Repeat for your work account.

---

# ⚙️ 3. Configure the `~/.ssh/config` File
This is the **most important step**.

You cannot use `github.com` twice.  
So you must create **aliases** like:

- `github-personal`  
- `github-work`

Create or edit the config file:
```bash
nano ~/.ssh/config
```

Paste this:
```ssh
# Personal GitHub Account
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes
  AddKeysToAgent yes

# Work GitHub Account
Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
  IdentitiesOnly yes
  AddKeysToAgent yes
```

### What this does:
- Both hosts talk to `github.com`
- Each uses its own SSH key
- Git selects the correct identity automatically

---

# 📦 4. Cloning Repositories Using the Correct Account

### Personal repo:
```bash
git clone git@github-personal:YOUR_USERNAME/YOUR_REPO.git
```

### Work repo:
```bash
git clone git@github-work:YOUR_COMPANY/YOUR_REPO.git
```

---

# 🧑‍💻 5. Set Git Identity Per Repository (Important)

Inside a *personal* repo:
```bash
git config user.name "Your Personal Name"
git config user.email "your_personal_email@example.com"
```

Inside a *work* repo:
```bash
git config user.name "Your Work Name"
git config user.email "your_work_email@company.com"
```

---

# 🧪 6. Test Your SSH Setup
```bash
ssh -T git@github-personal
ssh -T git@github-work
```

You should see:
```
Hi <username>! You've successfully authenticated...
```

---

# 🛠️ 7. Troubleshooting

### ❌ Permission denied (publickey)
- Key not added to agent → `ssh-add ~/.ssh/...`
- Wrong remote URL
- Incorrect config file alias

### ❌ Cloned with HTTPS by accident?
```bash
git remote set-url origin git@github-personal:USER/REPO.git
```
Or:
```bash
git remote set-url origin git@github-work:COMPANY/REPO.git
```

### ❌ Wrong Git identity?
```bash
git config user.email
```

---

# 🎯 Summary
- Create **separate SSH keys** for personal & work  
- Add keys to each GitHub account  
- Use `~/.ssh/config` aliases (`github-personal`, `github-work`)  
- Clone repos using the alias-based SSH URL  
- Set Git identity per repository  

---

# 👍 You're Ready!
This setup makes switching between multiple GitHub accounts effortless and error‑free.
