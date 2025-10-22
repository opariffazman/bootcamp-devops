## LAB 20: Linux Command Asas - Navigasi & Pengurusan Fail (Sesi 1)

### 🎯 Objektif
- Memahami struktur direktori Linux
- Menguasai command navigasi asas (pwd, ls, cd)
- Membuat dan mengurus direktori (mkdir, rmdir)
- Memahami konsep path (absolute vs relative)

### ⏱️ Durasi
2 jam

### 📋 Prerequisites
- EC2 instance running (boleh guna instance dari lab sebelumnya)
- Access ke terminal (SSH atau Session Manager)

---

## Bahagian 1: Struktur Direktori Linux (15 minit)

### Pengenalan Filesystem Hierarchy

Linux menggunakan struktur tree yang bermula dari root (`/`):

```
/                   (root directory)
├── home/          (user home directories)
├── etc/           (system configuration files)
├── var/           (variable data: logs, databases)
├── usr/           (user programs & utilities)
├── tmp/           (temporary files)
└── opt/           (optional software)
```

**Penting untuk DevOps:**
- `/etc/` - config files untuk services (nginx, docker, etc.)
- `/var/log/` - application logs
- `/home/` - user working directories
- `/tmp/` - temporary scripts dan files

---

## Bahagian 2: Command pwd (10 minit)

### Langkah 1: Semak Lokasi Semasa

```bash
pwd
```

**Expected output:**
```
/home/ec2-user
```

### Langkah 2: Praktis di Pelbagai Lokasi

```bash
# Pergi ke root directory
cd /

# Semak lokasi
pwd
```

**Expected output:**
```
/
```

```bash
# Pergi ke /var/log
cd /var/log

# Semak lokasi
pwd
```

**Expected output:**
```
/var/log
```

```bash
# Balik ke home
cd ~

# Semak lokasi
pwd
```

**Expected output:**
```
/home/ec2-user
```

**✅ Success:** Anda faham command `pwd` untuk check current directory

---

## Bahagian 3: Command ls (25 minit)

### Langkah 1: List Files Asas

```bash
# Pergi ke home directory
cd ~

# List files
ls
```

### Langkah 2: List Dengan Details

```bash
# List dengan details (permissions, size, date)
ls -l
```

**Output format:**
```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 21 10:30 folder-name
-rw-r--r-- 1 ec2-user ec2-user  125 Oct 21 10:25 file.txt
│││││││││ │ │        │        │    │             │
│││││││││ │ │        │        │    │             └─ Nama
│││││││││ │ │        │        │    └─ Date modified
│││││││││ │ │        │        └─ Size (bytes)
│││││││││ │ │        └─ Group
│││││││││ │ └─ Owner
│││││││││ └─ Number of links
└────────── Permissions (d=directory, -=file)
```

### Langkah 3: List Hidden Files

```bash
# List termasuk hidden files (files yang start dengan .)
ls -a
```

**Expected output akan show:**
```
.
..
.bash_logout
.bash_profile
.bashrc
```

### Langkah 4: List Dengan Human-Readable Sizes

```bash
# List dengan size yang mudah baca
ls -lh
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 1.2K Oct 21 10:25 file.txt
-rw-r--r-- 1 ec2-user ec2-user  15M Oct 21 10:30 large.log
```

### Langkah 5: Combine Options

```bash
# Combine semua options
ls -lah
```

### Langkah 6: List Specific Directory

```bash
# List directory lain tanpa tukar directory
ls /var/log

# List dengan details
ls -lh /var/log
```

### Langkah 7: Sort by Time

```bash
# List dengan sorting by time (newest first)
ls -lt

# Reverse order (oldest first)
ls -ltr
```

**✅ Success:** Anda faham pelbagai cara guna `ls`

---

## Bahagian 4: Command cd (25 minit)

### Langkah 1: Navigate ke Home Directory

```bash
# Method 1: Guna cd sahaja
cd

# Verify
pwd
```

```bash
# Method 2: Guna cd dengan ~
cd ~

# Verify
pwd
```

**Expected output:**
```
/home/ec2-user
```

### Langkah 2: Navigate ke Root Directory

```bash
# Pergi ke root
cd /

# Verify
pwd
```

**Expected output:**
```
/
```

### Langkah 3: Navigate ke Parent Directory

```bash
# Buat nested structure dulu
cd ~
mkdir -p projects/webapp/frontend

# Masuk ke frontend
cd projects/webapp/frontend
pwd
```

**Expected output:**
```
/home/ec2-user/projects/webapp/frontend
```

```bash
# Naik satu level (ke webapp)
cd ..
pwd
```

**Expected output:**
```
/home/ec2-user/projects/webapp
```

```bash
# Naik dua level (ke home)
cd ../..
pwd
```

**Expected output:**
```
/home/ec2-user
```

### Langkah 4: Navigate ke Previous Directory

```bash
# Pergi ke /var/log
cd /var/log
pwd

# Pergi ke /etc
cd /etc
pwd

# Balik ke previous directory (/var/log)
cd -
pwd
```

**Expected output:**
```
/var/log
```

### Langkah 5: Absolute Path vs Relative Path

**Absolute path - start dari root (/):**

```bash
# Dari mana-mana location, command ni akan pergi ke /var/log
cd /var/log
pwd
```

```bash
# Dari mana-mana location, pergi ke home
cd /home/ec2-user
pwd
```

**Relative path - relative dari current location:**

```bash
# Pergi ke home dulu
cd ~

# Buat structure
mkdir -p Documents/projects

# Pergi ke projects menggunakan relative path
cd Documents/projects
pwd
```

**Expected output:**
```
/home/ec2-user/Documents/projects
```

```bash
# Dari projects, naik ke Documents, then ke Downloads (jika ada)
cd ..
pwd
```

**✅ Success:** Anda faham navigate menggunakan absolute dan relative paths

---

## Bahagian 5: Command mkdir (25 minit)

### Langkah 1: Buat Single Directory

```bash
# Pergi ke home
cd ~

# Buat directory
mkdir devops-training

# Verify
ls -l
```

### Langkah 2: Buat Multiple Directories

```bash
# Buat multiple directories sekaligus
mkdir scripts configs logs

# Verify
ls -l
```

**Expected output:**
```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 configs
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 devops-training
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 logs
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 scripts
```

### Langkah 3: Buat Nested Directories

**❌ Cara yang GAGAL:**

```bash
# Cuba buat nested directory tanpa -p
mkdir webapp/frontend/components
```

**Expected error:**
```
mkdir: cannot create directory 'webapp/frontend/components': No such file or directory
```

**✅ Cara yang BETUL:**

```bash
# Guna -p untuk auto-create parent directories
mkdir -p webapp/frontend/components

# Verify structure
ls -R webapp
```

**Expected output:**
```
webapp:
frontend

webapp/frontend:
components

webapp/frontend/components:
```

### Langkah 4: Buat Complex Structure

```bash
# Buat project structure untuk DevOps
cd ~
mkdir -p devops-project/{scripts,configs,logs,backups}

# Verify
ls -l devops-project
```

**Expected output:**
```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 backups
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 configs
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 logs
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 scripts
```

### Langkah 5: Buat Multi-Level Structure

```bash
# Setup typical web application structure
cd ~
mkdir -p myapp/{src/{controllers,models,views},tests,config,logs}

# Verify
ls -R myapp
```

**Expected output:**
```
myapp:
config  logs  src  tests

myapp/config:

myapp/logs:

myapp/src:
controllers  models  views

myapp/src/controllers:

myapp/src/models:

myapp/src/views:

myapp/tests:
```

### Langkah 6: Buat Directory Dengan Specific Permissions

```bash
# Buat directory dengan permissions 755
mkdir -m 755 public_files

# Verify permissions
ls -ld public_files
```

**Expected output:**
```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:10 public_files
```

**✅ Success:** Anda faham buat directories dengan pelbagai cara

---

## Bahagian 6: Command rmdir (15 minit)

### Langkah 1: Buat Test Directories

```bash
cd ~
mkdir -p test/empty/nested
mkdir test/not-empty
```

### Langkah 2: Remove Empty Directory

```bash
# Remove nested empty directory
rmdir test/empty/nested

# Verify
ls -R test
```

**Expected output:**
```
test:
empty  not-empty

test/empty:

test/not-empty:
```

### Langkah 3: Cuba Remove Non-Empty Directory

```bash
# Buat file dalam not-empty
touch test/not-empty/sample.txt

# Cuba remove - akan GAGAL
rmdir test/not-empty
```

**Expected error:**
```
rmdir: failed to remove 'test/not-empty': Directory not empty
```

### Langkah 4: Remove Nested Empty Directories

```bash
# Buat nested empty structure
mkdir -p cleanup/level1/level2/level3

# Remove semua levels sekaligus
rmdir -p cleanup/level1/level2/level3

# Verify - semua dah gone
ls cleanup
```

**Expected error (good!):**
```
ls: cannot access 'cleanup': No such file or directory
```

**✅ Success:** Anda faham `rmdir` hanya untuk empty directories

---

## Bahagian 7: Tips & Shortcuts (15 minit)

### Langkah 1: Tab Completion

```bash
# Pergi ke /var
cd /var

# Taip sebahagian, then tekan Tab
cd l[Tab]
```

**Expected:** Auto-complete ke `cd log/` atau tunjuk options

```bash
# Double Tab untuk tunjuk semua options
cd [Tab][Tab]
```

**Expected output:**
```
backups/ cache/ crash/ lib/ local/ lock/ log/ mail/ opt/ run/ snap/ spool/ tmp/
```

### Langkah 2: Command History

```bash
# View command history
history

# Ulang previous command
!!

# Search history dengan Ctrl+R
# Tekan Ctrl+R, then taip keyword contoh 'mkdir'
# Tekan Ctrl+R lagi untuk next match
```

### Langkah 3: Clear Screen

```bash
# Clear screen
clear

# Atau guna shortcut
# Tekan Ctrl+L
```

### Langkah 4: Wildcards

```bash
# Setup test files
cd ~
mkdir wildcard-test
cd wildcard-test
touch file1.txt file2.txt file3.txt test1.log test2.log

# List semua .txt files
ls *.txt
```

**Expected output:**
```
file1.txt  file2.txt  file3.txt
```

```bash
# List semua files starting dengan test
ls test*
```

**Expected output:**
```
test1.log  test2.log
```

```bash
# Match single character dengan ?
touch fileA.txt fileB.txt
ls file?.txt
```

**Expected output:**
```
file1.txt  file2.txt  file3.txt  fileA.txt  fileB.txt
```

```bash
# Match character set dengan []
ls file[123].txt
```

**Expected output:**
```
file1.txt  file2.txt  file3.txt
```

**✅ Success:** Anda faham shortcuts dan wildcards

---

## Bahagian 8: Hands-On Exercise (20 minit)

### Exercise 1: Setup Development Environment

**Task:** Buat structure ini dalam satu command:

```
development/
├── frontend/
│   ├── react-app/
│   └── vue-app/
├── backend/
│   ├── nodejs/
│   └── python/
└── devops/
    ├── docker/
    ├── terraform/
    └── ansible/
```

**Langkah-langkah:**

```bash
# 1. Pergi ke home
cd ~

# 2. Buat structure
mkdir -p development/{frontend/{react-app,vue-app},backend/{nodejs,python},devops/{docker,terraform,ansible}}

# 3. Verify
ls -R development
```

**Expected output:**
```
development:
backend  devops  frontend

development/backend:
nodejs  python

development/backend/nodejs:

development/backend/python:

development/devops:
ansible  docker  terraform

development/devops/ansible:

development/devops/docker:

development/devops/terraform:

development/frontend:
react-app  vue-app

development/frontend/react-app:

development/frontend/vue-app:
```

### Exercise 2: Directory Navigation Challenge

**Task:** Navigate mengikut arahan tanpa tengok jawapan dulu!

```bash
# 1. Pergi ke home
cd ~

# 2. Pergi ke /var/log
cd /var/log

# 3. List semua files dengan details
ls -la

# 4. Balik ke previous directory
cd -

# 5. Pergi ke /etc
cd /etc

# 6. List files yang start dengan 'host'
ls host*

# 7. Pergi ke parent directory
cd ..

# 8. Verify you're at root (/)
pwd

# 9. Balik ke home
cd ~

# 10. Verify location
pwd
```

**Expected final output:**
```
/home/ec2-user
```

### Exercise 3: Microservices Project Setup

**Task:** Setup project structure untuk microservices

```bash
# 1. Pergi ke home
cd ~

# 2. Buat project structure
mkdir -p microservices/{services/{auth,payment,notification},infrastructure/{terraform,k8s,monitoring},docs}

# 3. Verify structure
ls -R microservices

# 4. Navigate ke monitoring directory
cd microservices/infrastructure/monitoring

# 5. Verify position
pwd

# 6. Pergi ke auth service (guna relative path)
cd ../../services/auth

# 7. Verify position
pwd

# 8. Balik ke home
cd ~
```

**Expected output untuk step 5:**
```
/home/ec2-user/microservices/infrastructure/monitoring
```

**Expected output untuk step 7:**
```
/home/ec2-user/microservices/services/auth
```

**✅ Success:** Anda berjaya setup complex directory structures!

---

## 🎓 Kesimpulan

Dalam lab ini anda telah belajar:

✅ Struktur direktori Linux  
✅ Command `pwd` untuk check current location  
✅ Command `ls` dengan pelbagai options  
✅ Command `cd` untuk navigation  
✅ Command `mkdir` untuk buat directories  
✅ Command `rmdir` untuk remove empty directories  
✅ Absolute vs relative paths  
✅ Tab completion & shortcuts  
✅ Wildcards untuk pattern matching  

### Command Summary

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `pwd` | Print working directory | `pwd` |
| `ls` | List files | `ls -lah` |
| `cd` | Change directory | `cd /var/log` |
| `cd ~` | Go to home | `cd ~` |
| `cd ..` | Go to parent | `cd ..` |
| `cd -` | Go to previous | `cd -` |
| `mkdir` | Make directory | `mkdir folder` |
| `mkdir -p` | Make nested directories | `mkdir -p path/to/dir` |
| `rmdir` | Remove empty directory | `rmdir folder` |

---

## ✅ Lab 20 Checklist

Pastikan anda boleh:

- [ ] Tahu current position dengan `pwd`
- [ ] List files dengan `ls -l`, `ls -a`, `ls -lah`
- [ ] Navigate menggunakan absolute path
- [ ] Navigate menggunakan relative path
- [ ] Pergi ke home dengan `cd ~`
- [ ] Pergi ke parent dengan `cd ..`
- [ ] Buat directory dengan `mkdir`
- [ ] Buat nested directories dengan `mkdir -p`
- [ ] Remove empty directory dengan `rmdir`
- [ ] Guna Tab completion
- [ ] Guna wildcards (`*`, `?`, `[]`)
- [ ] Setup complex directory structure dalam satu command
