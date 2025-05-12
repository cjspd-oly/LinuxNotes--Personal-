# 🧠 **Enabling Hibernation on Linux (Using Existing Swap Partition)**

---

## 1. **What is it?**  
**Hibernation** saves the current state (RAM) to the **swap partition**, powers off the system, and restores everything exactly as it was after boot.

---

## 2. **Why is it required?**  
- Saves battery on laptops  
- Lets you **pause work** and resume without closing apps  
- Keeps your session persistent across reboots

---

## 3. **Which problem does it solve?**  
It gives you a **zero-power suspend** option, especially useful on laptops when suspend isn’t enough or power is limited.

---

## 🔧 4. **Steps to Enable Hibernation**

---

### ✅ A. Check Swap Size

Make sure your **swap size ≥ RAM size**.

```bash
free -h
lsblk
```

If not, hibernation **will fail**.

---

### 🔍 B. Get UUID and Resume Offset

1. **Get UUID of swap**:
   ```bash
   sudo blkid /dev/nvme0n1p9
   ```

   Example:
   ```
   UUID="abcd-1234" TYPE="swap"
   ```

2. **Find resume offset** (only if using a swap **file**, not needed for swap partition)

---

### ⚙️ C. Edit GRUB Boot Parameters

1. Edit the GRUB config:
   ```bash
   sudo nano /etc/default/grub
   ```

2. Find the line starting with `GRUB_CMDLINE_LINUX_DEFAULT` and **add** the resume option:

   Example:
   ```bash
   GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=UUID=abcd-1234"
   ```

3. Save and update GRUB:
   ```bash
   sudo update-grub
   ```

---

### 🧩 D. Configure initramfs

1. Edit initramfs config:
   ```bash
   sudo nano /etc/initramfs-tools/conf.d/resume
   ```

2. Add this line:
   ```
   RESUME=UUID=abcd-1234
   ```

3. Update initramfs:
   ```bash
   sudo update-initramfs -u
   ```

---

### 🧪 E. Test Hibernation

Run:
```bash
sudo systemctl hibernate
```

After reboot, your session should **restore exactly** as it was.

---

#### 🚨 Troubleshooting Tips

- If it reboots **without restoring**:
  - Swap too small
  - Wrong UUID
  - Secure Boot may block hibernation (try disabling it in BIOS)
  - Check logs:
    ```bash
    journalctl -b -1 | grep resume
    ```

---

#### ✅ Additional Useful Insights

- On **some distros**, hibernation is disabled by default in logind settings — to enable it:
  ```bash
  sudo nano /etc/systemd/logind.conf
  ```
  Add or modify:
  ```
  HandleLidSwitch=hibernate
  ```

- GUI hibernate button may require:
  ```bash
  sudo apt install policykit-1
  ```

- To see available power options:
  ```bash
  systemctl list-unit-files | grep hibernate
  ```

---