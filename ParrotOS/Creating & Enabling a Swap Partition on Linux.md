# 🧠 **Creating & Enabling a Swap Partition on Linux (GParted + GNOME Disks Method)**

---

## 1. **What is it?**  
A step-by-step guide to creating a **swap partition**, setting it to mount on boot using **GNOME Disks**, and activating it via the terminal.

---

## 2. **Why is it required?**  
- Acts as virtual RAM  
- Prevents crashes when physical memory is exhausted  
- Enables features like **hibernation**

---

## 3. **Which problem does it solve?**  
Provides an automatic fallback memory area to keep the system stable under memory pressure.

---

## 4. **Setup Steps (My Method)**

---

### 🛠️ A. Create the Swap Partition (via GParted)

1. Launch **GParted**  
2. Select your disk (e.g., `/dev/nvme0n1`)  `Be careful here with choosing disk won't be the same.`
3. Right-click on unallocated space → **New**  
   - File system: `linux-swap`  
4. Click ✅ **Apply All Operations**

---

### ⚙️ B. Set Auto-Mount Using GNOME Disks (`nvme0n1p9`)

1. Open **GNOME Disks**  
2. Select your swap partition (e.g., `/dev/nvme0n1p9`)  
3. Click **Edit Mount Options**  
4. Toggle **OFF** “User Session Defaults”  
5. Set **Mount Point**: `/dev/nvme0n1p9`  
6. Enable ✅ **Mount at system startup**  
7. Click **OK** to save

> ⚠️ This step eliminates the need to manually edit `/etc/fstab`.

---

### 💽 C. Format and Enable Swap via Terminal (`nvme0n1p9`)

1. Format the partition as swap:
   ```bash
   sudo mkswap /dev/nvme0n1p9
   ```

2. Enable it immediately:
   ```bash
   sudo swapon /dev/nvme0n1p9
   ```

---

### 🔍 D. Verify the Setup

- Check if swap is active:
  ```bash
  swapon --show
  free -h
  ```

- After reboot, confirm it's still active:
  ```bash
  cat /proc/swaps
  ```

---

#### ✅ Additional Useful Insights

- Swap file alternative:
  If no free partitions exist, a **swap file** can be created instead

- You can disable swap temporarily:
  ```bash
  sudo swapoff /dev/nvme0n1p9
  ```

- Use `htop` to watch real-time memory & swap usage

---