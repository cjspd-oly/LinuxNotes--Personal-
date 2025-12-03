sudo dnf install ddcutil akmod-nvidia xorg-x11-drv-nvidia-power
sudo systemctl enable nvidia-powerd.service

sudo bash -c 'cat >/etc/modprobe.d/nvidia-i2c.conf <<EOF
options nvidia NVreg_EnableUserI2C=1
options nvidia NVreg_I2CPhysicalAddressing=1
EOF'

sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
reboot


Here are the **exact commands that made DDC/CI work on your Fedora system** — nothing extra, nothing missing.

---

# ✅ **Working Commands (In Correct Order)**

### **1. Install proprietary NVIDIA driver**

```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-power
sudo systemctl enable nvidia-powerd.service
```

### **2. Enable user-space I²C access for NVIDIA**

Create config:

```bash
sudo nano /etc/modprobe.d/nvidia-i2c.conf
```

Add:

```
options nvidia NVreg_EnableUserI2C=1
options nvidia NVreg_I2CPhysicalAddressing=1
```

### **3. Rebuild GRUB**

```bash
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

### **4. Reboot**

```bash
reboot
```

### **5. Confirm NVIDIA driver is active**

```bash
lspci -nnk | grep -A3 -E "VGA|Display|3D"
```

You got:

```
Kernel driver in use: nvidia
```

### **6. Check that I²C buses are exposed**

```bash
ls /dev/i2c-*
```

You got multiple buses:

```
/dev/i2c-0 ... /dev/i2c-8
```

### **7. Detect monitor via DDC/CI**

```bash
ddcutil detect
```

Your output:

```
Display 1
   I2C bus: /dev/i2c-6
   Model: Acer XZ306C X
   VCP Version: 2.1
```

---

# 🎉 **This is the complete working recipe. Nothing else was required.**

If you want, I can provide a **backup note**, **PDF**, or **script** that configures everything automatically.