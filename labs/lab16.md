## LAB 16: SSM Session Manager

### Langkah 1: Connect menggunakan Session Manager

1. Pergi ke EC2 console
2. Pilih instance `public-server`
2. Click "Connect"
3. Pilih tab "Session Manager"
4. Click "Connect"
5. Terminal akan terbuka di browser anda

✅ Berjaya connect tanpa SSH key!

### Langkah 2: Install Nginx menggunakan SSM

Dalam Session Manager terminal, jalankan commands:

```bash
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Langkah 3: Test Nginx

Dalam Session Manager terminal:

```bash
curl localhost
```

Anda akan nampak HTML output dari nginx.

### Langkah 4: Test Access dari Browser

1. Pergi ke EC2 console
2. Pilih instance `public-server`
3. Copy **Public IPv4 address**
4. Buka browser dan masukkan IP tersebut (contoh: `http://54.123.45.67`)
5. Anda akan nampak **"Welcome to nginx"** page

✅ Berjaya install dan access nginx tanpa SSH!

### Langkah 5: Cuba Connect ke Private Instance (Akan Gagal)

1. Pergi ke EC2 console
2. Pilih instance `private-server`
3. Click "Connect"
4. Pilih tab "Session Manager"
5. Button "Connect" akan **disabled** atau dapat error message

> **Sebab Gagal**: Walaupun instance ada IAM role, ia tidak dapat register dengan Systems Manager kerana:
>
> - Tiada Internet Gateway access
> - Tiada NAT Gateway
> - Tiada VPC Endpoints untuk SSM

**Kelebihan SSM:**

- Tidak perlu SSH key
- Tidak perlu expose port 22
- Tidak perlu security group inbound rules
- Audit trail automatic di CloudTrail
- Session logging boleh enable
