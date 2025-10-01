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

2. **Create file dengan nama anda:**

```bash
# Guna text editor atau command line
# Example: nano, vim, notepad, atau VSCode

# Cara 1: Guna echo (untuk simple)
touch nama-anda.md

# Cara 2: Buka dengan editor
code nama-anda.md    # VSCode
nano nama-anda.md    # Nano
vim nama-anda.md     # Vim
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

#### Contoh Biodata (Reference)

```markdown
# Biodata Peserta

## Maklumat Peribadi
- **Nama Penuh:** Ahmad Bin Abdullah

## Pekerjaan
- **Jawatan Semasa:** System Administrator

## Hobi
- 🎨 Gaming (strategy games)

## Fun Fact
> Saya pernah accidentally delete production database, tapi nasib baik ada backup! 😅 Dari itu saya jadi paranoid dengan backups.

## Hubungi Saya
- 📧 Email: ahmad.abdullah@email.com
- 🔗 LinkedIn: linkedin.com/in/ahmad-abdullah
- 🐙 GitHub: ahmadcodes
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

```bash
# Check status
git status
```

**Expected output:**
```
On branch add-ahmad
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        peserta/ahmad.md

nothing added to commit but untracked files present
```

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

**Expected output:**
```
[add-ahmad 1a2b3c4] docs: add biodata for Ahmad Abdullah
 1 file changed, 45 insertions(+)
 create mode 100644 peserta/ahmad.md
```

---

#### Step 6: Verify Commit

```bash
# Check commit history
git log --oneline

# Check specific commit
git show HEAD
```

---

### ✅ Lab 3 Checklist

- [ ] Created branch `add-[nama]`
- [ ] Created file `peserta/[nama].md`
- [ ] Filled biodata dengan maklumat lengkap
- [ ] File staged with `git add`
- [ ] Changes committed dengan clear message
- [ ] Verified commit dengan `git log`
