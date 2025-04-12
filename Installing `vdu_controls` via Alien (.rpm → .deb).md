# 🖥️ **Installing `vdu_controls` via Alien (.rpm → .deb)**

`vdu_controls` is a GUI front-end for `ddcutil` that lets you control monitor settings like brightness, contrast, and input source.

---

## 🔗 Step 1: Download RPM Package

Get it from:  
👉 [https://software.opensuse.org/package/ddcutil-service](https://software.opensuse.org/package/ddcutil-service)

---

## 🧪 Step 2: Convert `.rpm` to `.deb` Using `alien`

```bash
sudo alien --to-deb ddcutil-service*.rpm
```

- 📦 Converts the RPM into a `.deb` package.
- 📁 Output: `ddcutil-service_*.deb`

---

## 📥 Step 3: Install the `.deb` Package

```bash
sudo dpkg -i ddcutil-service_*.deb
```

- 📌 Installs `vdu_controls` and related files (usually placed under `/usr/bin`).

---

## ▶️ Step 4: Run the App

```bash
vdu_controls
```

---

## ⚠️ Common Errors + Fixes

### ❌ `ImportError: cannot import name 'QtCore' from 'PyQt5'`

> 🛠️ Fix:

```bash
sudo apt install python3-pyqt5
```

---

### ❌ `ModuleNotFoundError: No module named 'PyQt5.QtSvg'`

> 🛠️ Fix:

```bash
sudo apt install python3-pyqt5.qtsvg
```

---

## ✅ Ready to Use

Once dependencies are resolved:

```bash
vdu_controls
```

- 🖼️ GUI should launch successfully.
- 🛠️ Use it to control brightness, contrast etc.

---