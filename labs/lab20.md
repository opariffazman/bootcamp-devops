## LAB 20: Linux Command Asas - Navigasi & Pengurusan Fail (Sesi 1)

### Objektif

- Memahami struktur direktori Linux
- Menguasai command navigasi asas (pwd, ls, cd)
- Membuat dan mengurus direktori (mkdir, rmdir)
- Memahami konsep path (absolute vs relative)

### Durasi

2 jam 5 minit

### Prerequisites

- EC2 instance Ubuntu running di public subnet dengan Public IP
- IAM Instance Profile dengan SSM permissions (dari lab sebelumnya)
- Access ke terminal menggunakan Session Manager (SSM)
- Port 80 dibuka dalam Security Group (HTTP)
- Nginx installed (rujuk [Lab 8](lab8.md) jika belum install)

---

## Bahagian 0: Connect ke EC2 Instance

### Langkah 1: Connect Menggunakan Session Manager

1. Pergi ke EC2 Console
2. Select instance anda
3. Click **Connect**
4. Pilih tab **Session Manager**
5. Click **Connect**

### Langkah 2: Switch ke Bash Shell

Selepas connect, anda akan berada dalam `sh` shell. Tukar ke `bash` untuk pengalaman yang lebih baik:

```bash
# Check current shell
echo $SHELL

# Switch to bash
bash

# Verify you're in bash
echo $SHELL
```

Sekarang anda berada dalam bash shell dan ready untuk start lab.

**Nota:** Jika anda disconnect dan reconnect, anda perlu run `bash` command semula.

---

## Bahagian 1: Command pwd

### Langkah 1: Semak Lokasi Semasa

Selepas masuk bash shell, check lokasi semasa anda:

```bash
pwd
```

Anda akan berada di direktori berbeza `/var/snap/amazon-ssm-agent/xxx` kerana SSM connect anda ke directory ini by default.

### Langkah 2: Navigate ke Home Directory

```bash
# Pergi ke home directory
cd ~
pwd
```

Sekarang anda berada di home directory (`/home/ssm-user).

### Langkah 3: Praktis di Pelbagai Lokasi

```bash
# Pergi ke root directory
cd /
pwd

# Pergi ke /var/log
cd /var/log
pwd

# Balik ke home
cd ~
pwd
```

---

## Bahagian 2: Struktur Direktori Linux

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
- `/etc/nginx/` - nginx configuration files
- `/var/log/` - application logs
- `/var/log/nginx/` - nginx access & error logs
- `/var/www/html/` - default nginx web root (Ubuntu)
- `/home/` - user working directories
- `/tmp/` - temporary scripts dan files

### Langkah 1: Explore Nginx Directories

```bash
# Check if nginx config directory exists
ls -ld /etc/nginx

# List nginx config files
ls -l /etc/nginx/

# Check nginx web root
ls -la /var/www/html/

# Check nginx logs directory
ls -lh /var/log/nginx/
```

---

## Bahagian 3: Command ls

### Langkah 1: List Files Asas

```bash
# Pergi ke home directory
cd /

# List files
ls
```

### Langkah 2: List Dengan Details

```bash
# List dengan details (permissions, size, date)
ls -l
```

### Langkah 3: List Hidden Files

```bash
# List termasuk hidden files (files yang start dengan .)
ls -a
```

### Langkah 4: List Dengan Human-Readable Sizes

```bash
# List dengan size yang mudah baca
ls -lh
```

### Langkah 5: Combine Options

```bash
# Combine semua options
ls -lah
```

### Langkah 6: List Nginx Directories

```bash
# List nginx config directory tanpa navigate
ls /etc/nginx

# List nginx sites dengan details
ls -l /etc/nginx/sites-available/

# List nginx logs dengan human-readable sizes
ls -lh /var/log/nginx/

# List nginx html directory
ls -la /var/www/html/
```

### Langkah 7: Sort by Time

```bash
# Check newest first
ls -lt /var/log/nginx/

# Check oldest first
ls -ltr /var/log/nginx/
```

---

## Bahagian 4: Command cd

### Langkah 1: Navigate ke Home Directory

```bash
# Method 1: Guna cd sahaja
cd
pwd

# Method 2: Guna cd dengan ~
cd ~
pwd
```

### Langkah 2: Navigate ke Root Directory

```bash
# Pergi ke root
cd /
pwd
```

### Langkah 3: Navigate ke Nginx Directories

```bash
# Pergi ke nginx config directory
cd /etc/nginx
pwd

# List config files
ls -l

# Check sites-available
ls -l sites-available/

# Check sites-enabled
ls -l sites-enabled/
```

### Langkah 4: Navigate ke Parent Directory

```bash
# Dari /etc/nginx, naik satu level
cd ..
pwd

# Pergi ke nginx logs
cd /var/log/nginx
pwd

# List log files
ls -lh
```

### Langkah 5: Navigate ke Previous Directory

```bash
# Pergi ke nginx config
cd /etc/nginx
pwd

# Pergi ke nginx logs
cd /var/log/nginx
pwd

# Balik ke previous directory (config)
cd -
pwd

# Quick toggle between directories
cd -
pwd
```

### Langkah 6: Absolute Path vs Relative Path

**Absolute path - start dari root (/):**

```bash
# Dari mana-mana location, pergi ke nginx html directory
cd /var/www/html
pwd

# List files dalam html directory
ls -la
```

**Relative path - relative dari current location:**

```bash
# Dari /var/www/html
cd /etc/nginx

# Pergi ke sites-available menggunakan relative path
cd sites-available
pwd

# Balik ke parent directory
cd ..
pwd

# Pergi ke sites-enabled menggunakan relative path
cd sites-enabled
pwd
```

---

## Bahagian 5: Command mkdir

### Langkah 1: Buat Single Directory

```bash
# Pergi ke home
cd ~

# Buat directory
mkdir myproject

# Verify
ls -l
```

### Langkah 2: Buat Multiple Directories

```bash
# Buat multiple directories sekaligus
mkdir logs configs scripts

# Verify
ls -l
```

### Langkah 3: Buat Nested Directories

**Cara yang GAGAL:**

```bash
# Cuba buat nested directory tanpa -p
mkdir project1/web/public
```

**Cara yang BETUL:**

```bash
# Guna -p untuk auto-create parent directories
mkdir -p project1/web/public

# Verify structure
ls -R project1
```

### Langkah 4: Buat Multiple Nested Directories Sekaligus

```bash
# Buat structure dengan satu command
mkdir -p project2/{html,logs,backups}

# Verify
ls -l project2
```

### Langkah 5: Buat Directory Dengan Specific Permissions

```bash
# Buat directory dengan permissions 755 (public read)
mkdir -m 755 public_folder

# Buat directory dengan permissions 700 (owner only)
mkdir -m 700 private_folder

# Verify permissions
ls -ld public_folder private_folder
```

---

## Bahagian 6: Command rmdir

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

### Langkah 3: Cuba Remove Non-Empty Directory

```bash
# Buat file dalam not-empty
touch test/not-empty/sample.txt

# Cuba remove - akan GAGAL
rmdir test/not-empty
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

---

## Bahagian 7: Pengurusan Pengguna & Permissions Asas

### Langkah 1: Check Current User

```bash
# Semak siapa user semasa
whoami
```

### Langkah 2: Understanding File Permissions

```bash
# Lihat file dengan permissions
ls -l
```

**Format permissions: `-rwxr-xr-x`**

```
-rwxr-xr-x
│││││││││
│└┬┘└┬┘└┬┘
│ │  │  └─── Others (all users)
│ │  └────── Group
│ └───────── Owner (user)
└─────────── File type (- = file, d = directory)
```

**Permission meanings:**

- `r` (read) = boleh baca/view file
- `w` (write) = boleh modify/delete file
- `x` (execute) = boleh run file (untuk scripts)

**Contoh:**

```
-rw-r--r--  = Owner: read+write, Group: read, Others: read
-rwxr-xr-x  = Owner: read+write+execute, Group: read+execute, Others: read+execute
drwxr-xr-x  = Directory dengan same permissions
```

### Langkah 3: Practical Example

```bash
# Buat test file
cd ~
touch testfile.txt

# Lihat permissions
ls -l testfile.txt

# Buat directory
mkdir testdir

# Lihat permissions
ls -ld testdir
```

---

## Bahagian 8: Tips & Shortcuts

### Langkah 1: Tab Completion

```bash
# Pergi ke /var
cd /var

# Taip sebahagian, then tekan Tab
cd l[Tab]

# Double Tab untuk tunjuk semua options
cd [Tab][Tab]
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

### Langkah 4: Wildcards untuk DevOps Tasks

```bash
# List all nginx config files
ls /etc/nginx/*.conf

# List all nginx site configs
ls /etc/nginx/sites-available/*

# List all log files
ls /var/log/nginx/*.log

# List all HTML files
ls /usr/share/nginx/html/*.html
```

### Langkah 5: Practical Wildcard Usage

```bash
# Create test log files
cd ~
mkdir log-test
cd log-test
touch app-2024-01.log app-2024-02.log app-2024-03.log error.log access.log

# List all app logs
ls app-*.log

# Match with character set
ls app-2024-0[123].log
```

---

## Bahagian 9: Practikal

### Langkah 1: Explore Nginx Default Setup

```bash
# Navigate ke nginx web root
cd /var/www/html

# List files
ls -la
```

### Langkah 2: View Default index.html

```bash
# View current index.html
cat index.html
```

### Langkah 3: Create Custom HTML Directory Structure

```bash
# Buat directory untuk custom sites
sudo mkdir -p /var/www/{site1,site2}/html

# Buat directory untuk assets
sudo mkdir -p /var/www/site1/html/{css,js,images}

# Verify structure
ls -R /var/www/
```

### Langkah 4: Create Custom index.html

```bash
# Navigate ke site1
cd /var/www/site1/html

# Create custom index.html dengan version 1.0
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Website - Version 1.0</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 40px;
            background-color: #f0f0f0;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        h1 { color: #333; }
        .version { 
            color: #28a745;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to My Website</h1>
        <p>This is <span class="version">Version 1.0</span></p>
        <p>Server: Site1</p>
    </div>
</body>
</html>
EOF

# Verify file created
ls -lh index.html

# View content
cat index.html
```

### Langkah 5: Create Nginx Site Configuration

**Nota:** Ubuntu nginx guna konsep `sites-available` dan `sites-enabled`:

- `sites-available` - semua config files (active atau tidak)
- `sites-enabled` - symbolic links ke configs yang active

```bash
# Navigate ke sites-available
cd /etc/nginx/sites-available

# Create new site configuration
sudo tee mysite > /dev/null << 'EOF'
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /var/www/site1/html;
    index index.html;
    
    server_name _;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# Verify config created
cat mysite
```

### Langkah 6: Enable Site Configuration

```bash
# Remove default site
sudo rm /etc/nginx/sites-enabled/default

# Enable our new site (create symbolic link)
sudo ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/

# Verify symbolic link created
ls -l /etc/nginx/sites-enabled/
```

### Langkah 7: Test and Reload Nginx

```bash
# Test nginx configuration
sudo nginx -t

# Reload nginx
sudo systemctl reload nginx

# Check nginx status
sudo systemctl status nginx
```

### Langkah 8: Verify Deployment

```bash
# Test website in terminal
curl localhost
```

---

## Command Summary

| Command | Fungsi | Contoh DevOps |
|---------|--------|--------|
| `pwd` | Print working directory | `pwd` |
| `ls` | List files | `ls -lh /var/log/nginx/` |
| `ls -lt` | Sort by time | `ls -lt /var/log/nginx/` |
| `cd` | Change directory | `cd /etc/nginx/conf.d` |
| `cd ~` | Go to home | `cd ~` |
| `cd ..` | Go to parent | `cd ..` |
| `cd -` | Go to previous | `cd -` |
| `mkdir` | Make directory | `sudo mkdir -p /var/www/mysite/html` |
| `mkdir -p` | Make nested directories | `mkdir -p site/{html,logs,ssl}` |
| `rmdir` | Remove empty directory | `rmdir old_site` |
