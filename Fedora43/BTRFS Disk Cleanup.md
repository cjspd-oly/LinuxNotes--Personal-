## ⭐ Snapper – check & manage snapshots

```bash
sudo snapper get-config
sudo snapper list
sudo snapper list-configs
sudo snapper delete <SNAPSHOT_ID>
sudo snapper cleanup timeline
sudo snapper cleanup number
sudo snapper create -d "manual snapshot"
```

---

## ⭐ Snapper – edit config (root)

```bash
sudo gedit admin:///etc/snapper/configs/root
```

Add (if missing):

```ini
IGNORE_PATHS="/var/log /var/cache /var/tmp /tmp /var/lib/flatpak"
```

---

## ⭐Btrfs – inspect filesystem

```bash
sudo btrfs filesystem usage /
sudo btrfs filesystem df /
sudo btrfs filesystem show /dev/nvme0n1p8
sudo btrfs subvolume list /
```

---

## ⭐ Btrfs – space recovery (SAFE)

```bash
sudo btrfs balance start -dusage=60 -musage=60 /
sudo btrfs balance start -dusage=80 /
sudo btrfs balance start -dusage=85 /
sudo btrfs balance status /
sudo btrfs balance cancel /
```

---

## Remove orphaned subvolumes (manual cleanup) - wont be needed anymore

```bash
sudo btrfs subvolume delete /home/.snapshots/1/snapshot
sudo btrfs subvolume delete /home/.snapshots/2/snapshot
sudo btrfs subvolume delete /home/.snapshots

sudo btrfs subvolume delete /.snapshots-old/1/snapshot
sudo btrfs subvolume delete /.snapshots-old/2/snapshot
sudo btrfs subvolume delete /.snapshots-old
```

---

## System space checks

```bash
df -h /
du -sh /home/*
du -sh /.snapshots
```

---

## Safe cleanup (Btrfs-friendly)

```bash
sudo dnf autoremove
sudo dnf clean packages
sudo journalctl --disk-usage
sudo journalctl --vacuum-time=7d
```

---

## Prevent churn (run once)

```bash
chattr +C /home/pd/.cache
chattr +C /home/pd/Downloads
```

---

## Snapper timers

```bash
systemctl status snapper-timeline.timer
systemctl status snapper-cleanup.timer
```

---

## ❌ Do NOT use (on Btrfs + snapshots)

```bash
bleachbit
sudo btrfs balance start /
```

---