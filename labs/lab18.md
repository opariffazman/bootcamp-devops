## LAB 18: NAT Gateway (Optional Advanced)

> **⚠️ LAB OPTIONAL**: Lab ini adalah advanced dan tidak wajib untuk pemula. Anda boleh skip lab ini jika merasa overwhelmed.
>
> **💰 COST WARNING**: NAT Gateway dikenakan caj (~USD $0.045/hour + data transfer). Pastikan delete selepas lab!

### Apakah NAT Gateway?

NAT Gateway membolehkan instance di **private subnet** access internet untuk:
- Download updates dan packages
- Connect ke AWS services (SSM, S3, etc.)
- Outbound connection sahaja (tidak boleh receive inbound traffic dari internet)

### Langkah 1: Buat NAT Gateway

1. Pergi ke VPC console
2. Click "NAT Gateways" di sidebar
3. Click "Create NAT gateway"
4. Isikan maklumat berikut:
   - Name: `my-nat-gateway`
   - Subnet: `public-subnet` (NAT Gateway mesti di public subnet!)
   - Connectivity type: Public
   - Click "Allocate Elastic IP" (akan auto-create)
5. Click "Create NAT gateway"
6. Tunggu status bertukar dari **Pending** → **Available** (1-2 minit)

### Langkah 2: Update Private Route Table

1. Pergi ke "Route Tables" dalam VPC console
2. Pilih `private-rt`
3. Pergi ke tab "Routes"
4. Click "Edit routes"
5. Click "Add route"
6. Isikan:
   - Destination: `0.0.0.0/0`
   - Target: NAT Gateway → pilih `my-nat-gateway`
7. Click "Save changes"

> **Nota**: Sekarang private subnet ada route ke internet melalui NAT Gateway

### Langkah 3: Test Connection dari Private Server

1. Tunggu 2-3 minit untuk SSM agent register
2. Pergi ke EC2 console
3. Pilih instance `private-server`
4. Click "Connect"
5. Pilih tab "Session Manager"
6. Click "Connect"

✅ Sekarang boleh connect ke private server menggunakan SSM!

### Langkah 4: Test Internet Access

Dalam Session Manager untuk `private-server`:

```bash
# Test internet connectivity
ping -c 4 8.8.8.8

# Test DNS resolution
nslookup amazon.com

# Install package from internet
sudo dnf install htop -y
```

Semua commands akan berjaya kerana private server sekarang boleh access internet melalui NAT Gateway.

### Langkah 5: Test Inbound Access (Akan Gagal)

1. Pergi ke EC2 console
2. Pilih instance `private-server`
3. Perhatikan tiada **Public IPv4 address**

> **Penting**: Walaupun ada internet access, private server masih **tidak boleh** receive inbound connection dari internet. Ini berbeza dengan public subnet.

### Langkah 6: Verify dari Public Server

Connect ke `public-server` dan test ping:

```bash
# Ping private server (dapat Private IP dari EC2 console)
ping -c 4 10.0.0.123
```

✅ Communication antara subnet berfungsi!

### Langkah 7: Cleanup (PENTING!)

> **💰 NAT Gateway dikenakan caj setiap jam! Delete selepas selesai lab.**

1. Pergi ke VPC console
2. Click "NAT Gateways"
3. Pilih `my-nat-gateway`
4. Click "Actions" → "Delete NAT gateway"
5. Type `delete` untuk confirm
6. Click "Delete"

**Delete Elastic IP:**

1. Click "Elastic IPs" di sidebar
2. Pilih Elastic IP yang berkaitan dengan NAT Gateway
3. Click "Actions" → "Release Elastic IP addresses"
4. Click "Release"

**Update Route Table:**

1. Pergi ke "Route Tables"
2. Pilih `private-rt`
3. Tab "Routes" → "Edit routes"
4. Delete route `0.0.0.0/0` yang point ke NAT Gateway
5. Click "Save changes"

✅ Private server akan kembali ke keadaan asal (tiada internet access)

### Kesimpulan

**NAT Gateway:**

- Membolehkan private subnet access internet untuk outbound traffic
- Mesti diletakkan di **public subnet**
- Memerlukan Elastic IP address
- Instance di private subnet kekal private (tiada inbound access dari internet)
- **Dikenakan caj** - pastikan delete bila tidak guna!

**Use Cases:**

- Download updates dan security patches
- Access AWS services (S3, SSM, etc.)
- Install packages dari internet
- Application yang perlu outbound internet tapi tidak perlu public access

**Alternative:** VPC Endpoints (lebih murah untuk specific AWS services, akan cover di lab seterusnya jika berminat)
