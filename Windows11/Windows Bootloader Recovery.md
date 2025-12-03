---

# 🧰 bcdboot – Windows Bootloader Recovery Utility

---

## ⚡️ Use Case:
Fix broken or missing **Windows bootloader** after:
- Dual-booting with Linux
- EFI corruption
- `bootrec /fixboot` fails with `Access is denied`

---

## 🧪 Example: Real Scenario – Fixing Boot After Linux Overwrites EFI

### 🧭 System Setup:
- Dual-boot: Parrot OS + Windows 10
- Windows not showing in GRUB
- Bootrec `/fixboot` returns `Access is denied`
- Goal: Restore Windows bootloader using `bcdboot`

---

## 🧱 Step-by-Step Recovery

### 🔹 Boot from Windows Installation USB/DVD

1. Click: `Repair your computer` → `Troubleshoot` → `Command Prompt`

---

### 🔹 Step 1: Identify EFI Partition

```cmd
diskpart
list disk
select disk 0
list partition
```

Sample Output:
```
Partition 1    System     100 MB
Partition 2    Reserved   16 MB
Partition 3    Primary    200 GB
```

EFI is usually:
- ~100–300 MB (Mine was 100mb, 16mb was for parrot os)
- Type: System
- Format: FAT32

---

### 🔹 Step 2: Assign Drive Letter

```cmd
select partition 1
assign letter=Z
exit
```

Now `Z:` points to your EFI partition.

---

### 🔹 Step 3: Identify Windows Installation

Try each letter:

```cmd
dir C:\
dir D:\
dir E:\
```

Suppose `dir D:\` shows `Windows\` — that's the correct partition.

---

### 🔹 Step 4: Run bcdboot

```cmd
bcdboot D:\Windows /s Z: /f UEFI
```

Expected Output:

```
Boot files successfully created.
```

This copies boot files to:

```
Z:\EFI\Microsoft\Boot\bootmgfw.efi
```

---

## ✅ Bootloader Fixed!

Reboot. You should now boot directly into **Windows**.

---

## 🛠️ Optional: Reorder UEFI Boot

If system still boots to Linux or GRUB:

1. Enter BIOS/UEFI setup
2. Set **Windows Boot Manager** as first boot option

Or from CMD in Windows:

```cmd
bcdedit /set {bootmgr} path \EFI\Microsoft\Boot\bootmgfw.efi
```

---

## ⚠️ Troubleshooting Errors

| Error                                        | Reason                                       | Fix                                  |
| -------------------------------------------- | -------------------------------------------- | ------------------------------------ |
| `Access is denied`                           | `bootrec /fixboot` fails on UEFI             | Skip it. Use `bcdboot`               |
| `Failure when attempting to copy boot files` | Wrong partition or Windows install corrupted | Verify partition, try `chkdsk D: /f` |
| Boot files created but not booting           | Entry not added or boot order wrong          | Use BIOS or `bcdedit` to fix         |
| `bcdboot` doesn't work at all                | ESP may be corrupt or unformatted            | Try `format Z: /fs:FAT32` then retry |
unformatted
---

## 🧠 Pro Admin Tips

- Run `bcdboot` from **WinPE**, not from full Windows
- Back up current boot config:
  ```cmd
  bcdedit /export C:\bcdbackup
  ```
- View boot entries:
  ```cmd
  bcdedit /enum firmware
  ```
- Remove broken entries:
  ```cmd
  bcdedit /delete {GUID}
  ```

---

## 📝 Full Command Sequence

```cmd
diskpart
list disk
select disk 0
list partition
select partition 1
assign letter=Z
exit

dir D:\         # Confirm Windows install
bcdboot D:\Windows /s Z: /f UEFI
```

---

## 🏁 Reboot and You're Done!