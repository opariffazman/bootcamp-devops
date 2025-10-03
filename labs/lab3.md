## LAB 3: CREATE BRANCH & ADD BIODATA

### 🎯 Objektif

Buat branch sendiri dan tambah fail biodata dengan maklumat peribadi.

### 📝 Steps

#### Step 1: Create Feature Branch

**Naming convention:** `add-[nama-anda]`

```bash
# Example jika nama anda "Ahmad"
git checkout -b add-ahmad
```

**Gantikan `ahmad` dengan nama anda (lowercase, no spaces)!**

**Verify branch:**

```bash
git branch
```

**Expected output:**

```
* add-ahmad
  main
```

Tanda `*` menunjukkan branch aktif sekarang.

---

#### Step 2: Create Biodata File

1. **Navigate to peserta folder:**

```bash
cd peserta
```

2. **Create file dengan nama anda menggunakan salah satu editor pilihan anda:**

```bash
code nama-anda.md    # VSCode
notepad nama-anda.md # Windows Notepad
```

**Gantikan `nama-anda` dengan nama anda (lowercase, hyphen for spaces)!**

**Contoh:**

- Ahmad Bin Ali → `ahmad-ali.md`
- Siti Nurhaliza → `siti-nurhaliza.md`
- John Doe → `john-doe.md`

---

#### Step 3: Add Biodata Content

Copy template ni dan edit dengan maklumat anda:

```markdown
# Biodata Peserta

## Maklumat Peribadi
- **Nama Penuh:** [Nama anda]

## Pekerjaan
- **Jawatan Semasa:** [Job title anda]

## Hobi
- 🎯 [Hobi 1]

## Fun Fact
> [Satu perkara menarik tentang anda]

## Hubungi Saya
- 📧 Email: [email anda - optional]
- 🔗 LinkedIn: [LinkedIn profile - optional]
- 🐙 GitHub: [GitHub username]
```

---

#### Step 4: Save & Verify File

1. **Save file**

2. **Verify file created:**

```bash
# Check file exists
ls

# Preview content
cat nama-anda.md
```

3. **Return to root directory:**

```bash
cd ..
# Sekarang anda dalam bootcamp-devops/ root
```

---

#### Step 5: Stage & Commit Changes

**Stage file:**

```bash
git add peserta/nama-anda.md

# Atau stage semua
git add .
```

**Verify staged:**

```bash
git status
```

**Expected output:**

```
On branch add-ahmad
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   peserta/ahmad.md
```

**Commit dengan mesej yang clear:**

```bash
git commit -m "docs: add biodata for [Nama Anda]"
```

**Example:**

```bash
git commit -m "docs: add biodata for Ahmad Abdullah"
```

---

#### Step 6: Verify Commit

```bash
# Check commit history
git log --oneline
```

---

### ✅ Lab 3 Checklist

- [ ] Created branch `add-[nama]`
- [ ] Created file `peserta/[nama].md`
- [ ] Filled biodata dengan maklumat lengkap
- [ ] File staged with `git add`
- [ ] Changes committed dengan clear message
- [ ] Verified commit dengan `git log`
