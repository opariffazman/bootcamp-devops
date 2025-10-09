## LAB 10: CLEANUP

**PENTING:** Mesti terminate instance untuk elakkan charges!

### Langkah 1: Exit SSH

Dalam SSH session, type:
```bash
exit
```

### Langkah 2: Terminate Instance

1. Pergi ke AWS Console → EC2 → Instances
2. Select instance anda (checkbox)
3. **Instance state** → **Terminate instance**
4. Type **"terminate"** untuk confirm
5. Click **"Terminate"**

### Langkah 3: Verify

Tunggu 1-2 minit. Instance state akan jadi:
```
Terminated
```

✅ Cleanup complete! Tiada lagi charges.

## LAB10.1: AWS ZERO-SPEND BUDGET

### Langkah 1: Taip "Billing and Cost Management"

### Langkah 2: Setup Budget

1. Pergi ke Cost Monitor → Setup Required
2. Pastikan **Template Budget** adalah "Zero-Spend Budget"
3. Isikan email anda untuk terima notifikasi apabila ada alert
4. Click **"Create Budget"**

Reference Mudah:

![aws-zero-budget](https://github.com/user-attachments/assets/95f71aec-bebd-471a-9909-eff08c313bbc)
