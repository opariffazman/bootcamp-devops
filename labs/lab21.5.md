## LAB 21.5: Setup IAM User & AWS CLI untuk SSM

### Prerequisites

- Selesai [Lab 16](lab16.md) - SSM Session Manager
- Akaun AWS dengan access ke IAM Console
- Terminal atau Command Prompt di komputer local (Windows/Mac/Linux)

---

## Bahagian 1: Buat IAM User dengan Access Keys

### Langkah 1: Pergi ke IAM Console

1. Login ke AWS Console
2. Cari service **IAM** dalam search bar
3. Click **IAM** untuk masuk ke IAM Dashboard

### Langkah 2: Buat IAM User Baru

1. Dalam IAM Dashboard, click **Users** di sidebar kiri
2. Click button **Create user**
3. Masukkan **User name**: `bootcamp-user`
4. Click **Next**

### Langkah 3: Attach Permissions Policy

1. Pilih **Attach policies directly**
2. Dalam search box, cari dan tick policies berikut:
   - `AmazonSSMManagedInstanceCore` - untuk SSM access
   - `AmazonEC2ReadOnlyAccess` - untuk view EC2 instances
3. Click **Next**

### Langkah 4: Review dan Create User

1. Review maklumat user
2. Click **Create user**
3. User `bootcamp-user` akan muncul dalam users list

### Langkah 5: Create Access Key

1. Click pada user **bootcamp-user** yang baru dibuat
2. Click tab **Security credentials**
3. Scroll ke bahagian **Access keys**
4. Click **Create access key**
5. Pilih use case: **Command Line Interface (CLI)**
6. Tick checkbox **I understand the above recommendation and want to proceed to create an access key**
7. Click **Next**
8. (Optional) Masukkan description tag: `CLI access for bootcamp`
9. Click **Create access key**

### Langkah 6: Download atau Copy Access Keys

**PENTING**: Access key hanya ditunjukkan SEKALI sahaja!

1. Anda akan nampak:
   - **Access key ID** (contoh: `AKIAIOSFODNN7EXAMPLE`)
   - **Secret access key** (contoh: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`)

2. Pilih salah satu:
   - Click **Download .csv file** untuk simpan ke komputer
   - Atau copy kedua-dua keys ke tempat selamat (password manager)

3. Click **Done**

**⚠️ Security Best Practice:**
- JANGAN share access keys dengan orang lain
- JANGAN commit keys ke GitHub
- JANGAN simpan dalam plaintext files
- Guna password manager atau AWS Secrets Manager

---

## Bahagian 2: Install AWS CLI v2

### Untuk Linux (Ubuntu/Debian)

```bash
# Download AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# Install unzip jika belum ada
sudo apt install unzip -y

# Extract zip file
unzip awscliv2.zip

# Install AWS CLI
sudo ./aws/install

# Verify installation
aws --version
```

Expected output:
```
aws-cli/2.x.x Python/3.x.x Linux/x.x.x-x-generic exe/x86_64.ubuntu.xx
```

### Untuk MacOS

```bash
# Download AWS CLI v2
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"

# Install
sudo installer -pkg AWSCLIV2.pkg -target /

# Verify installation
aws --version
```

### Untuk Windows

1. Download installer: https://awscli.amazonaws.com/AWSCLIV2.msi
2. Run installer file `AWSCLIV2.msi`
3. Follow installation wizard
4. Open **Command Prompt** atau **PowerShell**
5. Verify installation:

```cmd
aws --version
```

---

## Bahagian 3: Configure AWS CLI

### Langkah 1: Run AWS Configure

```bash
aws configure
```

### Langkah 2: Masukkan Credentials

Anda akan diminta masukkan 4 values:

```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

**Penjelasan:**
- **Access Key ID**: Copy dari Bahagian 1
- **Secret Access Key**: Copy dari Bahagian 1
- **Region**: `ap-southeast-1` untuk Singapore (gunakan region tempat EC2 instance anda)
- **Output format**: `json` (atau pilih `text` atau `table`)

### Langkah 3: Verify Configuration

```bash
# Check configuration
aws configure list

# Test dengan list EC2 instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,Tags[?Key==`Name`].Value|[0]]' --output table
```

Expected output: Table dengan list EC2 instances anda

---

## Bahagian 4: Install Session Manager Plugin

### Untuk Linux (Ubuntu/Debian)

```bash
# Download Session Manager plugin
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"

# Install plugin
sudo dpkg -i session-manager-plugin.deb

# Verify installation
session-manager-plugin
```

Expected output:
```
The Session Manager plugin is installed successfully. Use the AWS CLI to start a session.
```

### Untuk MacOS

```bash
# Download Session Manager plugin
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac/sessionmanager-bundle.zip" -o "sessionmanager-bundle.zip"

# Extract
unzip sessionmanager-bundle.zip

# Install
sudo ./sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin

# Verify installation
session-manager-plugin
```

### Untuk Windows

1. Download installer: https://s3.amazonaws.com/session-manager-downloads/plugin/latest/windows/SessionManagerPluginSetup.exe
2. Run installer `SessionManagerPluginSetup.exe`
3. Follow installation wizard
4. Open **Command Prompt** baru
5. Verify installation:

```cmd
session-manager-plugin
```

---

## Bahagian 5: Connect ke EC2 menggunakan SSM dari CLI

### Langkah 1: Dapatkan Instance ID

```bash
# List semua EC2 instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,Tags[?Key==`Name`].Value|[0],State.Name]' --output table
```

Expected output:
```
-------------------------------------------------------
|              DescribeInstances                     |
+----------------------+------------------+-----------+
|  i-0123456789abcdef  |  public-server   |  running |
|  i-0fedcba987654321  |  private-server  |  running |
+----------------------+------------------+-----------+
```

Copy **Instance ID** yang anda nak connect (contoh: `i-0123456789abcdef`)

### Langkah 2: Start SSM Session

```bash
# Connect ke EC2 instance
aws ssm start-session --target i-0123456789abcdef
```

**Gantikan `i-0123456789abcdef` dengan Instance ID sebenar anda.**

Expected output:
```
Starting session with SessionId: bootcamp-user-0a1b2c3d4e5f6g7h8

sh-4.2$
```

✅ Anda sekarang connect ke EC2 instance dari terminal local!

### Langkah 3: Switch ke Bash Shell

```bash
# Switch to bash untuk pengalaman lebih baik
bash

# Check current directory
pwd

# Check instance hostname
hostname

# Check instance metadata
curl -s http://169.254.169.254/latest/meta-data/instance-id
```

### Langkah 4: Keluar dari Session

```bash
# Exit dari session
exit
```

---

## Bahagian 6: Advanced SSM Commands

### Langkah 1: Run Command Without Interactive Session

```bash
# Run single command pada EC2 instance
aws ssm send-command \
    --instance-ids "i-0123456789abcdef" \
    --document-name "AWS-RunShellScript" \
    --parameters 'commands=["df -h","uptime"]' \
    --output text
```

### Langkah 2: List Active Sessions

```bash
# Check active SSM sessions
aws ssm describe-sessions --state Active
```

### Langkah 3: Terminate Session

```bash
# Dapatkan Session ID
aws ssm describe-sessions --state Active --output table

# Terminate specific session
aws ssm terminate-session --session-id bootcamp-user-0a1b2c3d4e5f6g7h8
```

---

## Troubleshooting

### Problem: "TargetNotConnected" Error

**Error message:**
```
An error occurred (TargetNotConnected) when calling the StartSession operation
```

**Sebab:**
- EC2 instance tidak ada SSM agent running
- Instance tidak ada IAM role dengan SSM permissions
- Instance dalam private subnet tanpa NAT Gateway atau VPC Endpoints

**Penyelesaian:**
1. Check instance ada IAM role: `AmazonSSMManagedInstanceCore`
2. Check SSM agent status:
   ```bash
   # Dari dalam instance (via console)
   sudo systemctl status amazon-ssm-agent
   ```
3. Rujuk [Lab 15](lab15.md) untuk setup IAM role
4. Rujuk [Lab 18](lab18.md) untuk NAT Gateway (jika private subnet)

### Problem: "AccessDeniedException" Error

**Error message:**
```
An error occurred (AccessDeniedException) when calling the StartSession operation
```

**Penyelesaian:**
1. Check IAM user ada permission `AmazonSSMManagedInstanceCore`
2. Verify AWS credentials configured dengan betul:
   ```bash
   aws configure list
   aws sts get-caller-identity
   ```

### Problem: AWS CLI Command Not Found

**Penyelesaian:**
1. Verify installation:
   ```bash
   which aws
   aws --version
   ```
2. Restart terminal
3. Check PATH environment variable

---

## Command Summary

### IAM Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `aws iam list-users` | List semua IAM users | `aws iam list-users` |
| `aws iam get-user` | Check current IAM user | `aws iam get-user` |
| `aws sts get-caller-identity` | Check credentials identity | `aws sts get-caller-identity` |

### EC2 Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `aws ec2 describe-instances` | List EC2 instances | `aws ec2 describe-instances --output table` |
| `aws ec2 describe-instance-status` | Check instance status | `aws ec2 describe-instance-status --instance-ids i-xxx` |

### SSM Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `aws ssm start-session` | Start interactive session | `aws ssm start-session --target i-xxx` |
| `aws ssm describe-sessions` | List active sessions | `aws ssm describe-sessions --state Active` |
| `aws ssm terminate-session` | End session | `aws ssm terminate-session --session-id xxx` |
| `aws ssm send-command` | Run command remotely | `aws ssm send-command --instance-ids i-xxx --document-name "AWS-RunShellScript"` |

### AWS Configure Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `aws configure` | Setup credentials | `aws configure` |
| `aws configure list` | Show configuration | `aws configure list` |
| `aws configure get` | Get specific value | `aws configure get region` |
| `aws configure set` | Set specific value | `aws configure set region ap-southeast-1` |

---

## Best Practices

### 1. Security

- ✅ Guna IAM users dengan least privilege permissions
- ✅ Enable MFA untuk IAM users
- ✅ Rotate access keys regularly (every 90 days)
- ✅ Delete unused access keys
- ❌ JANGAN hardcode credentials dalam scripts
- ❌ JANGAN share access keys

### 2. CLI Usage

- ✅ Guna named profiles untuk multiple AWS accounts:
  ```bash
  aws configure --profile production
  aws ssm start-session --target i-xxx --profile production
  ```
- ✅ Guna environment variables untuk temporary credentials
- ✅ Enable CloudTrail untuk audit API calls

### 3. SSM Best Practices

- ✅ Guna SSM Session Manager instead of SSH (more secure)
- ✅ Enable session logging ke S3 atau CloudWatch Logs
- ✅ Setup session timeout limits
- ✅ Use Systems Manager Run Command untuk automation

---

## Next Steps

Selepas selesai lab ini, anda boleh:

1. **Automation**: Guna SSM Run Command untuk execute scripts pada multiple instances
2. **Parameter Store**: Simpan configuration dan secrets dalam SSM Parameter Store
3. **Session Logging**: Enable session recording untuk compliance
4. **Port Forwarding**: Setup port forwarding menggunakan SSM untuk access private services

---

## Cleanup (Optional)

Jika nak cleanup resources selepas lab:

```bash
# Delete access key (perlu Instance ID)
aws iam delete-access-key --access-key-id AKIAIOSFODNN7EXAMPLE --user-name bootcamp-user

# Delete IAM user (kena delete access keys dulu)
aws iam delete-user --user-name bootcamp-user

# Uninstall AWS CLI (Linux/Mac)
sudo rm -rf /usr/local/aws-cli
sudo rm /usr/local/bin/aws

# Uninstall Session Manager Plugin (Linux)
sudo dpkg -r session-manager-plugin
```

---

**✅ Lab 21.5 Selesai!**

Anda sekarang boleh:
- Buat IAM user dengan access keys
- Install AWS CLI v2 dan Session Manager plugin
- Configure AWS credentials
- Connect ke EC2 instances menggunakan SSM dari terminal local
- Run remote commands pada EC2 instances

Rujuk [Lab 22](lab22.md) untuk topik seterusnya.
