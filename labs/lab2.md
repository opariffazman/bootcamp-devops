## LAB 2: FORK & CLONE REPOSITORY

### 🎯 Objektif
Fork repository instructor dan clone ke komputer sendiri.

### 📋 Repository Info
**Repository:** `github.com/opariffazman/bootcamp-devops`

### 📝 Steps

#### Step 1: Fork Repository

1. **Navigate to repository:**
   - Open browser → `https://github.com/opariffazman/bootcamp-devops`

2. **Fork repository:**
   - Click **"Fork"** button (top right, beside Star)
   - Pastikan **"Copy the main branch only"** is checked
   - Click **"Create fork"**

3. **Wait for fork:**
   - GitHub akan buat copy ke account anda
   - URL akan jadi: `github.com/[your-username]/bootcamp-devops`

**✅ Success:** Anda akan nampak banner "forked from opariffazman/bootcamp-devops"

---

#### Step 2: Clone YOUR Fork

1. **Get SSH URL dari YOUR fork:**
   - Di YOUR forked repo page
   - Click **"Code"** button (hijau)
   - Pilih **"SSH"** tab
   - Copy URL (format: `git@github.com:your-username/bootcamp-devops.git`)

2. **Open terminal dan navigate ke folder kerja:**

```bash
# Contoh: Navigate ke Documents
cd ~/Documents

# Atau buat folder baru
mkdir devops-labs
cd devops-labs
```

3. **Clone repository:**

```bash
git clone git@github.com:[your-username]/bootcamp-devops.git
```

**Gantikan `[your-username]` dengan username GitHub anda!**

**Expected output:**
```
Cloning into 'bootcamp-devops'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 15 (delta 2), reused 15 (delta 2), pack-reused 0
Receiving objects: 100% (15/15), done.
Resolving deltas: 100% (2/2), done.
```

4. **Enter directory:**

```bash
cd bootcamp-devops
```

5. **Verify clone successful:**

```bash
# Check files
ls -la

# Check git status
git status

# Check remote
git remote -v
```

**Expected output untuk `git remote -v`:**
```
origin  git@github.com:your-username/bootcamp-devops.git (fetch)
origin  git@github.com:your-username/bootcamp-devops.git (push)
```

---

#### Step 3: Setup Upstream Remote

Upstream adalah link ke original repository (instructor punya).

```bash
git remote add upstream git@github.com:opariffazman/bootcamp-devops.git
```

**Verify:**
```bash
git remote -v
```

**Expected output:**
```
origin    git@github.com:your-username/bootcamp-devops.git (fetch)
origin    git@github.com:your-username/bootcamp-devops.git (push)
upstream  git@github.com:opariffazman/bootcamp-devops.git (fetch)
upstream  git@github.com:opariffazman/bootcamp-devops.git (push)
```

**Penjelasan:**
- **origin** = your fork (anda boleh push)
- **upstream** = original repo (anda tak boleh push, hanya fetch)

---

### ✅ Lab 2 Checklist

- [ ] Repository forked successfully
- [ ] Fork cloned to local computer
- [ ] Inside `bootcamp-devops` directory
- [ ] `origin` remote pointing to YOUR fork
- [ ] `upstream` remote pointing to original repo
- [ ] Can see repository files
