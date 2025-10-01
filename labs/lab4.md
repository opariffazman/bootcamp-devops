## LAB 4: PUSH BRANCH & CREATE PULL REQUEST

### 🎯 Objektif
Push branch anda ke GitHub dan buat Pull Request untuk merge ke repository asal.

### 📝 Steps

#### Step 1: Push Branch to YOUR Fork

```bash
git push -u origin add-[nama-anda]
```

**Example:**
```bash
git push -u origin add-ahmad
```

**Penjelasan flags:**
- `-u` = set upstream (tracking relationship)
- `origin` = push ke YOUR fork
- `add-ahmad` = nama branch

**Expected output:**
```
remote: Create a pull request for 'add-ahmad' on GitHub by visiting:
remote:      https://github.com/your-username/bootcamp-devops/pull/new/add-ahmad
remote:
To github.com:your-username/bootcamp-devops.git
 * [new branch]      add-ahmad -> add-ahmad
Branch 'add-ahmad' set up to track remote branch 'add-ahmad' from 'origin'.
```

**✅ Success indicator:** Nampak link untuk create PR!

---

#### Step 2: Navigate to GitHub

**Option 1: Guna link dari output**
- Copy link dari terminal output
- Paste dalam browser

**Option 2: Manual navigation**
1. Open browser
2. Go to: `https://github.com/[your-username]/bootcamp-devops`
3. GitHub akan auto-detect branch baru dan tunjukkan banner kuning:
   ```
   add-ahmad had recent pushes X minutes ago
   [Compare & pull request]
   ```
4. Click **"Compare & pull request"** button

---

#### Step 3: Create Pull Request

Anda akan nampak form "Open a pull request":

**1. Verify Base & Compare:**

Pastikan settings ni BETUL:
```
base repository: opariffazman/bootcamp-devops    base: main
head repository: [your-username]/bootcamp-devops compare: add-[nama]
```

**❗ IMPORTANT:**
- **base repository** MESTI `opariffazman/bootcamp-devops`
- **head repository** MESTI your fork
- If salah, klik link untuk tukar

---

**2. Fill PR Title:**

Format yang baik:
```
docs: add peserta biodata - [Nama Anda]
```

**Example:**
```
docs: add peserta biodata - Ahmad Abdullah
```

---

**3. Fill PR Description:**

Guna template ni:

```markdown
## 📋 Summary
Menambah biodata peserta untuk Sesi 4 GitHub Lab.

**Peserta:** [Nama Anda]
```

---

**4. Preview Changes:**

- Scroll down untuk lihat **"Files changed"** tab
- Verify file content betul
- Pastikan hanya file anda sahaja yang berubah

**Expected:**
```
✅ peserta/ahmad.md
```

---

**5. Request Reviewer (Optional):**

- Right sidebar → **Reviewers**
- Search: `opariffazman`
- Click untuk assign sebagai reviewer

---

**6. Create Pull Request:**

- Verify semua info betul
- Click **"Create pull request"** button (hijau, bawah sekali)

---

#### Step 4: Verify PR Created

**✅ Success indicators:**

1. **URL berubah** ke:
   ```
   github.com/opariffazman/bootcamp-devops/pull/[number]
   ```

2. **Status badge** nampak:
   ```
   🟢 Open    [your-username] wants to merge 1 commit into main
   ```

---

### ✅ Lab 4 Checklist

- [ ] Branch pushed to YOUR fork successfully
- [ ] Pull Request created to `opariffazman/bootcamp-devops`
- [ ] PR title follow convention
- [ ] PR description complete
- [ ] Files changed shows only your biodata
- [ ] PR is in "Open" state
- [ ] Can see PR number (e.g., #5)
- [ ] Ready for review
