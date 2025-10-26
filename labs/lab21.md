## LAB 21: Linux Command Asas - Operasi Fail

## Bahagian 0: Connect & Verify Environment

### Langkah 1: Connect ke Instance

1. Pergi ke EC2 Console
2. Select instance anda
3. Click **Connect** > **Session Manager** > **Connect**
4. Switch to bash: `bash`

### Langkah 2: Verify Nginx Setup

```bash
# Check nginx running
sudo systemctl status nginx

# Navigate to website directory
cd /var/www/site1/html

# Check current file
ls -l

# Test current site
curl localhost
```

---

## Bahagian 1: Command touch

### Langkah 1: Create Simple File

```bash
# Make sure you're in the right directory
cd /var/www/site1/html

# Create new empty file
sudo touch test.txt

# Verify created
ls -l test.txt
```

### Langkah 2: Update File Timestamp

```bash
# Check current timestamp
ls -l index.html

# Update timestamp
sudo touch index.html

# Check updated timestamp (time will be different)
ls -l index.html
```

---

## Bahagian 2: Command echo & Redirectors

### Langkah 1: Simple Echo

```bash
# Print text to screen
echo "Hello DevOps"

# Print current user
echo $USER
```

### Langkah 2: Redirector > (Overwrite)

**Redirector `>` - Write to file (overwrite existing content)**

```bash
# Create simple text file
cd ~
echo "This is line 1" > test.txt

# View the file
cat test.txt

# Try echo again - akan OVERWRITE
echo "This is NEW line 1" > test.txt

# View file - line pertama hilang!
cat test.txt
```

**Nota:** `>` akan delete content lama dan write content baru.

### Langkah 3: Redirector >> (Append)

**Redirector `>>` - Append to file (keep existing content)**

```bash
# Add more lines WITHOUT deleting existing content
echo "This is line 2" >> test.txt
echo "This is line 3" >> test.txt

# View the file - semua lines ada
cat test.txt
```

**Nota:** `>>` akan ADD content baru tanpa delete yang lama.

### Langkah 4: Understanding the Difference

```bash
# Start fresh
echo "Line 1" > demo.txt
cat demo.txt

# Use > again - akan overwrite!
echo "New Line 1" > demo.txt
cat demo.txt

# Use >> - akan append
echo "Line 2" >> demo.txt
echo "Line 3" >> demo.txt
cat demo.txt
```

**Summary:**

- `>` = Overwrite (delete old, write new)
- `>>` = Append (keep old, add new)

---

## Bahagian 3: Command cat

### Langkah 1: View File Content

```bash
# View our test file
cat test.txt

# View the website index.html
cat /var/www/site1/html/index.html
```

---

## Bahagian 4: Command grep (Search Text)

### Langkah 1: Simple Search

**grep - Search for text dalam file**

```bash
# Create test file with multiple lines
cd ~
cat > colors.txt << 'EOF'
red
blue
green
red
yellow
blue
EOF

# Search for "red"
grep "red" colors.txt

# Search for "blue"
grep "blue" colors.txt
```

### Langkah 2: Search dalam Website File

```bash
# Search for "Version" dalam index.html
grep "Version" /var/www/site1/html/index.html

# Search for "title" (case sensitive)
grep "title" /var/www/site1/html/index.html

# Search case-insensitive dengan -i
grep -i "title" /var/www/site1/html/index.html
```

### Langkah 3: Practical DevOps Usage

```bash
# Check what version deployed
grep "Version" /var/www/site1/html/index.html

# Verify backup contains old version
grep "Version" /var/www/backups/index-v3-backup.html
```

---

## Bahagian 5: Pipe | (Connect Commands)

### Langkah 1: Understanding Pipe

**Pipe `|` - Send output dari satu command ke command lain**

```bash
# Without pipe - just view file
cat colors.txt

# With pipe - view file AND search
cat colors.txt | grep "red"
```

**Nota:** `|` ambil output dari command kiri, hantar ke command kanan.

### Langkah 2: cat dengan grep

```bash
# View file and search
cat /var/www/site1/html/index.html | grep "Version"

# View file and search (case-insensitive)
cat /var/www/site1/html/index.html | grep -i "body"
```

### Langkah 3: Practical Examples

```bash
# Test website and check version
curl localhost | grep "Version"

# Check specific text in website
curl localhost | grep "Welcome"
```

---

## Bahagian 6: Command tee vs Redirector

### Langkah 1: Fahami tee

**tee - Write to file AND display to screen at same time**

```bash
cd ~

# Using redirector > - ONLY write to file (no screen output)
echo "Test with redirector" > redirect.txt
cat redirect.txt

# Using tee - write to file AND show on screen
echo "Test with tee" | tee tee.txt
```

**Nota:** `tee` berguna bila nak save output DAN tengok sekali.

### Langkah 2: tee vs Redirector

```bash
# Redirector > - silent (no screen output)
echo "Line 1" > method1.txt
echo "Line 2" >> method1.txt

# tee - shows output while writing
echo "Line 1" | tee method2.txt
echo "Line 2" | tee -a method2.txt

# Both files have same content
cat method1.txt
cat method2.txt
```

**Summary:**

- `>` or `>>` = Silent, hanya write ke file
- `tee` = Write ke file DAN display di screen
- `tee -a` = Append mode (sama seperti >>)

### Langkah 3: Guna tee jika perlu sudo

```bash
cd /var/www/site1/html

# akan gagal
sudo echo "test" > test-fail.txt

# akan lulus
echo "test" | sudo tee test-success.txt > /dev/null

# Verify
ls -l test-success.txt
```

---

## Bahagian 7: Command cp (Copy)

### Langkah 1: Copy Single File

```bash
# Navigate to website
cd /var/www/site1/html

# Copy index.html to backup
sudo cp index.html index-backup.html

# Verify copied
ls -l index*
```

### Langkah 2: Copy with Date

```bash
# Create backup with date
sudo cp index.html index-$(date +%Y%m%d).html

# Verify
ls -l
```

### Langkah 3: Copy to Different Directory

```bash
# Create backup directory
sudo mkdir -p /var/www/backups

# Copy file to backup directory
sudo cp index.html /var/www/backups/

# Verify
ls -l /var/www/backups/
```

### Langkah 4: Practical Backup Before Change

```bash
# Always backup before making changes!
cd /var/www/site1/html

# Create timestamped backup
sudo cp index.html /var/www/backups/index-$(date +%Y%m%d-%H%M%S).html

# Now safe to make changes
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Website - Version 3.0</title>
    <style>
        body {
            font-family: Arial;
            margin: 40px;
            background-color: #fff3e0;
        }
        h1 { color: #e65100; }
    </style>
</head>
<body>
    <h1>Welcome to My Updated Website</h1>
    <p>Version: 3.0</p>
    <p>Now with new colors!</p>
</body>
</html>
EOF

# Test
curl localhost

# Check backups exist
ls -l /var/www/backups/
```

---

## Bahagian 8: Command mv (Move/Rename)

### Langkah 1: Rename File

```bash
cd /var/www/site1/html

# Create test file
sudo touch old-name.html

# Rename it
sudo mv old-name.html new-name.html

# Verify renamed
ls -l *name*.html
```

### Langkah 2: Move File

```bash
# Create archive directory
sudo mkdir -p archive

# Move old backup files to archive
sudo mv index-backup.html archive/

# Verify moved
ls -l archive/
```

### Langkah 3: Practical - Restore from Backup

```bash
# List available backups
ls -l /var/www/backups/

# Restore from backup (if needed)
# First, backup current version
sudo cp index.html index-before-restore.html

# Then restore from backup
sudo cp /var/www/backups/index-*.html index.html

# Test
curl localhost
```

---

## Bahagian 9: Hands-On Exercise - Website Update Workflow

### Exercise 1: Update Website Safely

```bash
# Step 1: Navigate to website
cd /var/www/site1/html

# Step 2: Create backup
sudo cp index.html /var/www/backups/index-v3-backup.html

# Step 3: Update website
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Website - Version 4.0</title>
    <style>
        body {
            font-family: Arial;
            margin: 40px;
            text-align: center;
            background: linear-gradient(to right, #667eea, #764ba2);
            color: white;
        }
        .container {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>My Awesome Website</h1>
        <p>Version: 4.0</p>
        <p>Updated with new design!</p>
    </div>
</body>
</html>
EOF

# Step 4: Test the update
curl localhost

# Step 5: Verify backup exists
ls -l /var/www/backups/
```

### Exercise 2: Rollback if Needed

```bash
# If you want to undo changes:

# Step 1: Check available backups
ls -l /var/www/backups/

# Step 2: Copy backup to restore
sudo cp /var/www/backups/index-v3-backup.html index.html

# Step 3: Test restored version
curl localhost
```

### Exercise 3: Create Simple Maintenance Page

```bash
cd /var/www/site1/html

# Backup current site
sudo cp index.html index-live.html

# Create maintenance page
sudo tee index.html > /dev/null << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Maintenance</title>
    <style>
        body {
            font-family: Arial;
            text-align: center;
            padding: 50px;
            background-color: #ffa500;
            color: white;
        }
    </style>
</head>
<body>
    <h1>Under Maintenance</h1>
    <p>We are updating our website.</p>
    <p>Please check back soon!</p>
</body>
</html>
EOF

# Test maintenance page
curl localhost

# Switch back to live site
sudo mv index-live.html index.html

# Test live site
curl localhost
```

---

## Bahagian 10: Understanding File Content

### Langkah 1: View Current Website

```bash
cd /var/www/site1/html

# View complete file
cat index.html

# Search for specific text
grep "Version" index.html

# Search using cat and pipe
cat index.html | grep "Version"
```

### Langkah 2: Compare Files

```bash
# View current version title
echo "=== Current Version ==="
grep "title" index.html
grep "Version" index.html

# View backup version title
echo "=== Backup Version ==="
grep "title" /var/www/backups/index-v3-backup.html
grep "Version" /var/www/backups/index-v3-backup.html
```

### Langkah 3: Verify Deployment

```bash
# Check what's currently deployed
cat index.html | grep "Version"

# Check what's in backup
cat /var/www/backups/index-v3-backup.html | grep "Version"

# Verify they are different
echo "=== Live Site ==="
grep "Version" index.html
echo "=== Backup ==="
grep "Version" /var/www/backups/index-v3-backup.html
```

---

## Bahagian 11: Command find (Search Files)

### Langkah 1: Find by Name

```bash
# Find file dengan nama spesifik
cd /var/www/site1/html

# Find index.html
find . -name "index.html"

# Find all HTML files
find . -name "*.html"
```

### Langkah 2: Find dalam Multiple Directories

```bash
# Find all HTML files dalam /var/www
find /var/www -name "*.html"

# Find all backup files
find /var/www/backups -name "index-*.html"
```

### Langkah 3: Find by Type

```bash
# Find files only (bukan directory)
find /var/www/site1/html -type f

# Find directories only
find /var/www -type d
```

---

## Bahagian 12: Quick Reference - Cara Buat File

### Method 1: echo dengan >

```bash
# Simple single line file
echo "Hello World" > file.txt

# Good for: Quick one-line files
```

### Method 2: echo dengan >>

```bash
# Build file line by line
echo "Line 1" > file.txt
echo "Line 2" >> file.txt
echo "Line 3" >> file.txt

# Good for: Adding content progressively
```

### Method 3: cat dengan <<

```bash
# Multi-line file
cat > file.txt << 'EOF'
Line 1
Line 2
Line 3
EOF

# Good for: Multi-line content
```

### Method 4: tee

```bash
# With display and sudo
echo "Content" | sudo tee file.txt > /dev/null

---

## Command Summary

### Redirectors & Pipes

| Symbol | Nama | Fungsi | Contoh |
|--------|------|--------|--------|
| `>` | Redirect (overwrite) | Write to file, replace old content | `echo "text" > file.txt` |
| `>>` | Redirect (append) | Add to file, keep old content | `echo "text" >> file.txt` |
| `\|` | Pipe | Send output to next command | `cat file \| grep "word"` |

### File Creation

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `touch file` | Create empty file | `touch test.txt` |
| `echo "text" > file` | Create file with text | `echo "hello" > test.txt` |
| `cat > file` | Create file interactively | `cat > test.txt` |
| `tee file` | Write and display | `echo "text" \| tee file.txt` |

### File Viewing

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `cat file` | Display file content | `cat index.html` |
| `cat file \| grep` | Search in file | `cat index.html \| grep Version` |

### File Operations

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `cp file backup` | Copy file | `cp index.html backup.html` |
| `mv old new` | Rename file | `mv old.html new.html` |
| `mv file dir/` | Move file | `mv file.txt backup/` |

### Search Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `grep "text" file` | Search text in file | `grep "Version" index.html` |
| `find . -name "file"` | Find file by name | `find . -name "*.html"` |
| `find . -type f` | Find files only | `find /var/www -type f` |

---
