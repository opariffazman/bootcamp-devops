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

### Langkah 2: Attach IAM Role ke Public Instance

1. Pergi ke EC2 console
2. Pilih instance `public-web-server` (dari lab sebelumnya)
3. Click "Actions" → "Security" → "Modify IAM role"
4. Pilih IAM role: `EC2-SSM-Role`
5. Click "Update IAM role"

> **Nota**: Tunggu 2-3 minit untuk instance register dengan Systems Manager service.

### Langkah 3: Connect menggunakan Session Manager

1. Pilih instance `public-web-server`
2. Click "Connect"
3. Pilih tab "Session Manager"
4. Click "Connect"
5. Terminal akan terbuka di browser anda

✅ Berjaya connect tanpa SSH key!

### Langkah 4: Test Command di Session Manager

Dalam Session Manager terminal, cuba jalankan:

```bash
# Check user
whoami

# Check hostname
hostname

# Check nginx status
sudo systemctl status nginx

# Test nginx
curl localhost
```

Tekan butang **q** untuk keluar dari status.

### Langkah 5: Attach IAM Role ke Private Instance

1. Pergi ke EC2 console
2. Pilih instance `private-web-server`
3. Click "Actions" → "Security" → "Modify IAM role"
4. Pilih IAM role: `EC2-SSM-Role`
5. Click "Update IAM role"

### Langkah 6: Cuba Connect ke Private Instance (Akan Gagal)

1. Tunggu 2-3 minit
2. Pilih instance `private-web-server`
3. Click "Connect"
4. Pilih tab "Session Manager"
5. Button "Connect" akan **disabled** atau dapat error message

> **Sebab Gagal**: Walaupun instance ada IAM role, ia tidak dapat register dengan Systems Manager kerana:
>
> - Tiada Internet Gateway access
> - Tiada NAT Gateway
> - Tiada VPC Endpoints untuk SSM

### Langkah 7: Semak Status di Systems Manager

1. Pergi ke AWS Systems Manager console
2. Click "Fleet Manager" di sidebar
3. Anda akan nampak hanya `public-web-server` dalam senarai managed instances
4. `private-web-server` tidak akan muncul

**Kelebihan SSM:**

- Tidak perlu SSH key
- Tidak perlu expose port 22
- Tidak perlu security group inbound rules
- Audit trail automatic di CloudTrail
- Session logging boleh enable
