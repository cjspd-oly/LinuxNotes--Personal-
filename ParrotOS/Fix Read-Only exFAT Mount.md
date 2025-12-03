# 🛠️ Fix Read-Only exFAT Mount (LABEL: `iMedia`) Using GNOME Disks

---

## 🔍 1. **Get Your UID and GID (Terminal)**

```bash
id
```

Example output:
```
uid=1000(yourusername) gid=1000(yourusername) groups=...
```

✅ **Note these values** — you'll use them in the next step.

---

## 🧩 2. **Edit Mount Settings Using GNOME Disks**

1. Open **Disks** (`gnome-disks`).
2. Select your exFAT partition (labeled `iMedia`).
3. Click the **gear icon** below the partition → **Edit Mount Options**.
4. Turn **off** “User Session Defaults”.
5. Set **Mount Options** to:

```text
nosuid,nodev,nofail,x-gvfs-show,uid=1000,gid=1000,umask=0022
```

🔁 Replace `1000` with your actual UID and GID (from step 1).

6. Click **OK** and **restart your system** or **remount the drive**.

---

## ✅ Why This Works

- GNOME Disks GUI saves your config to `/etc/fstab`.
- Adding `uid/gid` gives you ownership of the mounted drive.
- `umask=0022` allows read/write for you, read-only for others.
- This prevents the drive from defaulting to read-only mode.

---