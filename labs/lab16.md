## LAB 16: IAM Role untuk SSM Access

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

### Langkah 2: Buat Security Group untuk SSM

1. Pergi ke "Security Groups" dalam dashboard VPC
2. Click "Create security group"
3. Isikan maklumat berikut:
   - Security group name: `ssm-sg`
   - Description: `Allow SSM access to EC2`
   - VPC: `my-vpc`
4. **Inbound rules**: Jangan tambah apa-apa (leave empty)
5. **Outbound rules**: Pastikan ada rule berikut (default):
   - Type: All traffic
   - Destination: 0.0.0.0/0
6. Click "Create security group"

> **Nota**: Security group ini tidak memerlukan inbound rules kerana SSM menggunakan outbound connection ke AWS Systems Manager endpoint.

### Langkah 3: Launch EC2 Instance di Public Subnet

1. Pergi ke EC2 console
2. Click "Launch instance"
3. Isikan maklumat berikut:
   - Name: `public-ec2-ssm`
   - AMI: **Ubuntu Server 24.04 LTS**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
4. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `public-subnet`
   - Auto-assign public IP: Enable
   - Security group: Pilih existing `ssm-sg`
5. Dalam "Advanced details":
   - IAM instance profile: `EC2-SSM-Role`
6. Click "Launch instance"

### Langkah 4: Launch EC2 Instance di Private Subnet

1. Click "Launch instance" sekali lagi
2. Isikan maklumat berikut:
   - Name: `private-ec2-ssm`
   - AMI: **Ubuntu Server 24.04 LTS**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
3. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `private-subnet`
   - Auto-assign public IP: Disable
   - Security group: Pilih existing `ssm-sg`
4. Dalam "Advanced details":
   - IAM instance profile: `EC2-SSM-Role`
5. Click "Launch instance"

### Langkah 5: Connect ke Public EC2 menggunakan Session Manager

1. Pergi ke EC2 console
2. Pilih instance `public-ec2-ssm`
3. Tunggu hingga **Status check**: 2/2 checks passed
4. Click "Connect"
5. Pilih tab "Session Manager"
6. Click "Connect"
7. Terminal akan terbuka di browser anda

> **Nota**: Instance `public-ec2-ssm` akan berjaya connect kerana ia berada di public subnet dengan internet access melalui Internet Gateway.

### Langkah 6: Install Nginx di Public EC2

Dalam Session Manager terminal, jalankan command berikut:

```bash
# Update system
sudo apt update

# Install nginx
sudo apt install nginx -y

# Start nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx
```

Pastikan output menunjukkan:
```
Active: active (running)
```

Press **q** untuk keluar.

### Langkah 7: Test Nginx dari Localhost

Dalam Session Manager terminal:

```bash
curl localhost
```

Anda akan nampak banyak HTML code. Ini bermakna nginx berfungsi!

> **Nota**: Anda tidak boleh access dari browser kerana security group `ssm-sg` tidak membenarkan HTTP traffic dari internet. Ini demonstrasi perbezaan antara access dari dalam instance dan access dari luar.

### Langkah 8: Cuba Connect ke Private EC2 (Akan Gagal)

1. Pergi ke EC2 console
2. Pilih instance `private-ec2-ssm`
3. Tunggu hingga **Status check**: 2/2 checks passed
4. Click "Connect"
5. Pilih tab "Session Manager"
6. Anda akan dapat mesej: **"The instance you selected is not configured to use Session Manager"** atau button "Connect" akan disabled

> **Perbezaan**: Instance di private subnet tidak dapat connect ke AWS Systems Manager kerana:
> - Tiada Internet Gateway access
> - Tiada NAT Gateway (belum dibuat)
> - Tiada VPC Endpoints untuk SSM (alternatif untuk private subnet)

### Langkah 9: Semak Status di Systems Manager

1. Pergi ke AWS Systems Manager console
2. Click "Fleet Manager" di sidebar
3. Anda akan nampak hanya `public-ec2-ssm` dalam senarai managed instances
4. `private-ec2-ssm` tidak akan muncul kerana ia tidak dapat communicate dengan Systems Manager service

### Kesimpulan

- **IAM Role** dengan policy `AmazonSSMManagedInstanceCore` diperlukan untuk SSM access
- **SSM Session Manager** membolehkan connect ke EC2 tanpa SSH key dan tanpa expose port 22
- Instance di **public subnet** boleh connect ke SSM melalui Internet Gateway
- Instance di **private subnet** tidak boleh connect ke SSM tanpa NAT Gateway atau VPC Endpoints
- **Security Group** untuk SSM hanya memerlukan outbound rules (tidak perlu inbound rules)
