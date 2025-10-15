## LAB 17: Network Communication

### Langkah 1: Update Security Group untuk ICMP

1. Pergi ke "Security Groups" dalam dashboard VPC
2. Pilih `web-sg`
3. Pergi ke tab "Inbound rules"
4. Click "Edit inbound rules"
5. Click "Add rule"
6. Isikan maklumat berikut:
   - Type: All ICMP - IPv4
   - Source: 10.0.0.0/16
7. Click "Save rules"

> **Nota**: Rule ini membenarkan ping dari mana-mana instance dalam VPC (10.0.0.0/16)

### Langkah 2: Dapatkan Private IP Address

1. Pergi ke EC2 console
2. Pilih instance `private-server`
3. Copy **Private IPv4 address** (contoh: `10.0.0.123`)

### Langkah 3: Ping dari Public ke Private Server

1. Connect ke `public-server` menggunakan Session Manager
2. Dalam terminal, jalankan:

```bash
ping -c 4 10.0.0.123
```

**Ganti `10.0.0.123` dengan Private IP anda!**

Output yang diharapkan:

```
64 bytes from 10.0.0.123: icmp_seq=1 ttl=255 time=0.5 ms
64 bytes from 10.0.0.123: icmp_seq=2 ttl=255 time=0.4 ms
64 bytes from 10.0.0.123: icmp_seq=3 ttl=255 time=0.4 ms
64 bytes from 10.0.0.123: icmp_seq=4 ttl=255 time=0.4 ms
```

✅ Public server boleh berkomunikasi dengan private server!

### Langkah 4: Dapatkan Private IP dari Public Server

1. Pergi ke EC2 console
2. Pilih instance `public-server`
3. Copy **Private IPv4 address** (contoh: `10.0.2.45`)
