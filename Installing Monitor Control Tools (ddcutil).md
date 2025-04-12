# 🖥️ **Control Monitor Settings via DDC/CI in Linux**

Control external monitor settings (like brightness, contrast, input source) directly from your terminal using `ddcutil` over the I2C bus. This is useful for scripting display adjustments or fixing brightness issues without touching physical buttons.

---

## 🧰 Step 1: Install Required Packages

```bash
sudo apt update
sudo apt install ddcutil i2c-tools
```

- 📦 `ddcutil`: Communicates with monitors via DDC/CI.
- 🧪 `i2c-tools`: Includes `i2cdetect`, required for probing I2C buses.

---

## 🔎 Step 2: Detect Supported Monitors

```bash
sudo ddcutil detect
```

- 📡 Scans I2C buses for monitors with DDC/CI support.
- ⚠️ If no output, kernel I2C support may not be enabled yet (see below).

---

## 🧠 Step 3: Load I2C Kernel Module (Temporarily)

```bash
sudo modprobe i2c-dev
```

- 🔧 Loads `i2c-dev`, enabling user-space access to I2C interfaces like `/dev/i2c-*`.
- 📌 Temporary: Will be removed on reboot unless set to load automatically.

---

## 📂 Step 4: Confirm I2C Interface Availability

```bash
ls /dev/i2c-*
```

- 🧾 Lists detected I2C bus devices.
- ✅ Necessary for `ddcutil` to communicate with the monitor.
- Example: `/dev/i2c-0`, `/dev/i2c-1`, etc.

---

## ♻️ Step 5: Auto-load I2C Module on Boot

```bash
echo i2c-dev | sudo tee -a /etc/modules
```

- 💾 Adds `i2c-dev` to the list of modules loaded at startup.
- 🔁 No need to run `modprobe` after reboot.

---

## ✅ **Done! Test Monitor Control**

Example:  
```bash
ddcutil getvcp 10         # 🔍 Get current brightness (VCP code 10)
ddcutil setvcp 10 70      # 💡 Set brightness to 70%
```

---

## ⚠️ Troubleshooting Tips

- 🔒 **Permission Denied?** Add user to `i2c` group:  
  ```bash
  sudo usermod -aG i2c $USER
  ```
  Then **logout & login** again.
  
- 🧪 **No Monitor Found?** Try switching HDMI/DisplayPort or different I2C bus:
  ```bash
  sudo ddcutil --bus=1 detect
  ```

---