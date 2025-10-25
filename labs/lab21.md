## LAB 21: Linux Command Intermediate - Manipulasi Fail & Pencarian (Sesi 2)

### Prerequisites

- Selesai [Lab 20](lab20.md) - Linux Command Asas
- EC2 instance Ubuntu running di public subnet dengan Public IP
- IAM Instance Profile dengan SSM permissions
- Access ke terminal menggunakan Session Manager (SSM)
- Nginx installed dan running

---

## Bahagian 0: Connect ke EC2 Instance

### Langkah 1: Connect Menggunakan Session Manager

1. Pergi ke EC2 Console
2. Select instance anda
3. Click **Connect**
4. Pilih tab **Session Manager**
5. Click **Connect**

### Langkah 2: Switch ke Bash Shell & Setup Working Directory

```bash
# Switch to bash
bash

# Navigate to home directory
cd ~

# Verify location
pwd
```

---

## Bahagian 1: Pengenalan Command Asas untuk Manipulasi Content

Sebelum kita start dengan file operations, mari kita fahami beberapa commands yang akan kita guna sepanjang lab ini.

### Command echo (Print Text)

`echo` adalah command untuk print text ke screen atau write ke file.

**Syntax:** `echo "text"`

```bash
# Print text ke screen
echo "Hello World"

# Print variable
echo "Current user: $USER"

# Print tanpa newline
echo -n "No newline"

# Print dengan special characters
echo -e "Line 1\nLine 2\nLine 3"
```

**Analogi:** Seperti function `print()` dalam programming, atau display message di screen.

### Operator > (Redirect Output - Overwrite)

Operator `>` digunakan untuk redirect output dari command ke file. File akan di-**overwrite** (replaced).

**Syntax:** `command > filename`

```bash
# Write text ke file (create atau overwrite)
echo "This is line 1" > test.txt

# View file
cat test.txt

# Overwrite dengan text baru (original content hilang!)
echo "This is new line" > test.txt
cat test.txt
```

**⚠️ AMARAN:** `>` akan **replace semua content** dalam file!

### Operator >> (Redirect Output - Append)

Operator `>>` digunakan untuk redirect output ke file dengan **append** (tambah di belakang).

**Syntax:** `command >> filename`

```bash
# Create file
echo "Line 1" > test.txt

# Append line 2 (tidak delete line 1)
echo "Line 2" >> test.txt

# Append line 3
echo "Line 3" >> test.txt

# View all lines
cat test.txt
```

**Perbezaan penting:**

```bash
# Overwrite (>)
echo "First" > file.txt    # file.txt: "First"
echo "Second" > file.txt   # file.txt: "Second" (First hilang!)

# Append (>>)
echo "First" > file.txt    # file.txt: "First"
echo "Second" >> file.txt  # file.txt: "First\nSecond" (First masih ada!)
```

### Command cat (Concatenate and Display)

`cat` digunakan untuk display file content atau combine multiple files.

**Syntax:** `cat filename`

```bash
# View single file
cat test.txt

# View multiple files
echo "File 1 content" > file1.txt
echo "File 2 content" > file2.txt
cat file1.txt file2.txt

# Create file dengan cat (press Ctrl+D untuk finish)
cat > newfile.txt
Type your content here
Press Ctrl+D to save

# View dengan line numbers
cat -n test.txt
```

**Use cases:**
- View file content
- Combine multiple files
- Create simple files

### Command tee (Read from Input and Write to File)

`tee` membaca dari input, kemudian **menulis ke file DAN print ke screen** sekaligus.

**Syntax:** `command | tee filename`

```bash
# Write dan display sekaligus
echo "Important message" | tee important.txt

# Append mode
echo "Second message" | tee -a important.txt

# Useful untuk logging command output
ls -l | tee directory-list.txt

# Dengan sudo (common untuk system files)
echo "Some config" | sudo tee /etc/some-config.conf
```

**Kenapa guna tee?**

```bash
# Without tee - can't see what's written
echo "content" > file.txt
# (nothing shown on screen)

# With tee - can see what's written
echo "content" | tee file.txt
# content (shown on screen AND written to file)
```

**Practical DevOps example:**

```bash
# Save command output dan view sekaligus
sudo systemctl status nginx | tee nginx-status.txt

# Now you can:
# 1. See status on screen
# 2. Have it saved in file for later
```

### Cat vs Tee - Bila Guna?

| Command | Function | Display to Screen? | Write to File? |
|---------|----------|-------------------|----------------|
| `cat file.txt` | Read file | ✅ Yes | ❌ No |
| `echo "text" > file.txt` | Write to file | ❌ No | ✅ Yes (overwrite) |
| `echo "text" >> file.txt` | Append to file | ❌ No | ✅ Yes (append) |
| `echo "text" \| tee file.txt` | Write & display | ✅ Yes | ✅ Yes (overwrite) |
| `echo "text" \| tee -a file.txt` | Append & display | ✅ Yes | ✅ Yes (append) |

### Langkah 1: Practice - echo dan Redirection

```bash
cd ~
mkdir -p lab21-basics
cd lab21-basics

# Test echo
echo "Learning Linux commands"

# Test > (overwrite)
echo "Version 1.0" > version.txt
cat version.txt

echo "Version 2.0" > version.txt
cat version.txt
# Notice: Version 1.0 dah gone!

# Test >> (append)
echo "First log entry" > app.log
echo "Second log entry" >> app.log
echo "Third log entry" >> app.log
cat app.log
# Notice: Semua entries ada!
```

### Langkah 2: Practice - cat

```bash
# View single file
cat version.txt

# View multiple files
cat version.txt app.log

# View dengan line numbers
cat -n app.log

# Create multi-line file
cat > multiline.txt << 'EOF'
This is line 1
This is line 2
This is line 3
EOF

cat multiline.txt
```

**Note:** `<< 'EOF'` adalah "here document" - cara mudah untuk create multi-line file. Type content, kemudian type `EOF` untuk finish.

### Langkah 3: Practice - tee

```bash
# Write dan display
echo "Server: nginx" | tee server-info.txt

# Append dengan -a
echo "Port: 80" | tee -a server-info.txt
echo "Status: running" | tee -a server-info.txt

# View result
cat server-info.txt

# Practical: Save command output
ls -lh | tee file-list.txt
# You see the output AND it's saved
```

### Langkah 4: Understanding sudo tee

When you need to write to system files (owned by root), you **cannot** use `>` with sudo:

```bash
# This does NOT work:
sudo echo "test" > /etc/some-file
# Error: Permission denied

# This WORKS:
echo "test" | sudo tee /etc/some-file

# Why? Because:
# - echo runs as normal user
# - tee runs with sudo privileges
# - tee writes to protected file
```

**Practical example:**

```bash
# Write to nginx config (protected file)
echo "# Custom config" | sudo tee -a /etc/nginx/nginx.conf

# Verify
sudo tail -n 1 /etc/nginx/nginx.conf
```

### Langkah 5: Combine Everything

```bash
# Create log file dengan multiple entries
echo "=== Application Log ===" > combined.log
echo "Date: $(date)" >> combined.log
echo "User: $USER" >> combined.log
echo "Host: $(hostname)" >> combined.log

# View dengan cat
cat combined.log

# Append more dengan tee (so we can see what we add)
echo "Status: Active" | tee -a combined.log

# View final result
cat -n combined.log
```

---

## Bahagian 2: Setup Test Environment

### Langkah 1: Buat Struktur Direktori untuk Practice

```bash
# Buat working directory
mkdir -p ~/lab21-practice
cd ~/lab21-practice

# Buat struktur untuk practice
mkdir -p {projects,backups,logs,configs}
mkdir -p projects/{website,api,mobile}
mkdir -p logs/{nginx,app,system}

# Verify struktur
ls -R
```

### Langkah 2: Buat Sample Files untuk Practice

Sekarang kita akan guna `echo` dan redirection operators yang baru kita belajar:

```bash
# Buat config files (empty files)
touch configs/nginx.conf
touch configs/app.conf
touch configs/database.conf

# Buat log files dengan content (guna echo >)
echo "2024-10-25 10:00:01 INFO Server started" > logs/app/app.log

# Tambah more entries (guna echo >>)
echo "2024-10-25 10:00:02 ERROR Connection failed" >> logs/app/app.log
echo "2024-10-25 10:00:03 INFO Request processed" >> logs/app/app.log

# View what we created (guna cat)
cat logs/app/app.log

# Buat sample web files
echo "<html><body><h1>Homepage</h1></body></html>" > projects/website/index.html
echo "<html><body><h1>About Us</h1></body></html>" > projects/website/about.html

# Verify files created
ls -lh configs/
ls -lh logs/app/
ls -lh projects/website/

# View web files
cat projects/website/index.html
```

**Perhatikan:**
- `touch` create empty file
- `echo >` create file dengan content (overwrite kalau file exists)
- `echo >>` append content ke file
- `cat` untuk view file content

---

## Bahagian 3: Command cp (Copy)

**Reminder:** Sebelum start, pastikan anda faham:
- `cat` - untuk view file content
- `>` - untuk write/overwrite file
- `>>` - untuk append ke file
- `echo` - untuk print atau create content

### Langkah 1: Copy Single File

**Syntax:** `cp source destination`

```bash
# Copy config file
cp configs/nginx.conf configs/nginx.conf.backup

# Verify
ls -l configs/
```

**Analogi:** Seperti photocopy dokumen - original masih ada, dapat copy baru.

### Langkah 2: Copy Multiple Files ke Directory

```bash
# Copy multiple files sekaligus
cp configs/nginx.conf configs/app.conf backups/

# Verify
ls -l backups/
```

### Langkah 3: Copy File dengan Rename

```bash
# Copy dan tukar nama sekaligus
cp configs/database.conf configs/database.conf.prod

# Verify
ls -l configs/
```

### Langkah 4: Copy Directory (Recursive)

**PENTING:** Untuk copy directory, mesti guna `-r` (recursive)

```bash
# Cuba copy tanpa -r (akan GAGAL)
cp projects/website backups/

# Cara yang BETUL - guna -r
cp -r projects/website backups/

# Verify
ls -l backups/
ls -l backups/website/
```

### Langkah 5: Copy dengan Preserve Permissions & Timestamps

```bash
# Copy dengan preserve attributes
cp -p configs/nginx.conf configs/nginx.conf.preserved

# Compare timestamps
ls -l configs/nginx.conf*
```

### Langkah 6: Interactive Copy (Safety Mode)

```bash
# Buat file test
echo "original content" > test-original.txt
echo "existing content" > test-existing.txt

# Copy dengan -i (akan tanya sebelum overwrite)
cp -i test-original.txt test-existing.txt

# Type 'n' untuk cancel, atau 'y' untuk overwrite
```

### Langkah 7: Copy dengan Verbose Output

```bash
# Copy dengan -v (show apa yang di-copy)
cp -v configs/app.conf backups/app.conf.backup

# Copy directory dengan verbose
cp -rv projects/api backups/
```

### Langkah 8: Practical - Backup Nginx Configs

```bash
# Buat backup directory dengan timestamp
BACKUP_DATE=$(date +%Y%m%d)
mkdir -p ~/nginx-backups/$BACKUP_DATE

# Copy nginx configs
sudo cp -r /etc/nginx ~/nginx-backups/$BACKUP_DATE/

# Verify backup
ls -lh ~/nginx-backups/$BACKUP_DATE/
```

---

## Bahagian 4: Command mv (Move/Rename)

### Langkah 1: Rename File

**Syntax:** `mv old_name new_name`

```bash
cd ~/lab21-practice

# Rename file
mv test-original.txt test-renamed.txt

# Verify - old name dah gone
ls -l test-*
```

**Analogi:** Seperti tukar label pada fail - fail yang sama, nama baru.

### Langkah 2: Move File ke Directory Lain

```bash
# Move file ke directory lain
mv test-renamed.txt backups/

# Verify - file dah pindah
ls -l backups/
ls -l test-*
```

**Analogi:** Seperti pindah dokumen dari satu kabinet ke kabinet lain.

### Langkah 3: Move Multiple Files

```bash
# Buat test files
touch logs/test1.log logs/test2.log logs/test3.log

# Move multiple files
mv logs/test1.log logs/test2.log logs/test3.log logs/app/

# Verify
ls -l logs/app/
```

### Langkah 4: Move Directory

```bash
# Move entire directory
mv projects/mobile backups/

# Verify
ls -l projects/
ls -l backups/
```

### Langkah 5: Move dengan Rename Sekaligus

```bash
# Move dan rename sekaligus
mv backups/mobile backups/mobile-app-old

# Verify
ls -l backups/
```

### Langkah 6: Interactive Move (Safety Mode)

```bash
# Buat test files
echo "content 1" > file1.txt
echo "content 2" > file2.txt

# Move dengan -i (tanya sebelum overwrite)
mv -i file1.txt file2.txt
```

### Langkah 7: Practical - Organize Logs by Date

```bash
# Buat directory untuk organize logs
mkdir -p logs/archive/2024-10
mkdir -p logs/archive/2024-09

# Buat sample old logs
touch logs/nginx/access-2024-09.log
touch logs/nginx/error-2024-09.log

# Move old logs ke archive
mv logs/nginx/*2024-09* logs/archive/2024-09/

# Verify
ls -l logs/archive/2024-09/
ls -l logs/nginx/
```

### Langkah 8: Practical - Rename Config untuk Different Environment

```bash
cd configs/

# Copy dan rename untuk different environments
cp nginx.conf nginx.conf.dev
cp nginx.conf nginx.conf.staging
cp nginx.conf nginx.conf.prod

# Verify
ls -l
```

---

## Bahagian 5: Command rm (Remove)

> **⚠️ AMARAN PENTING:** 
> - `rm` adalah **PERMANENT** - file yang di-delete TIDAK boleh recover!
> - Tiada "Recycle Bin" dalam Linux
> - Sentiasa double-check sebelum delete
> - Best practice: Guna `-i` untuk confirmation

### Langkah 1: Remove Single File

**Syntax:** `rm filename`

```bash
cd ~/lab21-practice

# Buat test file
touch test-delete.txt

# Remove file
rm test-delete.txt

# Verify - file dah gone
ls -l test-*
```

### Langkah 2: Remove Multiple Files

```bash
# Buat multiple test files
touch delete1.txt delete2.txt delete3.txt

# Remove multiple files sekaligus
rm delete1.txt delete2.txt delete3.txt

# Verify
ls -l delete*
```

### Langkah 3: Remove dengan Interactive Mode (RECOMMENDED)

```bash
# Buat test file
touch important-file.txt

# Remove dengan -i (akan tanya confirmation)
rm -i important-file.txt

# Type 'y' untuk delete, 'n' untuk cancel
```

**Best Practice:** Sentiasa guna `-i` untuk safety!

### Langkah 4: Remove dengan Verbose

```bash
# Buat test files
touch file1.txt file2.txt

# Remove dengan -v (show apa yang di-delete)
rm -v file1.txt file2.txt
```

### Langkah 5: Remove Empty Directory

```bash
# Buat empty directory
mkdir test-empty-dir

# Remove empty directory (guna rmdir atau rm -d)
rmdir test-empty-dir

# Alternative
mkdir test-empty-dir2
rm -d test-empty-dir2
```

### Langkah 6: Remove Directory dengan Content (Recursive)

> **🔴 DANGER ZONE** - Extremely dangerous command!

```bash
# Buat test directory dengan files
mkdir -p test-dir/subdir
touch test-dir/file1.txt
touch test-dir/subdir/file2.txt

# Remove directory dan semua content (DANGEROUS!)
rm -r test-dir

# Verify
ls -l test-dir
```

### Langkah 7: Safe Remove Practice

```bash
# Best practice: Combine -r, -i, dan -v
mkdir -p danger-zone/data
touch danger-zone/data/important.txt

# Safe remove dengan confirmation
rm -riv danger-zone

# System akan tanya untuk setiap file
```

### Langkah 8: Remove dengan Pattern (Wildcards)

```bash
# Buat log files
touch logs/app-2024-01.log
touch logs/app-2024-02.log
touch logs/app-2024-03.log
touch logs/app.conf

# Remove hanya .log files
rm -v logs/*.log

# Verify - .conf file masih ada
ls -l logs/
```

### Langkah 9: ⚠️ The Most Dangerous Command

> **🔴 NEVER RUN THIS:** `sudo rm -rf /`
>
> - `-r` = recursive (all directories)
> - `-f` = force (no confirmation)
> - `/` = root directory (EVERYTHING)
> 
> **Result:** Delete ENTIRE system! No recovery!

**Safe Practice Tips:**

1. ✅ Sentiasa guna `-i` untuk confirmation
2. ✅ Test dengan `ls` dulu sebelum `rm`
3. ✅ Guna `-v` untuk verbose output
4. ✅ Double-check path sebelum delete
5. ❌ Jangan guna `-f` kecuali 100% sure
6. ❌ Jangan combine `-rf` tanpa careful check

### Langkah 10: Practical - Cleanup Old Logs

```bash
# Buat old log files
cd ~/lab21-practice
touch logs/archive/old-2023-01.log
touch logs/archive/old-2023-02.log
touch logs/archive/old-2023-03.log
touch logs/archive/keep-2024-10.log

# Safe way: Check dulu
ls -l logs/archive/*2023*

# Then remove dengan confirmation
rm -iv logs/archive/*2023*

# Verify - 2024 files masih ada
ls -l logs/archive/
```

---

## Bahagian 6: Command find (Pencarian Fail)

### Langkah 1: Find by Name

**Syntax:** `find [path] -name "pattern"`

```bash
cd ~/lab21-practice

# Find file dengan nama specific
find . -name "nginx.conf"

# Find dengan wildcard
find . -name "*.conf"

# Find case-insensitive
find . -iname "*.CONF"
```

**Nota:** `.` bermaksud current directory

### Langkah 2: Find by Type

```bash
# Find hanya directories
find . -type d

# Find hanya files
find . -type f

# Find directories dengan nama specific
find . -type d -name "logs"
```

### Langkah 3: Find by Size

```bash
# Buat test files dengan different sizes
cd ~/lab21-practice
dd if=/dev/zero of=large-file.dat bs=1M count=10
dd if=/dev/zero of=small-file.dat bs=1K count=1

# Find files larger than 5MB
find . -type f -size +5M

# Find files smaller than 1MB
find . -type f -size -1M

# Find files exactly 10MB
find . -type f -size 10M
```

**Size units:**
- `c` = bytes
- `k` = kilobytes
- `M` = megabytes
- `G` = gigabytes

### Langkah 4: Find by Modification Time

```bash
# Find files modified dalam last 24 hours
find . -type f -mtime -1

# Find files modified lebih dari 7 hari lalu
find . -type f -mtime +7

# Find files modified exactly 1 hari lalu
find . -type f -mtime 1
```

### Langkah 5: Find by Permissions

```bash
# Find files dengan permission 644
find . -type f -perm 644

# Find directories dengan permission 755
find . -type d -perm 755
```

### Langkah 6: Combine Multiple Criteria

```bash
# Find .conf files yang modified dalam last 7 days
find . -type f -name "*.conf" -mtime -7

# Find .log files larger than 1MB
find . -type f -name "*.log" -size +1M
```

### Langkah 7: Find dengan Actions

```bash
# Find dan list dengan details
find . -type f -name "*.conf" -ls

# Find dan execute command (delete)
find . -type f -name "*.dat" -exec rm -v {} \;

# Verify .dat files dah deleted
find . -name "*.dat"
```

**Nota:** `{}` adalah placeholder untuk file yang dijumpai

### Langkah 8: Find dengan Delete

```bash
# Buat test files
touch old1.tmp old2.tmp old3.tmp

# Find dan delete (DANGEROUS!)
find . -name "*.tmp" -delete

# Verify
find . -name "*.tmp"
```

### Langkah 9: Practical - Find Nginx Log Files

```bash
# Find all nginx log files dalam system
sudo find /var/log -name "*nginx*.log"

# Find nginx log files larger than 10MB
sudo find /var/log -name "*nginx*.log" -size +10M

# Find nginx access logs
sudo find /var/log -name "*access*.log"
```

### Langkah 10: Practical - Find Old Backup Files

```bash
# Find backup files older than 30 days
find ~/nginx-backups -type f -mtime +30

# Find dan delete old backups (dengan confirmation)
find ~/nginx-backups -type f -mtime +30 -exec ls -lh {} \;

# If comfortable, delete
# find ~/nginx-backups -type f -mtime +30 -delete
```

### Langkah 11: Practical - Find Empty Directories

```bash
# Buat empty directory
mkdir -p ~/lab21-practice/empty-dir

# Find empty directories
find ~/lab21-practice -type d -empty

# Find dan remove empty directories
find ~/lab21-practice -type d -empty -delete
```

---

## Bahagian 7: Command grep (Search File Content)

### Langkah 1: Basic Grep

**Syntax:** `grep "pattern" filename`

```bash
cd ~/lab21-practice

# Search untuk specific word dalam file
grep "ERROR" logs/app/app.log

# Search multiple files
grep "INFO" logs/app/*.log
```

**Analogi:** Seperti Ctrl+F atau Find dalam Word/Excel, tapi untuk terminal.

### Langkah 2: Case-Insensitive Search

```bash
# Search ignore case
grep -i "error" logs/app/app.log

# Compare dengan case-sensitive
grep "error" logs/app/app.log
```

### Langkah 3: Show Line Numbers

```bash
# Search dengan show line numbers
grep -n "ERROR" logs/app/app.log
```

### Langkah 4: Count Matches

```bash
# Count berapa kali "INFO" muncul
grep -c "INFO" logs/app/app.log

# Count dalam multiple files
grep -c "ERROR" logs/app/*.log
```

### Langkah 5: Invert Match

```bash
# Show lines yang TIDAK contain "INFO"
grep -v "INFO" logs/app/app.log
```

### Langkah 6: Recursive Search

```bash
# Search dalam all files dalam directory
grep -r "ERROR" logs/

# Recursive dengan show filename
grep -rn "ERROR" logs/
```

### Langkah 7: Show Context Lines

```bash
# Show 2 lines before match
grep -B 2 "ERROR" logs/app/app.log

# Show 2 lines after match
grep -A 2 "ERROR" logs/app/app.log

# Show 2 lines before AND after
grep -C 2 "ERROR" logs/app/app.log
```

### Langkah 8: Search dengan Multiple Patterns

```bash
# Search untuk "ERROR" OR "WARN"
grep -E "ERROR|WARN" logs/app/app.log

# Alternative
grep "ERROR\|WARN" logs/app/app.log
```

### Langkah 9: Search dengan Regular Expression

```bash
# Buat sample log dengan timestamps
cat > logs/app/test.log << 'EOF'
2024-10-25 10:00:01 INFO Server started
2024-10-25 10:00:02 ERROR Connection failed to 192.168.1.1
2024-10-25 10:00:03 ERROR Connection failed to 192.168.1.2
2024-10-25 10:00:04 INFO Request processed
EOF

# Search lines dengan IP address pattern
grep -E "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" logs/app/test.log

# Search lines start dengan "2024"
grep "^2024" logs/app/test.log

# Search lines end dengan "failed"
grep "failed$" logs/app/test.log
```

### Langkah 10: Practical - Search Nginx Logs

```bash
# Search untuk 404 errors dalam nginx access log
sudo grep "404" /var/log/nginx/access.log

# Search untuk specific IP dalam logs
sudo grep "192.168" /var/log/nginx/access.log

# Count 404 errors
sudo grep -c "404" /var/log/nginx/access.log
```

### Langkah 11: Practical - Search Nginx Config

```bash
# Search untuk "listen" directive dalam nginx configs
sudo grep -r "listen" /etc/nginx/

# Search dengan line numbers
sudo grep -rn "server_name" /etc/nginx/

# Search dan show filename only
sudo grep -rl "ssl" /etc/nginx/
```

### Langkah 12: Highlight Matches

```bash
# Search dengan color highlight
grep --color=auto "ERROR" logs/app/app.log

# Make it permanent (add to ~/.bashrc)
echo 'alias grep="grep --color=auto"' >> ~/.bashrc
```

---

## Bahagian 8: File Viewing Commands

### Langkah 1: Command cat (View Entire File)

```bash
cd ~/lab21-practice

# View file content
cat logs/app/app.log

# View multiple files
cat configs/nginx.conf configs/app.conf

# View dengan line numbers
cat -n logs/app/app.log
```

**Use case:** View small files

### Langkah 2: Command head (View First Lines)

```bash
# Buat file dengan banyak lines
seq 1 100 > numbers.txt

# View first 10 lines (default)
head numbers.txt

# View first 5 lines
head -n 5 numbers.txt

# View first 20 lines
head -n 20 numbers.txt
```

**Use case:** Check top of log file, check file structure

### Langkah 3: Command tail (View Last Lines)

```bash
# View last 10 lines (default)
tail numbers.txt

# View last 5 lines
tail -n 5 numbers.txt

# View last 20 lines
tail -n 20 numbers.txt
```

**Use case:** Check latest log entries

### Langkah 4: Command tail -f (Follow Log in Real-Time)

**PENTING untuk DevOps!** - Monitor logs dalam real-time

```bash
# Open 2 terminal windows menggunakan Session Manager

# Terminal 1: Monitor nginx access log
sudo tail -f /var/log/nginx/access.log

# Terminal 2: Generate traffic
curl http://localhost
curl http://localhost/about.html
curl http://localhost/notfound.html

# Terminal 1 akan show requests dalam real-time!
```

**Use case:** 
- Monitor application logs
- Debug real-time issues
- Watch deployment logs

**Tips:** Press `Ctrl+C` untuk stop tail -f

### Langkah 5: Command less (Interactive File Viewer)

```bash
# View large file dengan less
less numbers.txt
```

**less keyboard shortcuts:**
- `Space` = page down
- `b` = page up
- `g` = go to start
- `G` = go to end
- `/pattern` = search forward
- `?pattern` = search backward
- `n` = next match
- `N` = previous match
- `q` = quit

```bash
# Try it:
# 1. Open file
less numbers.txt

# 2. Press 'G' to go to end
# 3. Type '/50' to search for 50
# 4. Press 'n' to find next occurrence
# 5. Press 'q' to quit
```

### Langkah 6: Command more (Simple Pager)

```bash
# View dengan more
more numbers.txt
```

**more keyboard shortcuts:**
- `Space` = page down
- `Enter` = one line down
- `q` = quit

**Nota:** `less` is more powerful than `more`!

### Langkah 7: Practical - Monitor Nginx Logs

```bash
# View latest nginx access log entries
sudo tail -n 50 /var/log/nginx/access.log

# Follow nginx error log
sudo tail -f /var/log/nginx/error.log

# In another terminal, create error (request non-existent page)
curl http://localhost/thispagedoesnotexist

# You'll see the 404 in error log (if configured)
```

### Langkah 8: Practical - View Large Config File

```bash
# View nginx.conf dengan less
sudo less /etc/nginx/nginx.conf

# Try:
# 1. Press 'G' to go to bottom
# 2. Type '/http' to search for http block
# 3. Press 'q' to quit
```

---

## Bahagian 9: Command diff (Compare Files)

### Langkah 1: Basic Diff

```bash
cd ~/lab21-practice

# Buat 2 versions of config
cat > config-v1.txt << 'EOF'
server_name example.com
port 80
workers 2
EOF

cat > config-v2.txt << 'EOF'
server_name example.com
port 443
workers 4
ssl enabled
EOF

# Compare files
diff config-v1.txt config-v2.txt
```

**Output explanation:**
- `2c2` = line 2 changed to line 2
- `<` = line dari file pertama
- `>` = line dari file kedua
- `3a4` = after line 3, added line 4

### Langkah 2: Side-by-Side Comparison

```bash
# Compare side by side
diff -y config-v1.txt config-v2.txt
```

### Langkah 3: Unified Diff Format

```bash
# Git-style diff
diff -u config-v1.txt config-v2.txt
```

### Langkah 4: Ignore White Spaces

```bash
# Buat files dengan spacing differences
cat > file1.txt << 'EOF'
line 1
line 2
line 3
EOF

cat > file2.txt << 'EOF'
line 1
line 2  
line 3
EOF

# Normal diff (will show difference)
diff file1.txt file2.txt

# Ignore trailing white space
diff -Z file1.txt file2.txt
```

### Langkah 5: Practical - Compare Nginx Config Versions

```bash
# Backup current nginx config
sudo cp /etc/nginx/nginx.conf ~/nginx.conf.backup

# Make a change (just for demo)
sudo sed -i 's/worker_processes.*/worker_processes 4;/' /etc/nginx/nginx.conf

# Compare versions
diff ~/nginx.conf.backup /etc/nginx/nginx.conf

# Restore original (important!)
sudo cp ~/nginx.conf.backup /etc/nginx/nginx.conf
```

### Langkah 6: Practical - Compare Config Before Deployment

```bash
cd ~/lab21-practice/configs

# Create production config
cat > nginx.conf.prod << 'EOF'
worker_processes 4;
keepalive_timeout 65;
gzip on;
EOF

# Create staging config
cat > nginx.conf.staging << 'EOF'
worker_processes 2;
keepalive_timeout 65;
gzip on;
debug on;
EOF

# Compare before deploying
diff -y nginx.conf.prod nginx.conf.staging
```

---

## Bahagian 10: Command Chaining & Pipes

### Langkah 1: Pipe Operator |

**Konsep:** Output dari command pertama jadi input untuk command kedua

```bash
cd ~/lab21-practice

# List files dan count
ls -l | wc -l

# Show processes dan search
ps aux | grep nginx
```

### Langkah 2: Combine find + grep

```bash
# Find all .conf files dan search untuk "nginx"
find . -name "*.conf" -exec grep -l "nginx" {} \;

# Alternative using pipe
find . -name "*.conf" | xargs grep -l "nginx"
```

### Langkah 3: Combine grep + wc

```bash
# Count ERROR messages dalam log
grep "ERROR" logs/app/app.log | wc -l

# Count unique IPs dalam nginx log
sudo grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' /var/log/nginx/access.log | sort -u | wc -l
```

### Langkah 4: Combine Commands dengan && (AND)

```bash
# Run second command only jika first success
cd ~/lab21-practice && ls -l

# Practical: Backup dan verify
sudo cp /etc/nginx/nginx.conf ~/nginx-backup.conf && ls -lh ~/nginx-backup.conf
```

### Langkah 5: Combine Commands dengan || (OR)

```bash
# Run second command only jika first fails
cd /nonexistent || echo "Directory does not exist"

# Practical: Create directory if not exists
cd ~/backups || mkdir -p ~/backups
```

### Langkah 6: Chain Multiple Commands dengan ;

```bash
# Run commands in sequence (regardless of success/failure)
cd ~ ; ls -l ; pwd
```

### Langkah 7: Redirect Output >

```bash
# Save command output to file
ls -l > file-list.txt

# View the file
cat file-list.txt

# Overwrite vs Append
echo "First line" > output.txt
echo "Second line" >> output.txt
cat output.txt
```

### Langkah 8: Practical - Find Large Log Files

```bash
# Find log files larger than 10MB dan list details
sudo find /var/log -name "*.log" -size +10M -exec ls -lh {} \; 2>/dev/null

# Find dan save to file
sudo find /var/log -name "*.log" -size +10M > ~/large-logs.txt
```

### Langkah 9: Practical - Count Errors by Type

```bash
cd ~/lab21-practice

# Add more log entries
cat >> logs/app/app.log << 'EOF'
2024-10-25 11:00:01 ERROR Database connection timeout
2024-10-25 11:00:02 WARN High memory usage
2024-10-25 11:00:03 ERROR Database connection timeout
2024-10-25 11:00:04 ERROR API call failed
2024-10-25 11:00:05 WARN Disk space low
EOF

# Count each error type
grep ERROR logs/app/app.log | sort | uniq -c

# Get most common error
grep ERROR logs/app/app.log | sort | uniq -c | sort -rn | head -n 1
```

### Langkah 10: Practical - Monitor Top Nginx Visitors

```bash
# Get top 10 IPs dari nginx access log
sudo cat /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -n 10
```

### Langkah 11: Practical - Cleanup Old Files Safely

```bash
cd ~/lab21-practice

# Buat old files
touch -d "2023-01-01" old-file1.txt
touch -d "2023-01-01" old-file2.txt
touch -d "2024-10-25" new-file.txt

# Find old files, list them, dan save to file for review
find . -name "*.txt" -mtime +365 -exec ls -lh {} \; | tee files-to-delete.txt

# Review the list
cat files-to-delete.txt

# If comfortable, delete them
find . -name "*.txt" -mtime +365 -delete

# Verify new file masih ada
ls -l new-file.txt
```

---

## Bahagian 11: Praktis DevOps Real-World Scenarios

### Scenario 1: Daily Nginx Config Backup

```bash
#!/bin/bash
# Buat backup script

# Create backup directory with date
BACKUP_DIR=~/nginx-backups/$(date +%Y-%m-%d)
mkdir -p $BACKUP_DIR

# Backup nginx configs
sudo cp -r /etc/nginx $BACKUP_DIR/

# Verify backup
ls -lh $BACKUP_DIR/

# Find backups older than 30 days
find ~/nginx-backups -type d -mtime +30 -name "20*"

echo "Backup completed: $BACKUP_DIR"
```

**Run the script:**

```bash
cd ~
cat > backup-nginx.sh << 'EOF'
#!/bin/bash
BACKUP_DIR=~/nginx-backups/$(date +%Y-%m-%d)
mkdir -p $BACKUP_DIR
sudo cp -r /etc/nginx $BACKUP_DIR/
echo "Backup completed: $BACKUP_DIR"
EOF

# Make executable
chmod +x backup-nginx.sh

# Run it
./backup-nginx.sh
```

### Scenario 2: Monitor Nginx Error Rate

```bash
# Count errors dalam last 100 lines
sudo tail -n 100 /var/log/nginx/access.log | grep -c " 404 \| 500 \| 502 "

# Show error rate
TOTAL=$(sudo tail -n 100 /var/log/nginx/access.log | wc -l)
ERRORS=$(sudo tail -n 100 /var/log/nginx/access.log | grep -c " 404 \| 500 \| 502 ")
echo "Error rate: $ERRORS/$TOTAL requests"
```

### Scenario 3: Find and Archive Old Logs

```bash
cd ~/lab21-practice

# Create log archive directory
mkdir -p logs/archive/$(date +%Y-%m)

# Find logs older than 7 days dan move to archive
find logs/ -name "*.log" -mtime +7 -exec mv {} logs/archive/$(date +%Y-%m)/ \;

# Compress old archives (older than 30 days)
find logs/archive -type f -name "*.log" -mtime +30 -exec gzip {} \;

# Verify
ls -lh logs/archive/*/
```

### Scenario 4: Search for Specific Error in Logs

```bash
# Search untuk "connection timeout" dalam all logs
sudo find /var/log -name "*.log" -exec grep -l "connection timeout" {} \;

# Get details dengan line numbers
sudo find /var/log -name "*.log" -exec grep -Hn "connection timeout" {} \;

# Count occurrences
sudo find /var/log -name "*.log" -exec grep -h "connection timeout" {} \; | wc -l
```

### Scenario 5: Quick Health Check Script

```bash
cat > health-check.sh << 'EOF'
#!/bin/bash

echo "=== System Health Check ==="
echo ""

echo "1. Nginx Status:"
sudo systemctl status nginx | grep "Active:"
echo ""

echo "2. Recent Errors (last 10):"
sudo tail -n 10 /var/log/nginx/error.log
echo ""

echo "3. Disk Usage:"
df -h / | grep "/$"
echo ""

echo "4. Recent 404 Errors:"
sudo grep " 404 " /var/log/nginx/access.log | tail -n 5
echo ""

echo "Health check completed!"
EOF

chmod +x health-check.sh
./health-check.sh
```

### Scenario 6: Compare Deployment Configs

```bash
# Before deployment - compare staging vs production config
diff -y /etc/nginx/sites-available/staging /etc/nginx/sites-available/production

# After deployment - verify config changed
diff /etc/nginx/nginx.conf ~/nginx-backups/latest/nginx.conf
```

---

## Tips & Best Practices

### 1. Safety First

```bash
# ALWAYS use -i for destructive operations
rm -i file.txt          # ✅ Safe
mv -i old.txt new.txt   # ✅ Safe
cp -i src.txt dst.txt   # ✅ Safe

# NEVER use -f without thinking
rm -rf /                # 🔴 NEVER DO THIS!
```

### 2. Test Before Execute

```bash
# Test find command dengan ls dulu
find . -name "*.tmp" -exec ls {} \;

# Kalau result betul, then delete
find . -name "*.tmp" -delete
```

### 3. Use Verbose Mode

```bash
# Always use -v untuk see what happens
cp -v source.txt backup.txt
mv -v old.txt new.txt
rm -v unwanted.txt
```

### 4. Backup Before Changes

```bash
# ALWAYS backup before major changes
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup.$(date +%Y%m%d)
```

### 5. Use Tab Completion

```bash
# Type sebahagian, press Tab
cd /etc/ng[Tab]           # auto-complete to /etc/nginx
ls /var/log/ngi[Tab]      # auto-complete to /var/log/nginx
```

### 6. Combine Commands Efficiently

```bash
# Instead of:
find . -name "*.log"
find . -name "*.log" | wc -l

# Do:
find . -name "*.log" | tee log-files.txt | wc -l
```

---

## Command Summary

### File Operations - Safety Rating

| Command | Fungsi | Safety | Common Options |
|---------|--------|--------|----------------|
| `cp` | Copy file/directory | 🟢 Safe | `-r` recursive, `-i` interactive, `-v` verbose, `-p` preserve |
| `mv` | Move/rename | 🟡 Careful | `-i` interactive, `-v` verbose |
| `rm` | Remove file/directory | 🔴 Dangerous | `-r` recursive, `-i` interactive, `-f` force, `-v` verbose |

### Search & Find

| Command | Fungsi | Common Options | Example |
|---------|--------|----------------|---------|
| `find` | Find files/directories | `-name`, `-type`, `-size`, `-mtime` | `find . -name "*.log"` |
| `grep` | Search within files | `-r`, `-i`, `-n`, `-v`, `-c` | `grep -r "ERROR" logs/` |

### File Viewing

| Command | Fungsi | Use Case | Keyboard Shortcuts |
|---------|--------|----------|-------------------|
| `cat` | View entire file | Small files | - |
| `head` | View first lines | Check file start | `-n` number of lines |
| `tail` | View last lines | Check latest entries | `-f` follow, `-n` lines |
| `less` | Interactive viewer | Large files | `Space`, `g`, `G`, `q`, `/search` |
| `more` | Simple pager | Basic viewing | `Space`, `q` |

### File Comparison

| Command | Fungsi | Common Options | Example |
|---------|--------|----------------|---------|
| `diff` | Compare files | `-y` side-by-side, `-u` unified | `diff old.txt new.txt` |

### Pipes & Redirection

| Operator | Fungsi | Example |
|----------|--------|---------|
| `\|` | Pipe output | `ls -l \| grep txt` |
| `>` | Redirect (overwrite) | `echo "text" > file.txt` |
| `>>` | Redirect (append) | `echo "text" >> file.txt` |
| `&&` | AND (run if success) | `cd /tmp && ls` |
| `\|\|` | OR (run if fail) | `cd /tmp \|\| echo "failed"` |
| `;` | Sequential | `cd /tmp ; ls ; pwd` |

---

## Quick Reference - Common Tasks

### Backup Files

```bash
# Single file
cp important.conf important.conf.backup

# Directory
cp -r /etc/nginx ~/nginx-backup-$(date +%Y%m%d)

# With timestamp
cp config.txt config.txt.$(date +%Y%m%d-%H%M%S)
```

### Organize Files

```bash
# Move by pattern
mv *.log logs/

# Move and rename
mv old-name.txt new-name.txt

# Move to archive
mv old-files/* archive/
```

### Clean Up Files

```bash
# Remove specific files
rm -i unwanted.txt

# Remove by pattern (careful!)
rm -i *.tmp

# Remove old files (older than 30 days)
find . -name "*.log" -mtime +30 -delete
```

### Search Files

```bash
# Find by name
find /var/log -name "*nginx*.log"

# Find large files
find . -size +100M

# Find and list
find . -name "*.conf" -exec ls -lh {} \;
```

### Search Content

```bash
# Basic search
grep "ERROR" logfile.log

# Recursive search
grep -r "TODO" .

# Count occurrences
grep -c "ERROR" logfile.log

# With context
grep -C 3 "ERROR" logfile.log
```

### Monitor Logs

```bash
# View latest
tail -n 50 /var/log/nginx/access.log

# Follow real-time
tail -f /var/log/nginx/access.log

# Search and follow
tail -f /var/log/nginx/access.log | grep "404"
```

---

## Verification & Testing

### Verify Your Skills

Selepas complete lab ini, anda should boleh:

1. ✅ Copy files dan directories dengan betul
2. ✅ Move dan rename files dengan selamat
3. ✅ Remove files dengan safety practices
4. ✅ Find files menggunakan different criteria
5. ✅ Search content dalam files
6. ✅ View logs dalam real-time
7. ✅ Compare config files
8. ✅ Combine commands menggunakan pipes
9. ✅ Create simple backup scripts
10. ✅ Monitor nginx logs effectively

### Quick Self-Test

```bash
cd ~/lab21-practice

# Test 1: Copy
cp -r projects/website backups/website-backup && echo "✅ Copy OK"

# Test 2: Move
touch test.txt && mv test.txt backups/ && echo "✅ Move OK"

# Test 3: Find
find . -name "*.conf" | grep -q "nginx.conf" && echo "✅ Find OK"

# Test 4: Grep
grep -q "INFO" logs/app/app.log && echo "✅ Grep OK"

# Test 5: Pipe
ls -l | wc -l > /dev/null && echo "✅ Pipe OK"

echo ""
echo "🎉 All tests passed! You're ready for production!"
```

---

## Cleanup (Optional)

Kalau nak clean up practice files:

```bash
# Remove practice directory
rm -ri ~/lab21-practice

# Keep backups if needed
ls -lh ~/nginx-backups/
```

---

## What's Next?

Selepas master lab ini, anda dah ready untuk:

- **Lab 22**: Text Processing (sed, awk, cut, sort, uniq)
- **Lab 23**: Process Management (ps, top, kill, systemd)
- **Lab 24**: Bash Scripting Basics
- **Lab 25**: Automation dengan Cron Jobs

---

## Troubleshooting

### Issue: "Permission denied"

```bash
# Solution: Guna sudo
sudo cp /etc/nginx/nginx.conf ~/backup/
```

### Issue: "Directory not empty" ketika rmdir

```bash
# Solution: Guna rm -r instead
rm -r directory-name/
```

### Issue: find command terlalu slow

```bash
# Solution: Limit search scope
find /specific/path -name "pattern"

# Instead of searching everything
find / -name "pattern"
```

### Issue: grep no results

```bash
# Try case-insensitive
grep -i "pattern" file.txt

# Try showing all matches
grep -r "pattern" .
```

---

**Tahniah!** 🎉 Anda dah complete Lab 21 - Linux Intermediate Commands!

Sekarang anda dah ready untuk handle:
- File management tasks
- Log monitoring
- Basic DevOps operations
- System administration basics

Teruskan ke lab seterusnya untuk learn more advanced topics!
