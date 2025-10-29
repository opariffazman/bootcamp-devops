## LAB 22: Pengurusan Pengguna, Kumpulan & Skrip Shell

### Prerequisites

- Selesai [Lab 21](lab21.md)

## Bahagian 0: Connect ke EC2 Instance

### Langkah 1: Connect Menggunakan Session Manager

1. Pergi ke EC2 Console
2. Select instance anda
3. Click **Connect** > **Session Manager** > **Connect**
4. Switch to bash: `bash`

---

## Bahagian 1: Pengurusan Pengguna (User Management)

### Langkah 1: Lihat pengguna semasa

**whoami - Tunjukkan nama pengguna semasa**

```bash
whoami
```

```bash
id
```

### Langkah 2: Lihat senarai pengguna

```bash
cat /etc/passwd | grep -E "ssm-user|ubuntu|ec2-user"
```

### Langkah 3: Cipta pengguna baru

**useradd - Tambah pengguna baru**

```bash
sudo useradd -m -s /bin/bash devops1
```

**Nota:**
- `-m` = cipta home directory
- `-s /bin/bash` = set default shell

```bash
cat /etc/passwd | grep devops1
```

### Langkah 4: Set password untuk pengguna

```bash
sudo passwd devops1
```

**Nota:** Masukkan password (contoh: `DevOps123!`)

### Langkah 5: Cipta lebih banyak pengguna

```bash
sudo useradd -m -s /bin/bash devops2
sudo useradd -m -s /bin/bash devops3
```

```bash
sudo passwd devops2
sudo passwd devops3
```

---

## Bahagian 2: Pengurusan Kumpulan (Group Management)

### Langkah 1: Lihat kumpulan

**groups - Tunjukkan kumpulan pengguna**

```bash
groups
```

```bash
cat /etc/group | grep devops
```

### Langkah 2: Cipta kumpulan baru

**groupadd - Tambah kumpulan baru**

```bash
sudo groupadd developers
sudo groupadd operations
```

```bash
cat /etc/group | grep -E "developers|operations"
```

### Langkah 3: Tambah pengguna ke dalam kumpulan

**usermod - Ubah maklumat pengguna**

```bash
sudo usermod -aG developers devops1
sudo usermod -aG developers devops2
```

```bash
sudo usermod -aG operations devops3
```

**Nota:** `-aG` = append to group (tambah tanpa buang kumpulan sedia ada)

### Langkah 4: Verify kumpulan pengguna

```bash
groups devops1
groups devops2
groups devops3
```

---

## Bahagian 3: Kebenaran Fail untuk Kumpulan

### Langkah 1: Cipta direktori untuk kumpulan

```bash
cd /var/www
sudo mkdir -p projects/dev-team
sudo mkdir -p projects/ops-team
```

```bash
ls -l projects/
```

### Langkah 2: Set ownership kepada kumpulan

**chgrp - Tukar group ownership**

```bash
sudo chgrp developers projects/dev-team
sudo chgrp operations projects/ops-team
```

```bash
ls -l projects/
```

### Langkah 3: Set permissions untuk kumpulan

```bash
sudo chmod 770 projects/dev-team
sudo chmod 770 projects/ops-team
```

```bash
ls -l projects/
```

**Nota:** `770` = owner & group boleh baca/tulis/execute, others tiada akses

### Langkah 4: Test akses

```bash
sudo -u devops1 touch projects/dev-team/test1.txt
sudo -u devops2 touch projects/dev-team/test2.txt
```

```bash
ls -l projects/dev-team/
```

---

## Bahagian 4: Asas Skrip Shell

### Langkah 1: Cipta skrip pertama

**Skrip shell - Fail dengan arahan bash**

```bash
cd ~
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello from shell script!"' >> hello.sh
```

```bash
cat hello.sh
```

### Langkah 2: Beri permission execute

```bash
chmod +x hello.sh
```

```bash
ls -l hello.sh
```

### Langkah 3: Jalankan skrip

```bash
./hello.sh
```

### Langkah 4: Skrip dengan pembolehubah (variables)

**Cipta script menggunakan nano editor**

```bash
nano variables.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

# Variables
NAME="DevOps Engineer"
VERSION="1.0"

echo "Welcome, $NAME"
echo "Script Version: $VERSION"
echo "Current User: $(whoami)"
echo "Current Date: $(date +%Y-%m-%d)"
```

**Simpan dan keluar:**
- Tekan `Ctrl + O` untuk save
- Tekan `Enter` untuk confirm
- Tekan `Ctrl + X` untuk keluar dari nano

**Verify kandungan:**

```bash
cat variables.sh
```

**Jalankan script:**

```bash
chmod +x variables.sh
./variables.sh
```

---

## Bahagian 5: Conditional Statements (if)

### Langkah 1: Skrip dengan if statement

```bash
nano check-file.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

FILE="test.txt"

if [ -f "$FILE" ]; then
    echo "✓ File $FILE wujud"
else
    echo "✗ File $FILE tidak wujud"
fi
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x check-file.sh
```

### Langkah 2: Test skrip

```bash
./check-file.sh
```

```bash
touch test.txt
./check-file.sh
```

### Langkah 3: Skrip check service

```bash
nano check-nginx.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

if systemctl is-active --quiet nginx; then
    echo "✓ Nginx is running"
    echo "Status: $(systemctl status nginx | grep Active)"
else
    echo "✗ Nginx is not running"
fi
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x check-nginx.sh
./check-nginx.sh
```

---

## Bahagian 6: Loops - For Loop

### Langkah 1: For loop asas

```bash
nano for-loop.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=== Counting 1 to 5 ==="
for i in 1 2 3 4 5
do
    echo "Number: $i"
done
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x for-loop.sh
./for-loop.sh
```

### Langkah 2: For loop dengan range

```bash
nano for-range.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=== Creating backup files ==="
for i in {1..3}
do
    echo "Creating backup-$i.txt"
    touch backup-$i.txt
done

ls -l backup-*.txt
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x for-range.sh
./for-range.sh
```

### Langkah 3: For loop untuk files

```bash
nano list-html.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=== HTML Files in /var/www/site1/html ==="
for file in /var/www/site1/html/*.html
do
    if [ -f "$file" ]; then
        echo "Found: $(basename $file)"
        echo "  Size: $(ls -lh $file | awk '{print $5}')"
    fi
done
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x list-html.sh
./list-html.sh
```

---

## Bahagian 7: Loops - While Loop

### Langkah 1: While loop asas

```bash
nano while-loop.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=== Countdown from 5 ==="
COUNT=5

while [ $COUNT -gt 0 ]
do
    echo "Count: $COUNT"
    COUNT=$((COUNT - 1))
    sleep 1
done

echo "Done!"
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x while-loop.sh
./while-loop.sh
```

### Langkah 2: While loop membaca file

```bash
nano read-file.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

if [ ! -f "colors.txt" ]; then
    echo "Creating colors.txt..."
    echo -e "red\ngreen\nblue\nyellow" > colors.txt
fi

echo "=== Reading colors.txt ==="
while read line
do
    echo "Color: $line"
done < colors.txt
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x read-file.sh
./read-file.sh
```

---

## Bahagian 8: Skrip Automasi Praktis

### Langkah 1: Backup script

```bash
nano backup-website.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

# Configuration
SOURCE_DIR="/var/www/site1/html"
BACKUP_DIR="/var/www/backups"
DATE=$(date +%Y%m%d-%H%M%S)
BACKUP_FILE="website-backup-$DATE.tar.gz"

echo "=== Website Backup Script ==="
echo "Source: $SOURCE_DIR"
echo "Destination: $BACKUP_DIR/$BACKUP_FILE"

# Create backup directory if not exists
if [ ! -d "$BACKUP_DIR" ]; then
    echo "Creating backup directory..."
    sudo mkdir -p "$BACKUP_DIR"
fi

# Create backup
echo "Creating backup..."
sudo tar -czf "$BACKUP_DIR/$BACKUP_FILE" -C "$(dirname $SOURCE_DIR)" "$(basename $SOURCE_DIR)"

if [ $? -eq 0 ]; then
    echo "✓ Backup completed successfully!"
    echo "Backup size: $(sudo ls -lh $BACKUP_DIR/$BACKUP_FILE | awk '{print $5}')"
else
    echo "✗ Backup failed!"
    exit 1
fi

# List all backups
echo ""
echo "=== All Backups ==="
sudo ls -lh "$BACKUP_DIR"
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x backup-website.sh
```

### Langkah 2: Jalankan backup script

```bash
./backup-website.sh
```

### Langkah 3: System monitoring script

```bash
nano system-info.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=================================="
echo "     SYSTEM INFORMATION"
echo "=================================="
echo ""

echo "--- Hostname ---"
hostname

echo ""
echo "--- Current User ---"
whoami

echo ""
echo "--- Uptime ---"
uptime

echo ""
echo "--- Memory Usage ---"
free -h | grep -E "Mem|Swap"

echo ""
echo "--- Disk Usage ---"
df -h | grep -E "Filesystem|/$"

echo ""
echo "--- CPU Info ---"
lscpu | grep -E "Model name|CPU\(s\):"

echo ""
echo "--- Active Services ---"
systemctl list-units --type=service --state=running | grep -E "nginx|amazon-ssm-agent" | head -5

echo ""
echo "=================================="
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x system-info.sh
./system-info.sh
```

### Langkah 4: User management script

```bash
nano create-users.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

echo "=== Bulk User Creation Script ==="

# Array of usernames
USERS=("developer1" "developer2" "operator1")

for user in "${USERS[@]}"
do
    # Check if user exists
    if id "$user" &>/dev/null; then
        echo "⚠ User $user already exists"
    else
        echo "Creating user: $user"
        sudo useradd -m -s /bin/bash "$user"
        echo "✓ User $user created"
    fi
done

echo ""
echo "=== User List ==="
cat /etc/passwd | grep -E "developer|operator"
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x create-users.sh
./create-users.sh
```

---

## Bahagian 9: Script dengan Parameters

### Langkah 1: Script dengan arguments

```bash
nano greet.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

# Check if name parameter provided
if [ $# -eq 0 ]; then
    echo "Usage: $0 <name>"
    exit 1
fi

NAME=$1
echo "Hello, $NAME!"
echo "Welcome to DevOps Bootcamp"
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x greet.sh
```

### Langkah 2: Test script dengan parameter

```bash
./greet.sh
```

```bash
./greet.sh Ahmad
```

```bash
./greet.sh "Siti Nurhaliza"
```

### Langkah 3: Script dengan multiple parameters

```bash
nano file-ops.sh
```

**Taip kandungan berikut:**

```bash
#!/bin/bash

# Check parameters
if [ $# -lt 2 ]; then
    echo "Usage: $0 <operation> <filename>"
    echo "Operations: create, delete, check"
    exit 1
fi

OPERATION=$1
FILENAME=$2

case $OPERATION in
    create)
        touch "$FILENAME"
        echo "✓ Created file: $FILENAME"
        ;;
    delete)
        if [ -f "$FILENAME" ]; then
            rm "$FILENAME"
            echo "✓ Deleted file: $FILENAME"
        else
            echo "✗ File $FILENAME does not exist"
        fi
        ;;
    check)
        if [ -f "$FILENAME" ]; then
            echo "✓ File $FILENAME exists"
            ls -lh "$FILENAME"
        else
            echo "✗ File $FILENAME does not exist"
        fi
        ;;
    *)
        echo "✗ Invalid operation: $OPERATION"
        echo "Valid operations: create, delete, check"
        exit 1
        ;;
esac
```

**Simpan:** `Ctrl + O`, `Enter`, `Ctrl + X`

```bash
chmod +x file-ops.sh
```

### Langkah 4: Test file operations script

```bash
./file-ops.sh create myfile.txt
```

```bash
./file-ops.sh check myfile.txt
```

```bash
./file-ops.sh delete myfile.txt
```

---

## Command Summary

### User Management

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `whoami` | Show current user | `whoami` |
| `id` | Show user ID and groups | `id` |
| `useradd -m user` | Create new user with home directory | `sudo useradd -m devops1` |
| `passwd user` | Set user password | `sudo passwd devops1` |
| `userdel user` | Delete user | `sudo userdel devops1` |

### Group Management

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `groups` | Show user's groups | `groups` |
| `groupadd group` | Create new group | `sudo groupadd developers` |
| `usermod -aG group user` | Add user to group | `sudo usermod -aG developers devops1` |
| `chgrp group file` | Change file group | `sudo chgrp developers file.txt` |

### Shell Script Basics

| Konsep | Fungsi | Contoh |
|--------|--------|--------|
| `#!/bin/bash` | Shebang - specify interpreter | First line of script |
| `chmod +x script.sh` | Make script executable | `chmod +x hello.sh` |
| `./script.sh` | Execute script | `./hello.sh` |
| `$VARIABLE` | Access variable value | `echo $NAME` |
| `$(command)` | Command substitution | `DATE=$(date)` |

### Conditional Statements

| Syntax | Fungsi | Contoh |
|--------|--------|--------|
| `if [ condition ]; then` | If statement | `if [ -f file ]; then` |
| `[ -f file ]` | Check if file exists | `if [ -f test.txt ]` |
| `[ -d dir ]` | Check if directory exists | `if [ -d /var/www ]` |
| `[ $a -eq $b ]` | Check if equal | `if [ $COUNT -eq 0 ]` |
| `[ $a -gt $b ]` | Check if greater than | `if [ $COUNT -gt 0 ]` |

### Loops

| Loop Type | Fungsi | Contoh |
|-----------|--------|--------|
| `for i in list` | Iterate over list | `for i in 1 2 3; do echo $i; done` |
| `for i in {1..5}` | Range loop | `for i in {1..5}; do echo $i; done` |
| `while [ condition ]` | While loop | `while [ $COUNT -gt 0 ]; do` |
| `read line` | Read input | `while read line; do echo $line; done` |

### Script Parameters

| Variable | Fungsi | Contoh |
|----------|--------|--------|
| `$0` | Script name | `echo $0` |
| `$1, $2, ...` | Positional parameters | `NAME=$1` |
| `$#` | Number of parameters | `if [ $# -eq 0 ]` |
| `$@` | All parameters | `for arg in "$@"` |

---
