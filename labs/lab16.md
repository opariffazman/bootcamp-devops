## LAB 16: SSM Session Manager

### Langkah 1: Connect menggunakan Session Manager

1. Pergi ke EC2 console
2. Pilih instance `public-server`
2. Click "Connect"
3. Pilih tab "Session Manager"
4. Click "Connect"
5. Terminal akan terbuka di browser anda

✅ Berjaya connect tanpa SSH key!

### Langkah 2: Test Command di Session Manager

Dalam Session Manager terminal, cuba jalankan:

```bash
whoami
hostname
sudo systemctl status nginx
curl localhost
```

Tekan butang **q** untuk keluar dari status.

### Langkah 3: Cuba Connect ke Private Instance (Akan Gagal)

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

### Langkah 4: Semak Status di Systems Manager

1. Pergi ke AWS Systems Manager console
2. Click "Fleet Manager" di sidebar
3. Anda akan nampak hanya `public-server` dalam senarai managed instances
4. `private-server` tidak akan muncul

**Kelebihan SSM:**

- Tidak perlu SSH key
- Tidak perlu expose port 22
- Tidak perlu security group inbound rules
- Audit trail automatic di CloudTrail
- Session logging boleh enable
