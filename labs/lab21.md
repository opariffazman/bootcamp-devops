## LAB 21: Linux Command Asas - Pengurusan Fail & Viewing (Sesi 2)

### 🎯 Objektif
- Membuat dan mengurus fail (touch, cp, mv, rm)
- Melihat kandungan fail (cat, less, head, tail)
- Memahami file permissions dan ownership
- Mencari fail dan content (find, grep)

### ⏱️ Durasi
2 jam

### 📋 Prerequisites
- Selesai Lab 20 (Sesi 1)
- EC2 instance running dengan terminal access

---

## Bahagian 1: Command touch (10 minit)

### Langkah 1: Buat Single File

```bash
# Pergi ke home
cd ~

# Buat file kosong
touch myfile.txt

# Verify
ls -l myfile.txt
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 0 Oct 22 10:00 myfile.txt
```

### Langkah 2: Buat Multiple Files

```bash
# Buat multiple files sekaligus
touch file1.txt file2.txt file3.txt

# Verify
ls -l
```

### Langkah 3: Buat Files Dengan Pattern

```bash
# Buat files dengan numbering
touch report_{1..5}.txt

# Verify
ls report_*
```

**Expected output:**
```
report_1.txt  report_2.txt  report_3.txt  report_4.txt  report_5.txt
```

### Langkah 4: Update File Timestamp

```bash
# Buat file
touch oldfile.txt

# Tunggu beberapa saat, then update timestamp
touch oldfile.txt

# Check timestamp updated
ls -l oldfile.txt
```

### Langkah 5: Practical DevOps Usage

```bash
# Setup config files structure
cd ~
mkdir -p devops-training/configs
cd devops-training/configs

# Buat common config files
touch nginx.conf docker-compose.yml database.env app.properties

# Verify
ls -l
```

**✅ Success:** Anda faham command `touch` untuk create files

---

## Bahagian 2: Command cp (20 minit)

### Langkah 1: Copy Single File

```bash
# Setup
cd ~/devops-training/configs

# Create sample file dengan content
echo "server { listen 80; }" > nginx.conf

# Copy file
cp nginx.conf nginx.conf.backup

# Verify
ls -l
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 22 Oct 22 10:05 nginx.conf
-rw-r--r-- 1 ec2-user ec2-user 22 Oct 22 10:05 nginx.conf.backup
```

### Langkah 2: Copy File ke Directory Lain

```bash
# Buat backup directory
mkdir -p ~/devops-training/backups

# Copy file ke directory lain
cp nginx.conf ~/devops-training/backups/

# Verify
ls -l ~/devops-training/backups/
```

### Langkah 3: Copy Dengan Rename

```bash
# Copy dengan nama berbeza
cp docker-compose.yml ~/devops-training/backups/docker-compose.yml.old

# Verify
ls -l ~/devops-training/backups/
```

### Langkah 4: Copy Multiple Files

```bash
# Copy multiple files ke directory
cp nginx.conf docker-compose.yml database.env ~/devops-training/backups/

# Verify
ls -l ~/devops-training/backups/
```

### Langkah 5: Copy Directory (Recursive)

```bash
# Buat directory dengan files
mkdir -p ~/devops-training/production
touch ~/devops-training/production/{app1.conf,app2.conf,app3.conf}

# Copy directory (kena guna -r)
cp -r ~/devops-training/production ~/devops-training/production-backup

# Verify
ls -l ~/devops-training/
ls -l ~/devops-training/production-backup/
```

### Langkah 6: Copy Dengan Interactive Mode

```bash
# Copy dengan confirmation jika file exist
cp -i nginx.conf nginx.conf.backup
```

**Expected prompt:**
```
cp: overwrite 'nginx.conf.backup'?
```

Type `n` untuk cancel atau `y` untuk proceed.

### Langkah 7: DevOps Scenario - Backup Dengan Timestamp

```bash
cd ~/devops-training/configs

# Backup dengan timestamp
cp nginx.conf nginx.conf.$(date +%Y%m%d_%H%M%S)

# Verify
ls -l nginx.conf*
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 22 Oct 22 10:05 nginx.conf
-rw-r--r-- 1 ec2-user ec2-user 22 Oct 22 10:05 nginx.conf.20241022_100530
-rw-r--r-- 1 ec2-user ec2-user 22 Oct 22 10:05 nginx.conf.backup
```

**✅ Success:** Anda faham command `cp` untuk copy files dan directories

---

## Bahagian 3: Command mv (15 minit)

### Langkah 1: Rename File

```bash
cd ~/devops-training/configs

# Rename file
touch tempfile.txt
mv tempfile.txt production.txt

# Verify
ls -l production.txt
```

### Langkah 2: Move File ke Directory Lain

```bash
# Create archive directory
mkdir -p ~/devops-training/archive

# Move file
mv production.txt ~/devops-training/archive/

# Verify original location (should be gone)
ls -l production.txt

# Verify new location
ls -l ~/devops-training/archive/
```

**Expected error untuk original location:**
```
ls: cannot access 'production.txt': No such file or directory
```

### Langkah 3: Move Multiple Files

```bash
cd ~/devops-training/configs

# Create test files
touch old1.conf old2.conf old3.conf

# Move multiple files
mv old1.conf old2.conf old3.conf ~/devops-training/archive/

# Verify
ls -l ~/devops-training/archive/
```

### Langkah 4: Move Dengan Interactive Mode

```bash
cd ~/devops-training/configs

# Create duplicate
touch test.txt
echo "new content" > test.txt
cp test.txt ~/devops-training/archive/

# Move dengan confirmation
mv -i test.txt ~/devops-training/archive/
```

**Expected prompt:**
```
mv: overwrite '/home/ec2-user/devops-training/archive/test.txt'?
```

### Langkah 5: Rename Directory

```bash
cd ~/devops-training

# Rename directory
mv archive old-files

# Verify
ls -l
```

### Langkah 6: DevOps Pattern - Log Rotation

```bash
cd ~/devops-training
mkdir -p logs

# Create sample log
echo "Log entry 1" > logs/app.log

# Rotate log with timestamp
mv logs/app.log logs/app.log.$(date +%Y%m%d)

# Create new log file
touch logs/app.log

# Verify
ls -l logs/
```

**✅ Success:** Anda faham command `mv` untuk move dan rename

---

## Bahagian 4: Command rm (15 minit)

⚠️ **PENTING:** Command `rm` adalah PERMANENT. Tiada recycle bin dalam Linux!

### Langkah 1: Remove Single File

```bash
cd ~/devops-training/configs

# Create test file
touch deleteme.txt

# Remove file
rm deleteme.txt

# Verify (should be gone)
ls deleteme.txt
```

**Expected error:**
```
ls: cannot access 'deleteme.txt': No such file or directory
```

### Langkah 2: Remove Dengan Interactive Mode (SAFE)

```bash
# Create test file
touch important.txt

# Remove dengan confirmation
rm -i important.txt
```

**Expected prompt:**
```
rm: remove regular empty file 'important.txt'?
```

Type `y` untuk delete atau `n` untuk cancel.

### Langkah 3: Remove Multiple Files

```bash
# Create test files
touch temp1.txt temp2.txt temp3.txt

# Remove multiple files
rm temp1.txt temp2.txt temp3.txt

# Verify
ls temp*.txt
```

**Expected error:**
```
ls: cannot access 'temp*.txt': No such file or directory
```

### Langkah 4: Remove Dengan Wildcard

```bash
# Create test files
touch test_{1..5}.log

# List first to verify
ls test_*.log

# Remove using wildcard
rm test_*.log

# Verify
ls test_*.log
```

### Langkah 5: Remove Directory (Recursive)

```bash
cd ~/devops-training

# Create directory dengan files
mkdir -p test-dir
touch test-dir/{file1,file2,file3}.txt

# Try remove tanpa -r (will FAIL)
rm test-dir
```

**Expected error:**
```
rm: cannot remove 'test-dir': Is a directory
```

```bash
# Remove dengan -r (recursive)
rm -r test-dir

# Verify
ls test-dir
```

**Expected error:**
```
ls: cannot access 'test-dir': No such file or directory
```

### Langkah 6: Force Remove (DANGEROUS!)

⚠️ **DANGER:** `rm -rf` adalah sangat berbahaya. Guna dengan berhati-hati!

```bash
# Create test structure
mkdir -p dangerous-test/sub1/sub2
touch dangerous-test/sub1/{a,b,c}.txt
touch dangerous-test/sub1/sub2/{x,y,z}.txt

# Force remove without confirmation
rm -rf dangerous-test

# Verify
ls dangerous-test
```

### Langkah 7: Safe Practice - Verify Before Delete

```bash
# Create test files
touch ~/devops-training/logs/old_{1..5}.log

# ALWAYS list first
ls ~/devops-training/logs/old_*.log

# Verify output looks correct, THEN delete
rm ~/devops-training/logs/old_*.log
```

**✅ Success:** Anda faham command `rm` dan bahaya-bahayanya

---

## Bahagian 5: Command cat (15 minit)

### Langkah 1: Display File Content

```bash
cd ~/devops-training

# Create sample file
cat > servers.txt
web1.example.com
web2.example.com
db1.example.com
```

Tekan `Ctrl+D` untuk save.

```bash
# Display content
cat servers.txt
```

**Expected output:**
```
web1.example.com
web2.example.com
db1.example.com
```

### Langkah 2: Display Dengan Line Numbers

```bash
# Display dengan line numbers
cat -n servers.txt
```

**Expected output:**
```
     1	web1.example.com
     2	web2.example.com
     3	db1.example.com
```

### Langkah 3: Concatenate Multiple Files

```bash
# Create multiple files
echo "Server Group A" > group-a.txt
echo "web1.example.com" >> group-a.txt

echo "Server Group B" > group-b.txt
echo "web2.example.com" >> group-b.txt

# Concatenate dan display
cat group-a.txt group-b.txt
```

**Expected output:**
```
Server Group A
web1.example.com
Server Group B
web2.example.com
```

### Langkah 4: Append to File

```bash
# Append content ke existing file
cat >> servers.txt
cache1.example.com
cache2.example.com
```

Tekan `Ctrl+D` untuk save.

```bash
# Verify
cat servers.txt
```

**Expected output:**
```
web1.example.com
web2.example.com
db1.example.com
cache1.example.com
cache2.example.com
```

### Langkah 5: Create Multi-Line File

```bash
# Create config file
cat > app.conf << EOF
[database]
host=localhost
port=5432
name=mydb

[cache]
host=redis
port=6379
EOF

# View content
cat app.conf
```

**✅ Success:** Anda faham command `cat` untuk view dan create files

---

## Bahagian 6: Command less, head & tail (20 minit)

### Langkah 1: View File Dengan less

```bash
# Create large file
cd ~/devops-training
seq 1 100 > numbers.txt

# View dengan less
less numbers.txt
```

**Shortcuts dalam less:**
- `Space` - next page
- `b` - previous page
- `/pattern` - search forward
- `n` - next match
- `q` - quit

Try search: Type `/50` then press Enter, then press `n` untuk next match.

Press `q` untuk keluar.

### Langkah 2: View First Lines Dengan head

```bash
# View first 10 lines (default)
head numbers.txt
```

**Expected output:**
```
1
2
3
4
5
6
7
8
9
10
```

```bash
# View first 5 lines
head -n 5 numbers.txt
```

**Expected output:**
```
1
2
3
4
5
```

### Langkah 3: View Last Lines Dengan tail

```bash
# View last 10 lines (default)
tail numbers.txt
```

**Expected output:**
```
91
92
93
94
95
96
97
98
99
100
```

```bash
# View last 5 lines
tail -n 5 numbers.txt
```

**Expected output:**
```
96
97
98
99
100
```

### Langkah 4: Follow File (Real-Time Monitoring)

⭐ **INI SANGAT PENTING UNTUK DEVOPS!**

```bash
# Terminal 1: Create log file dan follow
cd ~/devops-training/logs
touch app.log
tail -f app.log
```

Buka terminal/tab baru:

```bash
# Terminal 2: Add entries to log
cd ~/devops-training/logs
echo "$(date) - Application started" >> app.log
echo "$(date) - User logged in" >> app.log
echo "$(date) - Database connected" >> app.log
```

Pergi balik Terminal 1 - you should see entries appearing in real-time!

Press `Ctrl+C` untuk stop following.

### Langkah 5: DevOps Scenario - Monitor System Logs

```bash
# Monitor system log (if accessible)
sudo tail -f /var/log/syslog

# Or messages log
sudo tail -f /var/log/messages
```

Press `Ctrl+C` untuk stop.

**✅ Success:** Anda faham commands untuk view file contents

---

## Bahagian 7: File Permissions (25 minit)

### Langkah 1: Understanding Permission Format

```bash
cd ~/devops-training
touch sample.txt
ls -l sample.txt
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 0 Oct 22 11:00 sample.txt
│││││││││
│││││││└─ Other: read
││││││└── Group: read
│││││└─── User: read + write
││││└──── Group name
│││└───── Owner name
││└────── File type (- = file)
└─────── Permission bits
```

**Permission values:**
```
r (read)    = 4
w (write)   = 2
x (execute) = 1
```

**Common combinations:**
```
7 = rwx (4+2+1)
6 = rw- (4+2)
5 = r-x (4+1)
4 = r-- (4)
0 = --- (0)
```

### Langkah 2: Change Permissions - Script Executable

```bash
cd ~/devops-training

# Create script file
cat > deploy.sh << 'EOF'
#!/bin/bash
echo "Deploying application..."
EOF

# Try run (will FAIL)
./deploy.sh
```

**Expected error:**
```
bash: ./deploy.sh: Permission denied
```

```bash
# Check permissions
ls -l deploy.sh
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 43 Oct 22 11:05 deploy.sh
```

```bash
# Make executable
chmod +x deploy.sh

# Check permissions now
ls -l deploy.sh
```

**Expected output:**
```
-rwxr-xr-x 1 ec2-user ec2-user 43 Oct 22 11:05 deploy.sh
```

```bash
# Now run successfully
./deploy.sh
```

**Expected output:**
```
Deploying application...
```

### Langkah 3: Numeric Permissions

```bash
# Create test files
touch file644.txt file755.txt file600.txt

# Set different permissions
chmod 644 file644.txt    # rw-r--r--
chmod 755 file755.txt    # rwxr-xr-x
chmod 600 file600.txt    # rw-------

# Verify
ls -l file*.txt
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 0 Oct 22 11:10 file644.txt
-rw------- 1 ec2-user ec2-user 0 Oct 22 11:10 file600.txt
-rwxr-xr-x 1 ec2-user ec2-user 0 Oct 22 11:10 file755.txt
```

### Langkah 4: Symbolic Permissions

```bash
# Create test file
touch test.txt
ls -l test.txt

# Add execute untuk user
chmod u+x test.txt
ls -l test.txt

# Remove write dari group
chmod g-w test.txt
ls -l test.txt

# Add read untuk all
chmod a+r test.txt
ls -l test.txt
```

### Langkah 5: DevOps Scenario - Secure Credentials

```bash
cd ~/devops-training

# Create credentials file
cat > credentials.txt << EOF
DB_PASSWORD=secret123
API_KEY=sk-abc123xyz
EOF

# Secure it (owner read/write only)
chmod 600 credentials.txt

# Verify
ls -l credentials.txt
```

**Expected output:**
```
-rw------- 1 ec2-user ec2-user 47 Oct 22 11:15 credentials.txt
```

### Langkah 6: Recursive Permissions

```bash
# Create directory structure
mkdir -p ~/devops-training/scripts
touch ~/devops-training/scripts/{deploy,backup,monitor}.sh

# Make all scripts executable
chmod -R 755 ~/devops-training/scripts

# Verify
ls -l ~/devops-training/scripts/
```

**Expected output:**
```
-rwxr-xr-x 1 ec2-user ec2-user 0 Oct 22 11:20 backup.sh
-rwxr-xr-x 1 ec2-user ec2-user 0 Oct 22 11:20 deploy.sh
-rwxr-xr-x 1 ec2-user ec2-user 0 Oct 22 11:20 monitor.sh
```

**✅ Success:** Anda faham file permissions

---

## Bahagian 8: Command find (20 minit)

### Langkah 1: Find by Name

```bash
# Setup test files
cd ~
mkdir -p findtest/{logs,configs,scripts}
touch findtest/logs/{app,error,access}.log
touch findtest/configs/{nginx,app}.conf
touch findtest/scripts/{deploy,backup}.sh

# Find files by name
find findtest -name "app.log"
```

**Expected output:**
```
findtest/logs/app.log
```

### Langkah 2: Find With Wildcard

```bash
# Find all .log files
find findtest -name "*.log"
```

**Expected output:**
```
findtest/logs/access.log
findtest/logs/app.log
findtest/logs/error.log
```

### Langkah 3: Find by Type

```bash
# Find files only
find findtest -type f -name "*.conf"
```

**Expected output:**
```
findtest/configs/app.conf
findtest/configs/nginx.conf
```

```bash
# Find directories only
find findtest -type d
```

**Expected output:**
```
findtest
findtest/configs
findtest/logs
findtest/scripts
```

### Langkah 4: Find by Size

```bash
# Create files with different sizes
echo "small" > findtest/small.txt
dd if=/dev/zero of=findtest/large.txt bs=1M count=5

# Find files larger than 1MB
find findtest -type f -size +1M
```

**Expected output:**
```
findtest/large.txt
```

### Langkah 5: Find by Modification Time

```bash
# Find files modified in last 5 minutes
find findtest -type f -mmin -5
```

```bash
# Create old file (simulate)
touch -t 202401010000 findtest/oldfile.txt

# Find files modified more than 100 days ago
find findtest -type f -mtime +100
```

### Langkah 6: Find and Execute Command

```bash
# Find and list with details
find findtest -name "*.log" -exec ls -lh {} \;
```

```bash
# Find and delete (BE CAREFUL!)
# Create test files first
touch findtest/temp_{1..3}.tmp

# List first
find findtest -name "*.tmp"

# Then delete
find findtest -name "*.tmp" -delete

# Verify deleted
find findtest -name "*.tmp"
```

**✅ Success:** Anda faham command `find`

---

## Bahagian 9: Command grep (15 minit)

### Langkah 1: Search in Single File

```bash
cd ~/devops-training

# Create sample log
cat > application.log << EOF
2024-10-22 10:00:00 INFO Application started
2024-10-22 10:01:00 ERROR Database connection failed
2024-10-22 10:02:00 INFO Retrying connection
2024-10-22 10:03:00 ERROR Connection timeout
2024-10-22 10:04:00 INFO Database connected
EOF

# Search for "ERROR"
grep "ERROR" application.log
```

**Expected output:**
```
2024-10-22 10:01:00 ERROR Database connection failed
2024-10-22 10:03:00 ERROR Connection timeout
```

### Langkah 2: Case-Insensitive Search

```bash
# Search case-insensitive
grep -i "error" application.log
```

### Langkah 3: Show Line Numbers

```bash
# Search dengan line numbers
grep -n "ERROR" application.log
```

**Expected output:**
```
2:2024-10-22 10:01:00 ERROR Database connection failed
4:2024-10-22 10:03:00 ERROR Connection timeout
```

### Langkah 4: Count Matches

```bash
# Count how many errors
grep -c "ERROR" application.log
```

**Expected output:**
```
2
```

### Langkah 5: Invert Match

```bash
# Show lines WITHOUT "INFO"
grep -v "INFO" application.log
```

**Expected output:**
```
2024-10-22 10:01:00 ERROR Database connection failed
2024-10-22 10:03:00 ERROR Connection timeout
```

### Langkah 6: Search in Multiple Files

```bash
# Create more logs
echo "ERROR: Failed to start" > error1.log
echo "ERROR: Port in use" > error2.log

# Search in all .log files
grep "ERROR" *.log
```

### Langkah 7: Recursive Search

```bash
# Search in all files in directory
grep -r "ERROR" ~/devops-training/
```

### Langkah 8: Search With Context

```bash
# Show 2 lines AFTER match
grep -A 2 "Database connection failed" application.log

# Show 2 lines BEFORE match
grep -B 2 "Database connected" application.log

# Show 2 lines AROUND match
grep -C 2 "Retrying" application.log
```

**✅ Success:** Anda faham command `grep`

---

## Bahagian 10: Hands-On Exercise (20 minit)

### Exercise 1: Complete File Management

```bash
# 1. Setup
cd ~
mkdir -p exercise/production exercise/backup

# 2. Create application configs
cat > exercise/production/app.conf << EOF
port=8080
env=production
debug=false
EOF

cat > exercise/production/db.conf << EOF
host=localhost
port=5432
database=myapp
EOF

# 3. Backup configs dengan timestamp
cd exercise/production
cp app.conf ../backup/app.conf.$(date +%Y%m%d)
cp db.conf ../backup/db.conf.$(date +%Y%m%d)

# 4. Verify backup
ls -l ../backup/

# 5. Modify production config
echo "max_connections=100" >> db.conf

# 6. View differences
cat db.conf
cat ../backup/db.conf.*

# 7. Secure credential file
cat > credentials.env << EOF
DB_PASSWORD=secret123
API_KEY=abc123xyz
EOF

chmod 600 credentials.env

# 8. Verify permissions
ls -l credentials.env

# 9. Find all .conf files
find ~/exercise -name "*.conf"

# 10. Search for "port" in all files
grep -r "port" ~/exercise/
```

### Exercise 2: Log Analysis

```bash
# 1. Create sample log file
cd ~/devops-training
cat > access.log << EOF
192.168.1.100 - GET /api/users 200
192.168.1.101 - POST /api/login 200
192.168.1.100 - GET /api/products 200
192.168.1.102 - GET /api/users 404
192.168.1.100 - GET /api/orders 500
192.168.1.103 - POST /api/login 401
192.168.1.100 - GET /api/users 200
EOF

# 2. Count total requests
wc -l access.log

# 3. Find all errors (4xx, 5xx)
grep -E "(4[0-9]{2}|5[0-9]{2})" access.log

# 4. Count errors
grep -cE "(4[0-9]{2}|5[0-9]{2})" access.log

# 5. Find requests from specific IP
grep "192.168.1.100" access.log

# 6. Count 200 responses
grep -c " 200" access.log
```

**✅ Success:** Tahniah! Anda telah selesai Lab 21

---

## 🎓 Kesimpulan

Dalam lab ini anda telah belajar:

✅ File creation: `touch`  
✅ File operations: `cp`, `mv`, `rm`  
✅ Viewing files: `cat`, `less`, `head`, `tail`  
✅ File permissions: `chmod`  
✅ Finding files: `find`  
✅ Searching content: `grep`  
✅ Real-world DevOps scenarios  

### Command Summary

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `touch` | Create empty file | `touch file.txt` |
| `cp` | Copy files/dirs | `cp -r src/ dest/` |
| `mv` | Move/rename | `mv old.txt new.txt` |
| `rm` | Remove files/dirs | `rm -rf directory/` |
| `cat` | Display content | `cat file.txt` |
| `less` | View large files | `less large.log` |
| `head` | View start | `head -n 20 file.txt` |
| `tail` | View end | `tail -f app.log` |
| `chmod` | Change permissions | `chmod 755 script.sh` |
| `find` | Find files | `find / -name "*.log"` |
| `grep` | Search in files | `grep "error" *.log` |

---

## ✅ Lab 21 Checklist

Pastikan anda boleh:

- [ ] Create files dengan `touch`
- [ ] Copy files dan directories dengan `cp`
- [ ] Move dan rename dengan `mv`
- [ ] Remove files dengan `rm` (safely!)
- [ ] View file content dengan `cat`, `less`, `head`, `tail`
- [ ] Monitor logs real-time dengan `tail -f`
- [ ] Understand file permissions format
- [ ] Change permissions dengan `chmod`
- [ ] Find files by name, size, time dengan `find`
- [ ] Search content dalam files dengan `grep`
- [ ] Combine commands untuk complex tasks
