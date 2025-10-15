## LAB 15: IAM Role dan Security Groups

### Langkah 1: Buat IAM Role untuk SSM

1. Pergi ke IAM console
2. Click "Roles" di sidebar
3. Click "Create role"
4. Pilih trusted entity type: **AWS service**
5. Pilih use case: **EC2**
6. Click "Next"
7. Dalam "Permissions policies", cari dan tick: `AmazonSSMManagedInstanceCore`
8. Click "Next"
9. Isikan Role name: `EC2-SSM-Role`
10. Click "Create role"

### Langkah 2: Buat Security Group untuk EC2

1. Pergi ke "Security Groups" dalam dashboard VPC
2. Click "Create security group"
3. Isikan maklumat berikut:
   - Security group name: `web-sg`
   - Description: `Allow HTTP traffic`
   - VPC: `my-vpc`
4. **Inbound rules**: Click "Add rule"
   - Type: HTTP
   - Port: 80
   - Source: 0.0.0.0/0
5. **Outbound rules**: Pastikan ada rule berikut (default):
   - Type: All traffic
   - Destination: 0.0.0.0/0
6. Click "Create security group"

### Langkah 3: Launch EC2 Instance di Public Subnet

1. Pergi ke EC2 console
2. Click "Launch instance"
3. Isikan maklumat berikut:
   - Name: `public-server`
   - AMI: **Amazon Linux 2023 AMI**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
4. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `public-subnet`
   - Auto-assign public IP: Enable
   - Security group: Pilih existing `web-sg`
5. Dalam "Advanced details":
   - IAM instance profile: `EC2-SSM-Role`
6. Click "Launch instance"

### Langkah 4: Launch EC2 Instance di Private Subnet

1. Click "Launch instance" sekali lagi
2. Isikan maklumat berikut:
   - Name: `private-server`
   - AMI: **Amazon Linux 2023 AMI**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
3. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `private-subnet`
   - Auto-assign public IP: Disable
   - Security group: Pilih existing `web-sg`
4. Dalam "Advanced details":
   - IAM instance profile: `EC2-SSM-Role`
5. Click "Launch instance"

### Langkah 5: Tunggu Instance Running

1. Pergi ke EC2 console
2. Tunggu hingga kedua-dua instance menunjukkan:
   - Instance state: **Running**
   - Status check: **2/2 checks passed**

### Langkah 6: Install Nginx di Public Instance

1. Pilih instance `public-server`
2. Click "Connect"
3. Pilih tab "EC2 Instance Connect"
4. Click "Connect"
5. Dalam terminal, jalankan commands:

```bash
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Langkah 7: Test Access ke Public Instance

1. Pergi ke EC2 console
2. Pilih instance `public-server`
3. Copy **Public IPv4 address**
4. Buka browser dan masukkan IP tersebut (contoh: `http://54.123.45.67`)
5. Anda akan nampak **"Welcome to nginx"** page

✅ Security group berfungsi dan membenarkan HTTP traffic!
