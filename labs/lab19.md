## LAB 19: VPC Endpoints (Optional Advanced)

> **⚠️ LAB OPTIONAL**: Lab ini adalah advanced dan tidak wajib untuk pemula. Anda boleh skip lab ini jika merasa overwhelmed.
>
> **💰 COST NOTE**: VPC Endpoints untuk SSM tidak dikenakan caj hourly, hanya data transfer. Lebih murah dari NAT Gateway untuk access AWS services sahaja.

### Apakah VPC Endpoints?

VPC Endpoints membolehkan private subnet connect ke AWS services **tanpa internet access**:
- Lebih secure (traffic tidak keluar dari AWS network)
- Lebih murah untuk access AWS services berbanding NAT Gateway
- Tidak perlu NAT Gateway atau Internet Gateway

**Use case:** Private server perlu access SSM tapi tidak perlu internet access.

### Langkah 1: Buat VPC Endpoint untuk SSM

1. Pergi ke VPC console
2. Click "Endpoints" di sidebar
3. Click "Create endpoint"
4. Isikan maklumat berikut:
   - Name: `ssm-endpoint`
   - Service category: AWS services
   - Services: Cari dan pilih `com.amazonaws.ap-southeast-1.ssm`
   - VPC: `my-vpc`
   - Subnets: Tick `private-subnet`
   - Security groups: Pilih `web-sg`
5. Click "Create endpoint"

### Langkah 2: Buat Endpoint untuk SSM Messages

1. Click "Create endpoint"
2. Isikan:
   - Name: `ssmmessages-endpoint`
   - Service category: AWS services
   - Services: Cari dan pilih `com.amazonaws.ap-southeast-1.ssmmessages`
   - VPC: `my-vpc`
   - Subnets: Tick `private-subnet`
   - Security groups: Pilih `web-sg`
3. Click "Create endpoint"

### Langkah 3: Buat Endpoint untuk EC2 Messages

1. Click "Create endpoint"
2. Isikan:
   - Name: `ec2messages-endpoint`
   - Service category: AWS services
   - Services: Cari dan pilih `com.amazonaws.ap-southeast-1.ec2messages`
   - VPC: `my-vpc`
   - Subnets: Tick `private-subnet`
   - Security groups: Pilih `web-sg`
3. Click "Create endpoint"

### Langkah 4: Update Security Group untuk HTTPS

1. Pergi ke "Security Groups"
2. Pilih `web-sg`
3. Tab "Inbound rules" → "Edit inbound rules"
4. Click "Add rule"
5. Isikan:
   - Type: HTTPS
   - Port: 443
   - Source: 10.0.0.0/16
6. Click "Save rules"

> **Nota**: VPC Endpoints perlu HTTPS (port 443) untuk communication

### Langkah 5: Tunggu Endpoints Available

1. Pergi ke "Endpoints"
2. Tunggu hingga semua 3 endpoints status: **Available** (2-3 minit)

### Langkah 6: Test SSM Connection

1. Tunggu 2-3 minit untuk SSM agent register
2. Pergi ke EC2 console
3. Pilih instance `private-server`
4. Click "Connect"
5. Pilih tab "Session Manager"
6. Click "Connect"

✅ Sekarang boleh connect ke private server menggunakan SSM melalui VPC Endpoints!

### Langkah 7: Verify No Internet Access

Dalam Session Manager untuk `private-server`:

```bash
# Test internet connectivity (akan gagal)
ping -c 4 8.8.8.8

# Test cannot download from internet (akan timeout/gagal)
curl https://google.com
```

> **Penting**: Private server boleh access SSM tapi **tidak boleh** access internet. Ini lebih secure berbanding NAT Gateway.

### Langkah 8: Test AWS Service Access

Dalam Session Manager:

```bash
# Check SSM agent status
sudo systemctl status amazon-ssm-agent

# List AWS region (jika AWS CLI installed)
aws ec2 describe-regions --output table
```

SSM berfungsi dengan sempurna tanpa internet access!

### Langkah 9: Cleanup (Optional)

> **Nota**: VPC Endpoints boleh dibiarkan kerana tidak ada hourly charge. Tapi jika nak delete:

1. Pergi ke "Endpoints"
2. Pilih endpoint
3. Click "Actions" → "Delete VPC endpoints"
4. Type `delete` untuk confirm
5. Repeat untuk semua 3 endpoints

### Perbandingan: NAT Gateway vs VPC Endpoints

| Feature | NAT Gateway | VPC Endpoints |
|---------|-------------|---------------|
| **Cost** | ~$0.045/hour + data | Data transfer sahaja |
| **Internet Access** | ✅ Yes | ❌ No |
| **AWS Services Access** | ✅ Yes | ✅ Yes (specific services) |
| **Security** | Good | Better (traffic stays in AWS) |
| **Setup Complexity** | Simple (1 resource) | Medium (per service) |
| **Use Case** | Need internet + AWS services | Only AWS services |

### Kesimpulan

**VPC Endpoints:**

- Lebih secure dari NAT Gateway (traffic tidak keluar AWS network)
- Lebih murah untuk access AWS services sahaja
- Tidak perlu Elastic IP atau NAT Gateway
- Private subnet kekal fully isolated dari internet
- Perlu create endpoint untuk setiap AWS service

**Bila guna VPC Endpoints:**

- Application hanya perlu access AWS services (SSM, S3, DynamoDB, etc.)
- High security requirement
- Cost optimization untuk AWS services

**Bila guna NAT Gateway:**

- Perlu download dari internet (packages, updates, etc.)
- Perlu access third-party APIs
- Perlu outbound internet access

**Best Practice:** Combine both! 
- VPC Endpoints untuk AWS services (cheaper, secure)
- NAT Gateway untuk internet access (bila perlu)
