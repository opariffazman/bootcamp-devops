## LAB 5: LAUNCH EC2 INSTANCE

### Langkah 1: Pergi ke EC2

1. Search bar (atas) → type **"EC2"**
2. Click **EC2**
3. Pastikan region: **Singapore** (top right)

### Langkah 2: Launch Instance

1. Click **"Launch instance"** (button oren)

2. **Name:**
   ```
   my-web-server
   ```

3. **Application and OS Images:**
   - Pilih **Ubuntu**
   - **Ubuntu Server 24.04 LTS**
   - Architecture: **64-bit (x86)**

4. **Instance type:**
   - Pilih **t3.micro** (atau t2.micro)
   - Pastikan ada label "Free tier eligible"

### Langkah 3: Key Pair

1. Click **"Create new key pair"**

2. Settings:
   ```
   Key pair name: my-key
   Key pair type: RSA
   Private key file format: .pem
   ```

3. Click **"Create key pair"**
4. File akan auto-download
5. **PENTING: Simpan file ini!**

**Untuk Mac/Linux, buka Terminal:**
```bash
# Pindah ke folder .ssh
mkdir -p ~/.ssh
mv ~/Downloads/my-key.pem ~/.ssh/

# Set permissions
chmod 400 ~/.ssh/my-key.pem
```

**Untuk Windows, buka PowerShell:**
```powershell
# Pindah ke folder .ssh
New-Item -ItemType Directory -Force -Path $HOME\.ssh
Move-Item $HOME\Downloads\my-key.pem $HOME\.ssh\
```

### Langkah 4: Network Settings

1. Click **"Edit"** pada Network settings

2. **Auto-assign public IP:** Pilih **Enable**

3. **Firewall (Security groups):**
   - Pilih **"Create security group"**
   - Name: `web-server-sg`
   - Description: `Allow SSH and HTTP`

4. **Security group rules:**

   **Rule 1 (SSH) - sudah ada:**
   ```
   Type: SSH
   Port: 22
   Source: My IP
   ```

   **Rule 2 (HTTP) - perlu tambah:**
   - Click **"Add security group rule"**
   ```
   Type: HTTP
   Port: 80
   Source: Anywhere (0.0.0.0/0)
   ```

### Langkah 5: Storage

Biarkan default:
```
8 GiB gp3
Delete on termination: Yes
```

### Langkah 6: Launch

1. Review summary (panel kanan)
2. Click **"Launch instance"**
3. Tunggu 1-2 minit
4. Click **"View all instances"**
5. Tunggu hingga:
   - Instance state: **Running**
   - Status check: **2/2 checks passed**
