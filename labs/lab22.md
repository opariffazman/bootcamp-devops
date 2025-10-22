## LAB 22: Shell Scripting Asas - Automasi Tugas DevOps (Sesi 3)

### 🎯 Objektif
- Menulis shell script asas
- Menggunakan variables dan user input
- Implement conditionals (if/else)
- Implement loops (for, while)
- Automasikan tugas DevOps yang common

### ⏱️ Durasi
2 jam

### 📋 Prerequisites
- Selesai Lab 20 & 21 (Sesi 1 & 2)
- EC2 instance running dengan terminal access
- Familiar dengan Linux commands asas

---

## Bahagian 1: Shell Script Pertama (15 minit)

### Langkah 1: Buat Script File

```bash
cd ~
mkdir -p devops-scripts
cd devops-scripts

# Create first script
nano hello.sh
```

**Taip content berikut:**

```bash
#!/bin/bash
# My first shell script

echo "Hello, World!"
echo "Today is $(date)"
echo "Current user: $USER"
echo "Current directory: $(pwd)"
```

**Save:** Tekan `Ctrl+X`, then `Y`, then `Enter`

### Langkah 2: Make Script Executable

```bash
# Check permissions
ls -l hello.sh
```

**Expected output:**
```
-rw-r--r-- 1 ec2-user ec2-user 142 Oct 22 12:00 hello.sh
```

```bash
# Make executable
chmod +x hello.sh

# Check permissions now
ls -l hello.sh
```

**Expected output:**
```
-rwxr-xr-x 1 ec2-user ec2-user 142 Oct 22 12:00 hello.sh
```

### Langkah 3: Run Script

```bash
# Run script
./hello.sh
```

**Expected output:**
```
Hello, World!
Today is Wed Oct 22 12:00:00 UTC 2024
Current user: ec2-user
Current directory: /home/ec2-user/devops-scripts
```

### Langkah 4: Understanding Shebang

**Important elements:**

- `#!/bin/bash` - Shebang (tell system to use bash)
- `#` - Comments (untuk documentation)
- `echo` - Print output
- `$(command)` - Command substitution
- `$VARIABLE` - Access variable value

**✅ Success:** Anda telah buat dan run script pertama!

---

## Bahagian 2: Variables (20 minit)

### Langkah 1: Basic Variables

```bash
nano variables.sh
```

**Content:**

```bash
#!/bin/bash
# Variables demo

# Define variables (NO SPACES around =)
NAME="Ahmad"
AGE=25
SERVER="web-server-01"
PORT=8080

# Using variables
echo "Name: $NAME"
echo "Age: $AGE"
echo "Server: $SERVER running on port $PORT"

# Better practice - use curly braces
echo "Backup file: ${NAME}_backup.tar.gz"
```

**Save, make executable, dan run:**

```bash
chmod +x variables.sh
./variables.sh
```

**Expected output:**
```
Name: Ahmad
Age: 25
Server: web-server-01 running on port 8080
Backup file: Ahmad_backup.tar.gz
```

### Langkah 2: Environment Variables

```bash
nano env_vars.sh
```

**Content:**

```bash
#!/bin/bash
# Environment variables

echo "User: $USER"
echo "Home: $HOME"
echo "Path: $PATH"
echo "Hostname: $HOSTNAME"
echo "Shell: $SHELL"
```

```bash
chmod +x env_vars.sh
./env_vars.sh
```

### Langkah 3: Command Output to Variable

```bash
nano command_output.sh
```

**Content:**

```bash
#!/bin/bash
# Store command output in variables

CURRENT_DATE=$(date +%Y%m%d)
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}')
MEMORY_FREE=$(free -h | awk 'NR==2 {print $4}')
IP_ADDRESS=$(hostname -I | awk '{print $1}')

echo "Date: $CURRENT_DATE"
echo "Disk usage: $DISK_USAGE"
echo "Free memory: $MEMORY_FREE"
echo "IP Address: $IP_ADDRESS"
```

```bash
chmod +x command_output.sh
./command_output.sh
```

**Expected output:**
```
Date: 20241022
Disk usage: 45%
Free memory: 2.1Gi
IP Address: 172.31.x.x
```

### Langkah 4: Server Info Script

```bash
nano server_info.sh
```

**Content:**

```bash
#!/bin/bash
# Display server information

HOSTNAME=$(hostname)
IP_ADDRESS=$(hostname -I | awk '{print $1}')
UPTIME=$(uptime -p)
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}')
MEMORY_USAGE=$(free | awk 'NR==2 {printf "%.2f%%", $3*100/$2}')

echo "==================================="
echo "   SERVER INFORMATION"
echo "==================================="
echo "Hostname    : $HOSTNAME"
echo "IP Address  : $IP_ADDRESS"
echo "Uptime      : $UPTIME"
echo "Disk Usage  : $DISK_USAGE"
echo "Memory Usage: $MEMORY_USAGE"
echo "==================================="
```

```bash
chmod +x server_info.sh
./server_info.sh
```

**✅ Success:** Anda faham variables dalam shell scripting

---

## Bahagian 3: User Input (15 minit)

### Langkah 1: Simple Input

```bash
nano input_demo.sh
```

**Content:**

```bash
#!/bin/bash
# Reading user input

echo "What is your name?"
read NAME
echo "Hello, $NAME!"

# Input with prompt (better)
read -p "Enter server name: " SERVER
echo "Connecting to $SERVER..."
```

```bash
chmod +x input_demo.sh
./input_demo.sh
```

**Test:** Enter your name and server name when prompted.

### Langkah 2: Silent Input (Passwords)

```bash
nano password_demo.sh
```

**Content:**

```bash
#!/bin/bash
# Silent input for passwords

read -sp "Enter password: " PASSWORD
echo
echo "Password saved (hidden)"
echo "Password length: ${#PASSWORD} characters"
```

```bash
chmod +x password_demo.sh
./password_demo.sh
```

### Langkah 3: Script Arguments

```bash
nano arguments.sh
```

**Content:**

```bash
#!/bin/bash
# Script arguments demo

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

```bash
chmod +x arguments.sh

# Run with arguments
./arguments.sh web-server production 8080
```

**Expected output:**
```
Script name: ./arguments.sh
First argument: web-server
Second argument: production
All arguments: web-server production 8080
Number of arguments: 3
```

### Langkah 4: Backup Script Dengan Input

```bash
nano backup_input.sh
```

**Content:**

```bash
#!/bin/bash
# Backup script with user input

read -p "Enter directory to backup: " SOURCE_DIR
read -p "Enter backup destination: " BACKUP_DIR
read -p "Enter backup name: " BACKUP_NAME

# Create backup
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="${BACKUP_DIR}/${BACKUP_NAME}_${TIMESTAMP}.tar.gz"

echo "Creating backup..."
echo "Source: $SOURCE_DIR"
echo "Destination: $BACKUP_FILE"

# Create backup directory if not exists
mkdir -p $BACKUP_DIR

# Create backup
tar -czf $BACKUP_FILE $SOURCE_DIR 2>/dev/null

if [ $? -eq 0 ]; then
    echo "Backup successful!"
    ls -lh $BACKUP_FILE
else
    echo "Backup failed!"
fi
```

```bash
chmod +x backup_input.sh

# Test (create test directory first)
mkdir -p ~/test-backup
./backup_input.sh
```

**Test with:**
- Directory: `/home/ec2-user/devops-scripts`
- Destination: `/home/ec2-user/test-backup`
- Name: `scripts-backup`

**✅ Success:** Anda faham user input dalam scripts

---

## Bahagian 4: Conditionals (25 minit)

### Langkah 1: Simple If Statement

```bash
nano if_demo.sh
```

**Content:**

```bash
#!/bin/bash
# If statement demo

if [ $USER == "ec2-user" ]; then
    echo "Running as ec2-user"
fi

if [ $USER == "root" ]; then
    echo "Running as root"
else
    echo "Not running as root"
fi
```

```bash
chmod +x if_demo.sh
./if_demo.sh
```

### Langkah 2: File Checks

```bash
nano file_check.sh
```

**Content:**

```bash
#!/bin/bash
# File and directory checks

# Check if file exists
if [ -f /etc/passwd ]; then
    echo "✓ /etc/passwd exists"
fi

# Check if directory exists
if [ -d /var/log ]; then
    echo "✓ /var/log directory exists"
fi

# Check if file is readable
if [ -r ~/.bashrc ]; then
    echo "✓ .bashrc is readable"
fi

# Check if file is writable
if [ -w ~/test.txt ]; then
    echo "✓ test.txt is writable"
else
    echo "✗ test.txt is not writable or doesn't exist"
fi
```

```bash
chmod +x file_check.sh
./file_check.sh
```

### Langkah 3: Numeric Comparisons

```bash
nano disk_check.sh
```

**Content:**

```bash
#!/bin/bash
# Disk usage check with if/elif/else

DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

echo "Current disk usage: ${DISK_USAGE}%"

if [ $DISK_USAGE -lt 50 ]; then
    echo "Status: ✓ OK"
elif [ $DISK_USAGE -lt 80 ]; then
    echo "Status: ⚠ WARNING"
else
    echo "Status: ✗ CRITICAL"
fi
```

```bash
chmod +x disk_check.sh
./disk_check.sh
```

### Langkah 4: String Comparisons

```bash
nano string_check.sh
```

**Content:**

```bash
#!/bin/bash
# String comparisons

read -p "Enter environment (dev/staging/prod): " ENV

if [ "$ENV" == "prod" ]; then
    echo "⚠ WARNING: You are in PRODUCTION!"
    read -p "Are you sure? (yes/no): " CONFIRM
    if [ "$CONFIRM" != "yes" ]; then
        echo "Deployment cancelled"
        exit 0
    fi
fi

echo "Deploying to $ENV environment..."
```

```bash
chmod +x string_check.sh
./string_check.sh
```

### Langkah 5: System Health Check Script

```bash
nano health_check.sh
```

**Content:**

```bash
#!/bin/bash
# System health check

echo "==================================="
echo "   SYSTEM HEALTH CHECK"
echo "==================================="

# Check disk usage
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
echo -n "Disk Usage: ${DISK_USAGE}% - "
if [ $DISK_USAGE -gt 80 ]; then
    echo "✗ CRITICAL"
elif [ $DISK_USAGE -gt 50 ]; then
    echo "⚠ WARNING"
else
    echo "✓ OK"
fi

# Check memory usage
MEMORY_USAGE=$(free | awk 'NR==2 {printf "%.0f", $3*100/$2}')
echo -n "Memory Usage: ${MEMORY_USAGE}% - "
if [ $MEMORY_USAGE -gt 80 ]; then
    echo "✗ CRITICAL"
elif [ $MEMORY_USAGE -gt 60 ]; then
    echo "⚠ WARNING"
else
    echo "✓ OK"
fi

# Check if important directories exist
if [ -d /var/log ]; then
    echo "Log Directory: ✓ OK"
else
    echo "Log Directory: ✗ MISSING"
fi

echo "==================================="
```

```bash
chmod +x health_check.sh
./health_check.sh
```

**Comparison operators reference:**

**Numeric:**
- `-eq` : equal to
- `-ne` : not equal to
- `-gt` : greater than
- `-ge` : greater than or equal
- `-lt` : less than
- `-le` : less than or equal

**String:**
- `==` : equal to
- `!=` : not equal to
- `-z` : string is empty
- `-n` : string is not empty

**File:**
- `-f` : file exists
- `-d` : directory exists
- `-r` : file is readable
- `-w` : file is writable
- `-x` : file is executable

**✅ Success:** Anda faham conditionals dalam bash

---

## Bahagian 5: Loops (30 minit)

### Langkah 1: Simple For Loop

```bash
nano for_demo.sh
```

**Content:**

```bash
#!/bin/bash
# For loop demo

# Loop over list
echo "Checking servers..."
for SERVER in web1 web2 web3 db1; do
    echo "  - Checking $SERVER"
done

echo ""

# Loop with range
echo "Countdown:"
for i in {5..1}; do
    echo "  $i"
    sleep 1
done
echo "  Blast off!"
```

```bash
chmod +x for_demo.sh
./for_demo.sh
```

### Langkah 2: Loop Over Files

```bash
nano file_loop.sh
```

**Content:**

```bash
#!/bin/bash
# Loop over files

# Create test files first
mkdir -p ~/loop-test
touch ~/loop-test/{file1,file2,file3}.txt

echo "Processing files..."
for FILE in ~/loop-test/*.txt; do
    echo "  Processing: $FILE"
    # Add actual processing here
done
```

```bash
chmod +x file_loop.sh
./file_loop.sh
```

### Langkah 3: C-Style For Loop

```bash
nano c_style_loop.sh
```

**Content:**

```bash
#!/bin/bash
# C-style for loop

echo "Creating backup files..."
for ((i=1; i<=5; i++)); do
    FILENAME="backup_${i}.tar.gz"
    echo "  Creating $FILENAME"
    touch ~/loop-test/$FILENAME
done

echo "Done!"
ls -l ~/loop-test/
```

```bash
chmod +x c_style_loop.sh
./c_style_loop.sh
```

### Langkah 4: While Loop

```bash
nano while_demo.sh
```

**Content:**

```bash
#!/bin/bash
# While loop demo

COUNT=1
echo "Counting to 5..."
while [ $COUNT -le 5 ]; do
    echo "  Count: $COUNT"
    COUNT=$((COUNT + 1))
    sleep 1
done

echo "Done!"
```

```bash
chmod +x while_demo.sh
./while_demo.sh
```

### Langkah 5: Read File Line by Line

```bash
# Create server list first
cat > servers.txt << EOF
web1.example.com
web2.example.com
db1.example.com
cache1.example.com
EOF

nano read_file.sh
```

**Content:**

```bash
#!/bin/bash
# Read file line by line

if [ ! -f servers.txt ]; then
    echo "Error: servers.txt not found!"
    exit 1
fi

echo "Servers in list:"
while IFS= read -r SERVER; do
    echo "  - $SERVER"
done < servers.txt
```

```bash
chmod +x read_file.sh
./read_file.sh
```

### Langkah 6: Multi-Server Check Script

```bash
nano check_servers.sh
```

**Content:**

```bash
#!/bin/bash
# Check multiple servers

SERVER_FILE="servers.txt"

if [ ! -f $SERVER_FILE ]; then
    echo "Error: $SERVER_FILE not found!"
    exit 1
fi

echo "==================================="
echo "   SERVER CONNECTIVITY CHECK"
echo "==================================="

while IFS= read -r SERVER; do
    echo -n "Checking $SERVER... "
    
    # Ping server (1 packet, 1 second timeout)
    if ping -c 1 -W 1 $SERVER &> /dev/null; then
        echo "✓ UP"
    else
        echo "✗ DOWN"
    fi
done < $SERVER_FILE

echo "==================================="
```

```bash
chmod +x check_servers.sh
./check_servers.sh
```

**Note:** Servers will show DOWN because they're example domains.

**✅ Success:** Anda faham loops dalam bash scripting

---

## Bahagian 6: Practical DevOps Scripts (30 minit)

### Langkah 1: Log Rotation Script

```bash
nano log_rotate.sh
```

**Content:**

```bash
#!/bin/bash
# Log rotation script

LOG_DIR="$HOME/app-logs"
ARCHIVE_DIR="$HOME/app-logs/archive"
DAYS_TO_KEEP=7

# Create directories
mkdir -p $LOG_DIR
mkdir -p $ARCHIVE_DIR

# Create sample log for demo
echo "Sample log entry" > $LOG_DIR/app.log

echo "Starting log rotation..."

# Find and compress logs older than 1 day
find $LOG_DIR -name "*.log" -type f -mtime +1 | while read LOG_FILE; do
    FILENAME=$(basename $LOG_FILE)
    TIMESTAMP=$(date +%Y%m%d_%H%M%S)
    
    echo "  Rotating $FILENAME..."
    gzip -c $LOG_FILE > $ARCHIVE_DIR/${FILENAME}_${TIMESTAMP}.gz
    
    # Clear original log
    > $LOG_FILE
done

# Delete old archives
echo "Cleaning old archives (older than $DAYS_TO_KEEP days)..."
find $ARCHIVE_DIR -name "*.gz" -type f -mtime +$DAYS_TO_KEEP -delete

echo "Log rotation completed!"
ls -lh $ARCHIVE_DIR/
```

```bash
chmod +x log_rotate.sh
./log_rotate.sh
```

### Langkah 2: Backup Script

```bash
nano backup.sh
```

**Content:**

```bash
#!/bin/bash
# Backup script with validation

# Configuration
SOURCE_DIR="$HOME/devops-scripts"
BACKUP_DIR="$HOME/backups"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/backup_${TIMESTAMP}.tar.gz"

echo "==================================="
echo "   BACKUP SCRIPT"
echo "==================================="

# Validate source directory exists
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory not found: $SOURCE_DIR"
    exit 1
fi

# Create backup directory
mkdir -p $BACKUP_DIR

# Create backup
echo "Creating backup..."
echo "  Source: $SOURCE_DIR"
echo "  Destination: $BACKUP_FILE"

tar -czf $BACKUP_FILE $SOURCE_DIR 2>/dev/null

# Check if backup successful
if [ $? -eq 0 ]; then
    echo "✓ Backup successful!"
    echo ""
    echo "Backup details:"
    ls -lh $BACKUP_FILE
    
    # Count files in backup
    FILE_COUNT=$(tar -tzf $BACKUP_FILE | wc -l)
    echo "Files backed up: $FILE_COUNT"
else
    echo "✗ Backup failed!"
    exit 1
fi

# Keep only last 5 backups
echo ""
echo "Cleaning old backups (keeping last 5)..."
ls -t $BACKUP_DIR/backup_*.tar.gz | tail -n +6 | xargs -r rm
echo "Remaining backups:"
ls -lh $BACKUP_DIR/

echo "==================================="
```

```bash
chmod +x backup.sh
./backup.sh
```

### Langkah 3: System Monitoring Script

```bash
nano monitor.sh
```

**Content:**

```bash
#!/bin/bash
# System monitoring script

LOG_FILE="$HOME/system_monitor.log"
DISK_THRESHOLD=80
MEMORY_THRESHOLD=80

# Function to log messages
log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a $LOG_FILE
}

# Function to send alert
send_alert() {
    local MESSAGE=$1
    log_message "ALERT: $MESSAGE"
    # In production, send to Slack/Email/SMS
}

log_message "Starting system monitoring..."

# Check disk usage
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $DISK_USAGE -ge $DISK_THRESHOLD ]; then
    send_alert "Disk usage is ${DISK_USAGE}% (threshold: ${DISK_THRESHOLD}%)"
else
    log_message "Disk usage OK: ${DISK_USAGE}%"
fi

# Check memory usage
MEMORY_USAGE=$(free | awk 'NR==2 {printf "%.0f", $3*100/$2}')
if [ $MEMORY_USAGE -ge $MEMORY_THRESHOLD ]; then
    send_alert "Memory usage is ${MEMORY_USAGE}% (threshold: ${MEMORY_THRESHOLD}%)"
else
    log_message "Memory usage OK: ${MEMORY_USAGE}%"
fi

# Check if SSH is running
if systemctl is-active --quiet sshd 2>/dev/null; then
    log_message "SSH service: Running"
else
    send_alert "SSH service is NOT running!"
fi

log_message "Monitoring check completed"
echo ""
echo "View log file:"
tail -10 $LOG_FILE
```

```bash
chmod +x monitor.sh
./monitor.sh
```

### Langkah 4: Deployment Menu Script

```bash
nano deploy_menu.sh
```

**Content:**

```bash
#!/bin/bash
# Deployment menu script

show_menu() {
    echo "==================================="
    echo "   DEPLOYMENT MENU"
    echo "==================================="
    echo "1. Deploy to Development"
    echo "2. Deploy to Staging"
    echo "3. Deploy to Production"
    echo "4. Rollback"
    echo "5. Check Status"
    echo "6. Exit"
    echo "==================================="
    echo -n "Select option [1-6]: "
}

deploy_dev() {
    echo "Deploying to Development..."
    sleep 2
    echo "✓ Deployment to Development completed!"
}

deploy_staging() {
    echo "Deploying to Staging..."
    sleep 2
    echo "✓ Deployment to Staging completed!"
}

deploy_prod() {
    echo "⚠ WARNING: You are about to deploy to PRODUCTION!"
    read -p "Are you sure? (yes/no): " CONFIRM
    if [ "$CONFIRM" == "yes" ]; then
        echo "Deploying to Production..."
        sleep 2
        echo "✓ Deployment to Production completed!"
    else
        echo "Deployment cancelled"
    fi
}

rollback() {
    echo "Rolling back deployment..."
    sleep 2
    echo "✓ Rollback completed!"
}

check_status() {
    echo "Checking deployment status..."
    echo "  Development: ✓ Running"
    echo "  Staging: ✓ Running"
    echo "  Production: ✓ Running"
}

# Main loop
while true; do
    show_menu
    read OPTION
    
    case $OPTION in
        1) deploy_dev ;;
        2) deploy_staging ;;
        3) deploy_prod ;;
        4) rollback ;;
        5) check_status ;;
        6) echo "Exiting..."; exit 0 ;;
        *) echo "Invalid option!" ;;
    esac
    
    echo ""
    read -p "Press Enter to continue..."
    clear
done
```

```bash
chmod +x deploy_menu.sh
./deploy_menu.sh
```

**Test:** Try berbagai options, then select 6 untuk exit.

**✅ Success:** Anda telah buat practical DevOps automation scripts!

---

## Bahagian 7: Final Exercise (20 minit)

### Exercise: Create DevOps Automation Script

**Task:** Buat script yang gabungkan semua yang anda belajar

```bash
nano devops_auto.sh
```

**Requirements:**

1. Show menu dengan 4 options:
   - Server info
   - Backup files
   - System health check
   - Exit

2. Each option should call different function

3. Use variables, conditionals, dan loops where appropriate

**Sample solution:**

```bash
#!/bin/bash
# DevOps Automation Script

BACKUP_DIR="$HOME/auto-backups"

show_menu() {
    echo "==================================="
    echo "   DEVOPS AUTOMATION"
    echo "==================================="
    echo "1. Server Information"
    echo "2. Backup Files"
    echo "3. System Health Check"
    echo "4. Exit"
    echo "==================================="
    echo -n "Select option [1-4]: "
}

server_info() {
    echo ""
    echo "=== SERVER INFORMATION ==="
    echo "Hostname: $(hostname)"
    echo "IP Address: $(hostname -I | awk '{print $1}')"
    echo "Uptime: $(uptime -p)"
    echo "Disk Usage: $(df -h / | awk 'NR==2 {print $5}')"
    echo "Memory Usage: $(free | awk 'NR==2 {printf "%.1f%%", $3*100/$2}')"
    echo "=========================="
}

backup_files() {
    echo ""
    read -p "Enter directory to backup: " SOURCE
    
    if [ ! -d "$SOURCE" ]; then
        echo "Error: Directory not found!"
        return 1
    fi
    
    mkdir -p $BACKUP_DIR
    TIMESTAMP=$(date +%Y%m%d_%H%M%S)
    BACKUP_FILE="$BACKUP_DIR/backup_${TIMESTAMP}.tar.gz"
    
    echo "Creating backup..."
    tar -czf $BACKUP_FILE $SOURCE 2>/dev/null
    
    if [ $? -eq 0 ]; then
        echo "✓ Backup successful: $BACKUP_FILE"
        ls -lh $BACKUP_FILE
    else
        echo "✗ Backup failed!"
    fi
}

health_check() {
    echo ""
    echo "=== HEALTH CHECK ==="
    
    # Disk
    DISK=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
    echo -n "Disk: ${DISK}% - "
    [ $DISK -gt 80 ] && echo "✗ CRITICAL" || echo "✓ OK"
    
    # Memory
    MEM=$(free | awk 'NR==2 {printf "%.0f", $3*100/$2}')
    echo -n "Memory: ${MEM}% - "
    [ $MEM -gt 80 ] && echo "✗ CRITICAL" || echo "✓ OK"
    
    echo "===================="
}

# Main
while true; do
    show_menu
    read OPTION
    
    case $OPTION in
        1) server_info ;;
        2) backup_files ;;
        3) health_check ;;
        4) echo "Goodbye!"; exit 0 ;;
        *) echo "Invalid option!" ;;
    esac
    
    echo ""
    read -p "Press Enter to continue..."
    clear
done
```

```bash
chmod +x devops_auto.sh
./devops_auto.sh
```

**Test semua features!**

**✅ Success:** Tahniah! Anda telah selesai Lab 22

---

## 🎓 Kesimpulan

Dalam lab ini anda telah belajar:

✅ Shell scripting basics dengan shebang  
✅ Variables (user-defined dan environment)  
✅ User input dengan `read`  
✅ Script arguments (`$1`, `$2`, etc.)  
✅ Conditionals (`if/elif/else`)  
✅ Loops (`for`, `while`)  
✅ Functions untuk code reuse  
✅ Real-world DevOps automation scripts  
✅ Error handling  
✅ Logging  

### Best Practices Learned

1. ✓ Always add shebang: `#!/bin/bash`
2. ✓ Use comments untuk documentation
3. ✓ Quote variables: `"$VAR"`
4. ✓ Check errors: `if [ $? -eq 0 ]`
5. ✓ Make scripts executable: `chmod +x script.sh`
6. ✓ Validate input sebelum process
7. ✓ Log important actions
8. ✓ Test scripts sebelum production

### Common Patterns untuk DevOps

**Error handling:**
```bash
if ! command; then
    echo "Error occurred"
    exit 1
fi
```

**Logging:**
```bash
log() {
    echo "[$(date)] $1" | tee -a /var/log/script.log
}
```

**Confirmation:**
```bash
read -p "Are you sure? (y/n): " CONFIRM
[ "$CONFIRM" != "y" ] && exit 0
```

### Script Template

```bash
#!/bin/bash
# Script description
# Author: Your name
# Date: Date

set -e  # Exit on error

# Configuration
VAR1="value1"
VAR2="value2"

# Functions
function_name() {
    # Function code
}

# Main
echo "Starting script..."
function_name
echo "Script completed!"
```

---

## ✅ Lab 22 Checklist

Pastikan anda boleh:

- [ ] Menulis shell script dengan shebang
- [ ] Menggunakan variables untuk store data
- [ ] Read user input dengan `read`
- [ ] Access script arguments (`$1`, `$2`)
- [ ] Implement if/else conditionals
- [ ] Write for loops untuk iterate lists
- [ ] Write while loops untuk conditions
- [ ] Create functions untuk code organization
- [ ] Handle errors gracefully
- [ ] Create menu-driven scripts
- [ ] Automate common DevOps tasks
- [ ] Test scripts sebelum production use

---

## 🎉 Tahniah!

Anda telah selesai ketiga-tiga sesi Linux Command & Shell Scripting!

**Skills yang anda ada:**
- ✓ Linux command line mastery
- ✓ File & directory management
- ✓ System monitoring
- ✓ Shell scripting automation
- ✓ DevOps best practices

**Praktis terus dengan:**
- Automate daily tasks
- Monitor system health
- Create deployment scripts
- Build backup automation
- Log management automation
