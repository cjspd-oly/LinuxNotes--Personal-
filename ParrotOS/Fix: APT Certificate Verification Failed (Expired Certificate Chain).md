# ❌ **Fix: APT Certificate Verification Failed (Expired Certificate Chain)**

When running `sudo apt update`, you may see:

> **Certificate verification failed: The certificate is NOT trusted. The certificate chain uses expired certificate. Could not handshake: Error in the certificate verification.**

---

## 🧠 Why It Happens

- 🔑 APT fails HTTPS handshake due to an **expired or untrusted certificate** in the CA chain.
- 🌐 This is often triggered by:
  - Outdated CA certificates
  - Expired intermediate certificates
  - Misconfigured system time
  - Self-signed or improperly maintained repo servers

---

## ⚙️ **Temporary Workaround (Bypass Verification) – Insecure**

```bash
echo 'Acquire::https::Verify-Peer "false";' | sudo tee /etc/apt/apt.conf.d/99insecure-https
sudo apt update
```

- 🚨 This **disables HTTPS certificate validation**—not safe for regular use.
- ⚠️ Only use this **temporarily** to install or update specific packages while fixing the root cause.

---

## 🔁 **Revert to Secure APT Mode (After Fixing)**

```bash
sudo rm /etc/apt/apt.conf.d/99insecure-https
```

- ✅ Restores default certificate verification behavior.
- 🔐 Always re-enable verification to avoid MITM attacks.

---

## 🛡️ Proper Solutions (Recommended)

1. 🕒 **Fix System Time**
   ```bash
   timedatectl status
   sudo timedatectl set-ntp true
   ```

2. 📜 **Update CA Certificates**
   ```bash
   sudo apt install --reinstall ca-certificates
   sudo update-ca-certificates
   ```

3. 🔄 **Switch to HTTP (if repo allows)**  
   In `/etc/apt/sources.list`, change:
   ```
   deb https://example.com/repo stable main
   ```
   to:
   ```
   deb http://example.com/repo stable main
   ```

4. 🧪 **Manually Inspect the Cert Chain**  
   Use:
   ```bash
   openssl s_client -connect example.com:443 -showcerts
   ```

---

Let me know if you want an automated script to toggle this insecure mode or log the cert issue!