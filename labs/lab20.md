## LAB 20: Linux Command Asas - Navigasi & Pengurusan Fail (Sesi 1)

### 🎯 Objektif

- Memahami struktur direktori Linux
- Menguasai command navigasi asas (pwd, ls, cd)
- Membuat dan mengurus direktori (mkdir, rmdir)
- Memahami konsep path (absolute vs relative)

### ⏱️ Durasi

2 jam

### 📋 Prerequisites

- EC2 instance running di public subnet & mempunyai Public IP
- Access ke terminal (SSH atau Session Manager)
- Port 8080, 8081, 8082 dibuka dalam Security Group (untuk testing websites)

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
- `/etc/nginx/` - nginx configuration files
- `/var/log/` - application logs
- `/var/log/nginx/` - nginx access & error logs
- `/usr/share/nginx/html/` - default nginx web root
- `/var/www/html/` - common web root directory
- `/home/` - user working directories
- `/tmp/` - temporary scripts dan files

### Langkah 1: Explore Nginx Directories

```bash
# Check if nginx config directory exists
ls -ld /etc/nginx

# List nginx config files
ls -l /etc/nginx/

# Check nginx web root
ls -la /usr/share/nginx/html/

# Check nginx logs directory
ls -lh /var/log/nginx/
```

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

### Langkah 6: List Nginx Directories

```bash
# List nginx config directory tanpa navigate
ls /etc/nginx

# List nginx sites dengan details
ls -l /etc/nginx/sites-available/

# List nginx logs dengan human-readable sizes
ls -lh /var/log/nginx/

# List nginx html directory
ls -la /usr/share/nginx/html/
```

### Langkah 7: Sort by Time (Monitoring Log Files)

```bash
# Check nginx logs sorted by time (newest first)
ls -lt /var/log/nginx/

# Check oldest logs first (untuk cleanup)
ls -ltr /var/log/nginx/

# Check system logs by time
ls -lth /var/log/ | head -10
```

**Practical usage:** Ini penting untuk DevOps untuk:

- Identify latest log files
- Monitor which files are being actively written
- Find old logs for cleanup

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

### Langkah 3: Navigate ke Nginx Directories

```bash
# Pergi ke nginx config directory
cd /etc/nginx
pwd
```

**Expected output:**

```
/etc/nginx
```

```bash
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
```

**Expected output:**

```
/etc
```

```bash
# Pergi ke nginx logs
cd /var/log/nginx
pwd
```

**Expected output:**

```
/var/log/nginx
```

```bash
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
```

**Expected output:**

```
/etc/nginx
```

```bash
# Quick toggle between directories
cd -  # akan balik ke /var/log/nginx
pwd
```

### Langkah 6: Absolute Path vs Relative Path

**Absolute path - start dari root (/):**

```bash
# Dari mana-mana location, pergi ke nginx html directory
cd /usr/share/nginx/html
pwd
```

**Expected output:**

```
/usr/share/nginx/html
```

```bash
# List files dalam html directory
ls -la
```

**Relative path - relative dari current location:**

```bash
# Dari /usr/share/nginx/html
cd /etc/nginx

# Pergi ke conf.d menggunakan relative path
cd conf.d
pwd
```

**Expected output:**

```
/etc/nginx/conf.d
```

```bash
# Balik ke parent directory
cd ..
pwd
```

**Expected output:**

```
/etc/nginx
```

```bash
# Pergi ke sites-available menggunakan relative path
cd sites-available
pwd
```

**✅ Success:** Anda faham navigate menggunakan absolute dan relative paths

---

## Bahagian 5: Command mkdir (25 minit)

### Langkah 1: Setup Web Application Directory

```bash
# Pergi ke nginx html directory
cd /usr/share/nginx/html

# Buat directory untuk static assets
sudo mkdir css js images

# Verify
ls -l
```

**Expected output:**

```
drwxr-xr-x 2 root root 4096 Oct 22 10:00 css
drwxr-xr-x 2 root root 4096 Oct 22 10:00 images
-rw-r--r-- 1 root root  615 Oct 21 09:30 index.html
drwxr-xr-x 2 root root 4096 Oct 22 10:00 js
```

### Langkah 2: Buat Project Structure

```bash
# Pergi ke home
cd ~

# Buat DevOps project structure
mkdir -p webapp/{deployment,monitoring,backup}

# Verify
ls -l webapp
```

**Expected output:**

```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 backup
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 deployment
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:00 monitoring
```

### Langkah 3: Buat Nested Directories untuk Assets

**❌ Cara yang GAGAL:**

```bash
# Cuba buat nested directory tanpa -p
mkdir webapp/assets/images/products
```

**Expected error:**

```
mkdir: cannot create directory 'webapp/assets/images/products': No such file or directory
```

**✅ Cara yang BETUL:**

```bash
# Guna -p untuk auto-create parent directories
cd ~/webapp
mkdir -p assets/{images/{products,banners},css,js}

# Verify structure
ls -R assets
```

**Expected output:**

```
assets:
css  images  js

assets/css:

assets/images:
banners  products

assets/images/banners:

assets/images/products:

assets/js:
```

### Langkah 4: Setup Nginx Site Structure

```bash
# Buat custom site structure
cd ~
mkdir -p sites/myapp/{public,logs,ssl,backups}

# Verify
ls -l sites/myapp
```

**Expected output:**

```
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 backups
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 logs
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 public
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:05 ssl
```

### Langkah 5: Buat Directory Dengan Specific Permissions

```bash
# Buat directory untuk web content (public read)
cd ~/sites/myapp
mkdir -m 755 public_html

# Buat directory untuk private data (owner only)
mkdir -m 700 private_data

# Verify permissions
ls -ld public_html private_data
```

**Expected output:**

```
drwx------ 2 ec2-user ec2-user 4096 Oct 22 10:10 private_data
drwxr-xr-x 2 ec2-user ec2-user 4096 Oct 22 10:10 public_html
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

## Bahagian 7: Tips & Shortcuts untuk DevOps (15 minit)

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

**Expected output contoh:**

```
/var/log/nginx/access.log
/var/log/nginx/error.log
```

```bash
# Match specific patterns
cd /var/log

# List all .log files
ls *.log

# List all compressed logs
ls *.gz

# List nginx logs only
ls nginx/*.log
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
```

**Expected output:**

```
app-2024-01.log  app-2024-02.log  app-2024-03.log
```

```bash
# Match with character set
ls app-2024-0[123].log
```

**Expected output:**

```
app-2024-01.log  app-2024-02.log  app-2024-03.log
```

**✅ Success:** Anda faham wildcards untuk DevOps tasks

---

## Bahagian 8: Practical DevOps - Working with Nginx (25 minit)

### Langkah 1: Install Nginx (jika belum ada)

```bash
# Install nginx
sudo yum install -y nginx

# Start nginx service
sudo systemctl start nginx

# Enable nginx to start on boot
sudo systemctl enable nginx

# Check nginx status
sudo systemctl status nginx
```

### Langkah 2: Explore Nginx Default Setup

```bash
# Navigate ke nginx web root
cd /usr/share/nginx/html

# List files
ls -la
```

**Expected output:**

```
total 20
drwxr-xr-x 2 root root 4096 Oct 22 10:00 .
drwxr-xr-x 4 root root 4096 Oct 22 10:00 ..
-rw-r--r-- 1 root root  497 Oct 21 09:30 50x.html
-rw-r--r-- 1 root root  615 Oct 21 09:30 index.html
-rw-r--r-- 1 root root 3650 Oct 21 09:30 nginx-logo.png
```

### Langkah 3: View Default index.html

```bash
# View current index.html
cat index.html
```

### Langkah 4: Create Custom HTML Directory Structure

```bash
# Buat directory untuk custom sites
sudo mkdir -p /var/www/{site1,site2}/html

# Buat directory untuk assets
sudo mkdir -p /var/www/site1/html/{css,js,images}

# Verify structure
ls -R /var/www/
```

### Langkah 5: Create Custom index.html

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
        <p>Last Updated: $(date)</p>
    </div>
</body>
</html>
EOF

# Verify file created
ls -lh index.html

# View content
cat index.html
```

### Langkah 6: Update Nginx Configuration

```bash
# Navigate ke nginx sites-available
cd /etc/nginx/conf.d

# Create new site configuration
sudo tee site1.conf > /dev/null << 'EOF'
server {
    listen 8080;
    server_name _;
    
    root /var/www/site1/html;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# Verify config file
cat site1.conf
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
# Test website (in terminal)
curl http://localhost:8080

# Or get server IP and test in browser
curl http://$(hostname -I | awk '{print $1}'):8080
```

**You should see:** Your custom HTML with "Version 1.0"

### Langkah 9: Make Changes and See Instant Updates

```bash
# Navigate ke html directory
cd /var/www/site1/html

# Update version to 2.0
sudo sed -i 's/Version 1.0/Version 2.0/g' index.html
sudo sed -i 's/#28a745/#dc3545/g' index.html

# Verify changes
cat index.html | grep -i version

# Test updated website
curl http://localhost:8080 | grep -i version
```

**Expected output:**

```
        <p>This is <span class="version">Version 2.0</span></p>
```

### Langkah 10: Navigate Between Configurations and Logs

```bash
# Check nginx config
cd /etc/nginx/conf.d
pwd
ls -l

# Check nginx logs
cd /var/log/nginx
pwd
ls -lh

# View access log
sudo tail -20 access.log

# Quick toggle back to web root
cd /var/www/site1/html
pwd

# Quick toggle to config
cd /etc/nginx/conf.d
pwd

# Use cd - to toggle between directories
cd -
pwd

# Toggle back
cd -
pwd
```

**✅ Success:** Anda telah deploy website dan lihat perubahan secara real-time!

**Key Learnings:**

- Setup nginx web directories
- Create custom HTML content
- Navigate between config, web root, and logs
- Make changes and verify instantly
- Understand real DevOps workflow

---

## Bahagian 9: Hands-On Final Exercise - Multiple Sites (30 minit)

### Exercise 1: Deploy Multiple Websites

**Scenario:** Deploy 2 websites yang berbeza dengan versions yang berbeza.

```bash
# 1. Create directories untuk 2 sites
sudo mkdir -p /var/www/{blog,shop}/html

# 2. Create blog site (Version 1.0)
cd /var/www/blog/html
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Blog - v1.0</title>
    <style>
        body { 
            font-family: Arial; 
            margin: 40px; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .container { 
            background: rgba(255,255,255,0.1); 
            padding: 30px; 
            border-radius: 10px; 
        }
        h1 { color: #fff; }
        .version { 
            background: #28a745; 
            padding: 5px 10px; 
            border-radius: 5px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌟 My Awesome Blog</h1>
        <p>Version: <span class="version">1.0</span></p>
        <p>Status: Production</p>
        <p>Type: Blog Platform</p>
    </div>
</body>
</html>
EOF

# 3. Create shop site (Version 1.0)
cd /var/www/shop/html
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Shop - v1.0</title>
    <style>
        body { 
            font-family: Arial; 
            margin: 40px; 
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }
        .container { 
            background: rgba(255,255,255,0.1); 
            padding: 30px; 
            border-radius: 10px; 
        }
        h1 { color: #fff; }
        .version { 
            background: #007bff; 
            padding: 5px 10px; 
            border-radius: 5px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🛒 My Online Shop</h1>
        <p>Version: <span class="version">1.0</span></p>
        <p>Status: Production</p>
        <p>Type: E-commerce Platform</p>
    </div>
</body>
</html>
EOF

# 4. Create nginx configs untuk both sites
cd /etc/nginx/conf.d

sudo tee blog.conf > /dev/null << 'EOF'
server {
    listen 8081;
    server_name _;
    root /var/www/blog/html;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

sudo tee shop.conf > /dev/null << 'EOF'
server {
    listen 8082;
    server_name _;
    root /var/www/shop/html;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# 5. Test and reload nginx
sudo nginx -t
sudo systemctl reload nginx

# 6. Test both sites
echo "Testing Blog Site:"
curl http://localhost:8081 | grep -i version

echo -e "\nTesting Shop Site:"
curl http://localhost:8082 | grep -i version
```

### Exercise 2: Update and Compare Versions

```bash
# 1. Navigate ke blog site
cd /var/www/blog/html
pwd

# 2. Update blog to version 2.0
sudo sed -i 's/v1.0/v2.0/g' index.html
sudo sed -i 's/Version: <span class="version">1.0/Version: <span class="version">2.0/g' index.html
sudo sed -i 's/#28a745/#ffc107/g' index.html

# 3. Test blog update
curl http://localhost:8081 | grep -i version

# 4. Navigate ke shop site
cd /var/www/shop/html
pwd

# 5. Update shop to version 2.5
sudo sed -i 's/v1.0/v2.5/g' index.html
sudo sed -i 's/Version: <span class="version">1.0/Version: <span class="version">2.5/g' index.html
sudo sed -i 's/#007bff/#dc3545/g' index.html

# 6. Test shop update
curl http://localhost:8082 | grep -i version

# 7. Navigate between sites using cd -
cd /var/www/blog/html
pwd

cd /var/www/shop/html
pwd

cd -
pwd

cd -
pwd
```

### Exercise 3: Navigation Challenge

**Task:** Practice navigating between different website directories

```bash
# 1. Start dari home
cd ~
pwd

# 2. Pergi ke blog html
cd /var/www/blog/html
pwd

# 3. List files
ls -la

# 4. Pergi ke nginx config
cd /etc/nginx/conf.d
pwd

# 5. List blog config
cat blog.conf

# 6. Quick toggle balik ke blog html
cd -
pwd

# 7. Pergi ke shop html (using relative path from blog)
cd ../../shop/html
pwd

# 8. List files
ls -la

# 9. Pergi ke nginx logs
cd /var/log/nginx
pwd

# 10. View latest access log entries
sudo tail -20 access.log | grep -E '8081|8082'

# 11. Toggle between logs and shop html
cd -
pwd

cd -
pwd
```

### Exercise 4: Directory Comparison

```bash
# Compare both sites side by side
echo "=== BLOG SITE ==="
ls -lh /var/www/blog/html/
echo ""

echo "=== SHOP SITE ==="
ls -lh /var/www/shop/html/
echo ""

echo "=== NGINX CONFIGS ==="
ls -l /etc/nginx/conf.d/*.conf
echo ""

echo "=== TEST BOTH SITES ==="
echo "Blog Version:"
curl -s http://localhost:8081 | grep -i "version"
echo ""
echo "Shop Version:"
curl -s http://localhost:8082 | grep -i "version"
```

**✅ Success:** Anda telah:

- Deploy multiple websites
- Update versions independently
- Navigate efficiently between directories
- Verify changes in real-time
- Manage configurations per site

---

## Bahagian 10: Cleanup (Optional)

```bash
# Stop sites (optional)
sudo rm /etc/nginx/conf.d/blog.conf
sudo rm /etc/nginx/conf.d/shop.conf
sudo nginx -t
sudo systemctl reload nginx

# Remove site directories (optional)
sudo rm -rf /var/www/blog
sudo rm -rf /var/www/shop
```

---

---

## 🎓 Kesimpulan

Dalam lab ini anda telah belajar:

✅ Struktur direktori Linux untuk DevOps  
✅ Navigate nginx directories (/etc/nginx, /var/log/nginx, /usr/share/nginx/html)  
✅ Command `pwd` untuk check current location  
✅ Command `ls` dengan pelbagai options untuk monitoring  
✅ Command `cd` untuk navigation efficiency  
✅ Command `mkdir` untuk buat web directories  
✅ Command `rmdir` untuk remove empty directories  
✅ Absolute vs relative paths dalam web deployment  
✅ Tab completion & shortcuts untuk productivity  
✅ Wildcards untuk pattern matching logs dan configs  
✅ **Deploy real nginx websites dengan HTML**  
✅ **Update websites dan verify changes instantly**  
✅ **Manage multiple sites dengan different versions**  

### Command Summary

| Command | Fungsi | Contoh DevOps |
|---------|--------|--------|
| `pwd` | Print working directory | `pwd` (check if in correct directory) |
| `ls` | List files | `ls -lh /var/log/nginx/` (check logs) |
| `ls -lt` | Sort by time | `ls -lt /var/log/nginx/` (find latest logs) |
| `cd` | Change directory | `cd /etc/nginx/conf.d` |
| `cd ~` | Go to home | `cd ~` |
| `cd ..` | Go to parent | `cd ..` (from /var/www/site1/html to /var/www/site1) |
| `cd -` | Go to previous | `cd -` (toggle between config and web root) |
| `mkdir` | Make directory | `sudo mkdir -p /var/www/mysite/html` |
| `mkdir -p` | Make nested directories | `mkdir -p site/{html,logs,ssl}` |
| `rmdir` | Remove empty directory | `rmdir old_site` |

### Real DevOps Scenarios Practiced

1. ✅ Deploy nginx website dengan custom HTML
2. ✅ Update website versions (1.0 → 2.0)  
3. ✅ Navigate between config, web root, dan logs efficiently
4. ✅ Manage multiple sites simultaneously
5. ✅ Verify deployments dengan curl
6. ✅ Use wildcards untuk find config dan log files

---

## ✅ Lab 20 Checklist

Pastikan anda boleh:

**Basic Navigation:**

- [ ] Tahu current position dengan `pwd`
- [ ] List files dengan `ls -l`, `ls -a`, `ls -lah`
- [ ] Navigate menggunakan absolute path
- [ ] Navigate menggunakan relative path
- [ ] Pergi ke home dengan `cd ~`
- [ ] Pergi ke parent dengan `cd ..`
- [ ] Toggle between directories dengan `cd -`
- [ ] Guna Tab completion
- [ ] Guna wildcards (`*`, `?`, `[]`)

**Directory Management:**

- [ ] Buat directory dengan `mkdir`
- [ ] Buat nested directories dengan `mkdir -p`
- [ ] Remove empty directory dengan `rmdir`
- [ ] Setup complex directory structure dalam satu command

**DevOps Practical:**

- [ ] Navigate ke nginx config directory (/etc/nginx)
- [ ] Navigate ke nginx web root (/usr/share/nginx/html)
- [ ] Navigate ke nginx logs (/var/log/nginx)
- [ ] Install dan configure nginx
- [ ] Create custom HTML files
- [ ] Deploy website dan verify dengan curl
- [ ] Update website version dan see changes
- [ ] Manage multiple sites dengan different ports
- [ ] Toggle efficiently between config, web root, dan logs
