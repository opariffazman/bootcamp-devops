## LAB 1: SSH KEY SETUP

### 🎯 Objektif
Setup SSH authentication untuk GitHub account anda.

### 📋 Prerequisites
- GitHub account (daftar di github.com jika belum ada)
- Git installed di komputer
- Terminal/Command Prompt access

### 📝 Steps

#### Step 1: Generate SSH Key

Buka terminal/command prompt dan run:

```bash
ssh-keygen -t ed25519 -C "email-anda@example.com"
```

**Gantikan** `email-anda@example.com` dengan email GitHub anda.

**Nota:** Jika system anda tak support ed25519, guna:
```bash
ssh-keygen -t rsa -b 4096 -C "email-anda@example.com"
```

**Sistem akan tanya:**

1. **Enter file in which to save the key:**
   ```
   Press ENTER (guna default location)
   ```

2. **Enter passphrase (optional):**
   ```
   Press ENTER untuk kosongkan ATAU
   Taip password untuk extra security
   ```

**Output yang anda patut nampak:**
```
Your identification has been saved in /Users/username/.ssh/id_ed25519
Your public key has been saved in /Users/username/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxx email-anda@example.com
```

---

#### Step 2: Copy Public Key

**Windows (Git Bash):**
```bash
cat ~/.ssh/id_ed25519.pub | clip
```

**Windows (PowerShell):**
```powershell
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
```

**macOS:**
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

**Linux:**
```bash
cat ~/.ssh/id_ed25519.pub
# Copy output manually (Ctrl+Shift+C)
```

**Output akan nampak macam ni:**
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJl3dIeudNqd0PTOXT... email-anda@example.com
```

---

#### Step 3: Add to GitHub

1. **Pergi ke GitHub:**
   - Login → Click avatar (top right) → **Settings**

2. **Navigate to SSH Keys:**
   - Sidebar kiri → **SSH and GPG keys**

3. **Add New Key:**
   - Click **"New SSH key"** (button hijau)

4. **Fill Form:**
   - **Title:** `Laptop [Nama Anda]` (contoh: "Laptop Ahmad")
   - **Key type:** Authentication Key (default)
   - **Key:** Paste public key yang tadi copy

5. **Save:**
   - Click **"Add SSH key"**
   - Confirm dengan password GitHub anda

---

#### Step 4: Test Connection

```bash
ssh -T git@github.com
```

**First time connect, sistem akan tanya:**
```
Are you sure you want to continue connecting (yes/no/fingerprint)?
```
**Taip:** `yes` dan press ENTER

**✅ Success output:**
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

**❌ Jika gagal:**
```bash
# Test dengan verbose
ssh -T git@github.com -v
```

---

### ✅ Lab 1 Checklist

- [ ] SSH key generated successfully
- [ ] Key added to SSH agent
- [ ] Public key added to GitHub
- [ ] Connection test successful
- [ ] Received "Hi username!" message
