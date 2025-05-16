
# Offline Installation of Redis Enterprise on RHEL 9

This guide provides the step-by-step instructions to install Redis Enterprise offline on a RHEL 9 server.

## ✅ Prerequisites
- Access to a RHEL 9 server (online) to download the necessary RPM packages and dependencies.
- Access to a RHEL 9 server (offline) where Redis Enterprise will be installed.
- SCP, rsync, or other transfer methods to move files between servers.

---

## ✅ Step 1: Download Packages on Online Server

1. **Create a directory for the packages:**

```bash
mkdir -p /tmp/redis-offline
cd /tmp/redis-offline
```

2. **Copy the Redis Enterprise RPM package:**

```bash
cp /path/to/redislabs-7.8.6-60.rhel9.x86_64.rpm .
```

3. **Download Dependencies:**

```bash
dnf install --downloadonly --downloaddir=/tmp/redis-offline redislabs-7.8.6-60.rhel9.x86_64.rpm
```

Or using `yum`:

```bash
yum install --downloadonly --downloaddir=/tmp/redis-offline redislabs-7.8.6-60.rhel9.x86_64.rpm
```

4. **Verify downloaded files:**

```bash
ls -lh /tmp/redis-offline/
```

5. **Compress the files for transfer:**

```bash
tar -czvf redis-offline.tar.gz -C /tmp/redis-offline .
```

---

## ✅ Step 2: Transfer to Offline Server

Transfer the compressed file to the offline server:

```bash
scp redis-offline.tar.gz user@offline-server:/tmp/
```

---

## ✅ Step 3: Extract and Install on Offline Server

1. **Login to the offline server:**

```bash
ssh user@offline-server
```

2. **Extract the package:**

```bash
cd /tmp/
tar -xzvf redis-offline.tar.gz
cd redis-offline/
```

3. **Disable Online Repositories:**

Edit the repository file:

```bash
vi /etc/yum.repos.d/redhat.repo
```

Set `enabled = 0` for all repositories.

Alternatively, run:

```bash
dnf config-manager --set-disabled \*
```

4. **Install RPMs Offline:**

```bash
dnf install -y *.rpm --disablerepo="*"
```

Or using `yum`:

```bash
yum localinstall -y *.rpm --disablerepo="*"
```

5. **Run the Installation Script:**

If the installation fails, uninstall first:

```bash
cd /opt/redislabs/bin
./rl_uninstall.sh
```

Then re-run the installation script.

---

## ✅ Step 4: Verify the Installation

Check the Redis Enterprise installation:

```bash
rpm -q redislabs
```

If there are missing dependencies, return to the online server and download them.

---

Need additional troubleshooting steps? Feel free to reach out! 👍🙂
