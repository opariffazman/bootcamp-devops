## LAB 7: CONNECT VIA SSH

### Langkah 1: Dapatkan Public IP

1. Dalam Instances page
2. Click pada instance anda
3. Copy **Public IPv4 address**
   - Contoh: `54.123.45.67`

### Langkah 2: Connect dari Laptop

**Mac/Linux - buka Terminal:**
```bash
cd ~/.ssh
ssh -i my-key.pem ubuntu@54.123.45.67
```

**Windows - buka PowerShell:**
```powershell
cd $HOME\.ssh
ssh -i my-key.pem ubuntu@54.123.45.67
```

**Ganti `54.123.45.67` dengan IP anda!**

### Langkah 3: Confirm Connection

Bila first time connect, akan tanya:
```
Are you sure you want to continue connecting (yes/no)?
```

Type: **yes** dan tekan Enter

### Langkah 4: Verify

Anda sekarang dalam server! Terminal show:
```
ubuntu@ip-172-31-x-x:~$
```

✅ Berjaya masuk!

---
