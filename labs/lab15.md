## LAB 15: Security Groups

### Langkah 1: Buat Security Group untuk EC2

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

### Langkah 2: Launch EC2 Instance di Public Subnet

1. Pergi ke EC2 console
2. Click "Launch instance"
3. Isikan maklumat berikut:
   - Name: `public-web-server`
   - AMI: **Ubuntu Server 24.04 LTS**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
4. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `public-subnet`
   - Auto-assign public IP: Enable
   - Security group: Pilih existing `web-sg`
5. Click "Launch instance"

### Langkah 3: Launch EC2 Instance di Private Subnet

1. Click "Launch instance" sekali lagi
2. Isikan maklumat berikut:
   - Name: `private-web-server`
   - AMI: **Ubuntu Server 24.04 LTS**
   - Architecture: **64-bit (x86)**
   - Instance type: t3.micro
   - **Key pair**: Proceed without a key pair
3. Dalam "Network settings":
   - VPC: `my-vpc`
   - Subnet: `private-subnet`
   - Auto-assign public IP: Disable
   - Security group: Pilih existing `web-sg`
4. Click "Launch instance"

### Langkah 4: Tunggu Instance Running

1. Pergi ke EC2 console
2. Tunggu hingga kedua-dua instance menunjukkan:
   - Instance state: **Running**
   - Status check: **2/2 checks passed**

### Langkah 5: Test Access ke Public Instance

1. Pilih instance `public-web-server`
2. Copy **Public IPv4 address**
3. Buka browser dan masukkan IP tersebut (contoh: `http://54.123.45.67`)

> **Nota**: Page akan timeout kerana nginx belum diinstall. Tetapi connection tidak ditolak (rejected) - ini bermakna security group berfungsi dan membenarkan traffic port 80.

### Langkah 6: Test Access ke Private Instance

1. Pilih instance `private-web-server`
2. Perhatikan bahawa tiada **Public IPv4 address**

> **Kesimpulan**: Instance di private subnet tidak boleh diakses dari internet walaupun security group membenarkan HTTP traffic.

### Langkah 7: Update Security Group (Optional Demo)

1. Pergi ke "Security Groups" dalam dashboard VPC
2. Pilih `web-sg`
3. Pergi ke tab "Inbound rules"
4. Click "Edit inbound rules"
5. **Delete** rule HTTP
6. Click "Save rules"
7. Cuba access `public-web-server` dari browser
8. Connection akan **ditolak dengan segera** (connection refused/reset)

> **Perbezaan timeout vs refused**:
> - **Timeout**: Security group membenarkan traffic, tetapi tiada application listening
> - **Refused/Reset**: Security group menolak traffic

### Kesimpulan

- **Security Group** bertindak sebagai firewall untuk EC2 instance
- **Inbound rules** mengawal traffic yang masuk ke instance
- **Outbound rules** mengawal traffic yang keluar dari instance
- Instance di **public subnet** boleh diakses dari internet jika security group membenarkan
- Instance di **private subnet** tidak boleh diakses dari internet walaupun security group membenarkan
