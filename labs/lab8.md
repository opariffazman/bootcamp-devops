## LAB 8: INSTALL NGINX

Semua command di bawah perlu run **dalam SSH session**.

### Langkah 1: Update System

```bash
sudo apt update
```

Tunggu selesai (30 saat - 1 minit).

### Langkah 2: Install Nginx

```bash
sudo apt install nginx -y
```

Tunggu installation complete (1-2 minit).

### Langkah 3: Start Nginx

```bash
# Start service
sudo systemctl start nginx

# Enable auto-start
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx
```

Pastikan output show:
```
Active: active (running)
```

Press **q** untuk keluar.

✅ Nginx berjalan!

### Langkah 4: Configure Firewall

```bash
# Allow port 80 (HTTP)
sudo ufw allow 80/tcp

# Allow port 22 (SSH)
sudo ufw allow 22/tcp

# Enable firewall
sudo ufw enable
```

Bila ditanya:
```
Command may disrupt existing ssh connections. Proceed (y|n)?
```

Type: **y** dan tekan Enter

### Langkah 5: Test Nginx

```bash
curl localhost
```

Anda akan nampak banyak HTML code. Ini bermakna Nginx berfungsi!
