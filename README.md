# Miatoll Kernel with Docker Support

Custom kernel build for Ubuntu Touch on Xiaomi Redmi Note 9 Pro / 9S / 10 Lite / Poco M2 Pro (miatoll) with Docker container support enabled.

## Quick Start

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Name it `miatoll-docker-kernel`
3. Make it **Public** (required for free GitHub Actions)
4. Click **Create repository**

### Step 2: Push This Code

On your device or PC:

```bash
cd /home/phablet/projects/docker/miatoll-kernel-build
git init
git add .
git commit -m "Initial commit: Miatoll kernel with Docker support"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/miatoll-docker-kernel.git
git push -u origin main
```

### Step 3: Run the Build

1. Go to your repository on GitHub
2. Click **Actions** tab
3. Click **"I understand my workflows, go ahead and enable them"**
4. Click **Build Miatoll Kernel with Docker Support** on the left
5. Click **Run workflow** → **Run workflow**
6. Wait 30-60 minutes for the build to complete

### Step 4: Download the Kernel

1. Once build completes (green checkmark), click on the workflow run
2. Scroll down to **Artifacts**
3. Download `miatoll-docker-kernel-X`
4. Extract `boot.img` from the zip

### Step 5: Flash the Kernel

**IMPORTANT: Backup first!**

```bash
# Backup current boot image
sudo dd if=/dev/disk/by-partlabel/boot of=/userdata/boot-backup.img bs=4M
echo "Backup saved to /userdata/boot-backup.img"

# Copy new boot.img to device (via USB/SSH/etc)
# Then flash it:
sudo dd if=/path/to/boot.img of=/dev/disk/by-partlabel/boot bs=4M
sync

# Reboot
sudo reboot
```

### Step 6: Test Docker

After reboot:

```bash
sudo docker run hello-world
```

## Reverting to Original Kernel

If something goes wrong:

```bash
sudo dd if=/userdata/boot-backup.img of=/dev/disk/by-partlabel/boot bs=4M
sync
sudo reboot
```

## What This Adds to the Kernel

| Config | Purpose |
|--------|---------|
| `CONFIG_POSIX_MQUEUE=y` | Container IPC (message queues) |
| `CONFIG_NF_TABLES=y` | Modern netfilter for networking |
| `CONFIG_NETFILTER_XT_MATCH_CGROUP=y` | Cgroup matching for iptables |
| `CONFIG_VETH=y` | Virtual ethernet for containers |
| `CONFIG_BRIDGE=y` | Network bridging |
| `CONFIG_OVERLAY_FS=y` | Docker storage driver |

## Troubleshooting

### Build fails
- Check the build log in the artifacts
- Some kernel configs may conflict - open an issue

### Device won't boot after flashing
- Boot to recovery and restore backup
- Or reflash stock Ubuntu Touch

### Docker still doesn't work
- Check `sudo docker info` for errors
- Verify configs with `zcat /proc/config.gz | grep POSIX_MQUEUE`

## References

- [UBports Kernel Build Docs](https://docs.ubports.com/en/latest/porting/build_and_boot/standalone_kernel_build.html)
- [Docker Kernel Requirements](https://gist.github.com/fredldotme/5dffc8bfa410cfa8f541276407154494)
- [Docker on Ubuntu Touch Forum](https://forums.ubports.com/topic/11595/how-to-run-docker-on-your-device)
