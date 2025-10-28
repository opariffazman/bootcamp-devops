## LAB 21: Operasi Fail

### Prerequisites

- Selesai [Lab 20](lab20.md)

## Bahagian 0: Connect ke EC2 Instance

### Langkah 1: Connect Menggunakan Session Manager

1. Pergi ke EC2 Console
2. Select instance anda
3. Click **Connect** > **Session Manager** > **Connect**
4. Switch to bash: `bash`

---

## Bahagian 1: Command touch

### Langkah 1: Fahami touch

**touch - Cipta fail atau update timestamp fail**

```bash
cd /var/www/html/
```

```bash
ls -l index.html
```

```bash
sudo touch index.html
```

```bash
ls -l index.html
```

---

## Bahagian 2: Command echo (Perintah output)

### Langkah 1: Fahami echo

**echo - output konten kepada terminal**

```bash
echo "Hello DevOps"
```

### Langkah 2: Kombinasi dengan Redirector

**Redirector `>` - Tulis konten dalam fail**

```bash
cd ~
echo "This is line 1" > test.txt
```

```bash
cat test.txt
```

```bash
echo "This is NEW line 1" > test.txt
```

```bash
cat test.txt
```

**Nota:** `>` akan buang konten lama dan tulis konten baru.

### Langkah 3: Redirector >> (Tambah)

**Redirector `>>` - Tambah konten kepada file**

```bash
echo "This is line 2" >> test.txt
echo "This is line 3" >> test.txt
```

```bash
cat test.txt
```

**Nota:** `>>` akan tambah konten baru tanpa buang konten sebelumnya.

### Langkah 4: Buat fail colors.txt dengan echo & redirector

```bash
cd ~
echo "red" >> colors.txt
echo "green" >> colors.txt
echo "blue" >> colors.txt
```

---

## Bahagian 3: Command cat (Perintah lihat teks)

### Langkah 1: Lihat kontent fail

**cat - concatenate konten**

```bash
cat colors.txt
```

---

## Bahagian 4: grep (Perintah carian teks)

### Langkah 1: Fahami grep

**grep - Cari teks dalam fail**

```bash
grep "red" colors.txt
```

```bash
grep "blue" colors.txt
```

### Langkah 2: Cari dalam fail html

```bash
grep "Version" /var/www/site1/html/index.html
```

`-i` - case-insensitive

```bash
cat /var/www/site1/html/index.html | grep -i "version"
```

```bash
grep "title" /var/www/site1/html/index.html
```

---

## Bahagian 5: Pipe | (Menghubungkan perintah)

### Langkah 1: Fahami Pipe

**Pipe `|` - Send output dari satu command ke command lain**

```bash
cat colors.txt | grep "red"
```

**Nota:** `|` ambil output dari command kiri, hantar ke command kanan.

### Langkah 2: cat dengan grep

```bash
# View file and search
cat /var/www/site1/html/index.html | grep "Version"
```

### Langkah 3: Practical Examples

```bash
# Test website and check version
curl localhost | grep "Version"
```

---

## Bahagian 6: Command tee (Perintah input & output)

### Langkah 1: Fahami tee

**tee - Tulis konten ke dalam fail dan tunjukkan serta merta di terminal**

```bash
echo "Test with redirector" > redirect.txt
cat redirect.txt
```

```bash
echo "Test with tee" | tee tee.txt
```

**Nota:** `tee` berguna bila nak save output DAN tengok sekali.

### Langkah 2: Guna tee jika perlu sudo

```bash
cd /var/www/site1/html

# akan gagal
sudo echo "test" > test-fail.txt

# akan lulus
echo "test" | sudo tee test-success.txt > /dev/null

# Verify
ls -l test-success.txt
```

---

## Bahagian 7: Command cp (Perintah salin fail)

### Langkah 1: Fahami cp

**cp - Menyalin fail menjadi fail berbeza**

```bash
cd /var/www/site1/html
```

```bash
sudo cp index.html index-backup.html
```

```bash
ls -l index*
```

### Langkah 2: Salin fail dengan menggunakan date

```bash
sudo cp index.html index-$(date +%Y%m%d).html
```

```bash
ls -l
```

### Langkah 3: Salin fail kepada folder berbeza

```bash
sudo mkdir -p /var/www/backups
```

```bash
sudo cp index.html /var/www/backups/
```

```bash
ls -l /var/www/backups/
```

---

## Bahagian 8: Command mv (Perintah alih)

### Langkah 1: Fahami mv

**mv - Mengalihkan file kepada nama baru**

```bash
sudo touch old-name.html
```

```bash
sudo mv old-name.html new-name.html
```

```bash
ls -l *name*.html
```

### Langkah 2: Mengalihkan fail ke tempat baru

```bash
sudo mkdir -p archive
```

```bash
sudo mv index-backup.html archive/
```

```bash
ls -l archive/
```

---

## Bahagian 9: Command find (Perintah cari fail atau directory)

### Langkah 1: Fahami find

**find - cari fail**

```bash
cd /var/www/site1/html
```

```bash
find . -name "index.html"
```

```bash
find . -name "*.html"
```

## Langkah 2: Cari file dan directory

```bash
find /var/www/site1/html -type f
```

```bash
find /var/www -type d
```

---

## Command Summary

### Redirectors & Pipes

| Symbol | Nama | Fungsi | Contoh |
|--------|------|--------|--------|
| `>` | Redirect (overwrite) | Write to file, replace old content | `echo "text" > file.txt` |
| `>>` | Redirect (append) | Add to file, keep old content | `echo "text" >> file.txt` |
| `\|` | Pipe | Send output to next command | `cat file \| grep "word"` |

### File Creation

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `touch file` | Create empty file or update timestamp | `touch test.txt` |
| `echo "text" > file` | Create file with text | `echo "hello" > test.txt` |
| `echo "text" >> file` | Update file with text | `echo "hello 2" >> test.txt` |
| `tee file` | Write and display | `echo "text" \| tee file.txt` |

### File Viewing

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `cat file` | Display file content | `cat index.html` |
| `cat file \| grep` | Search in file | `cat index.html \| grep Version` |

### File Operations

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `cp file backup` | Copy file | `cp index.html backup.html` |
| `mv old new` | Rename file | `mv old.html new.html` |
| `mv file dir/` | Move file | `mv file.txt backup/` |

### Search Commands

| Command | Fungsi | Contoh |
|---------|--------|--------|
| `grep "text" file` | Search text in file | `grep "Version" index.html` |
| `find . -name "file"` | Find file by name | `find . -name "*.html"` |
| `find . -type f` | Find files only | `find /var/www -type f` |

---
