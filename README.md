
## 1) DevStack Installation Guide (Ubuntu 20.04 / 22.04)

> DevStack creates a full OpenStack dev environment for testing and learning.

### Requirements

| Item | Minimum |
|---|---|
| OS | Ubuntu 20.04 or 22.04 |
| RAM | 8GB+ (16GB recommended) |
| CPU | 4+ cores |
| Disk | ~30GB free |
| User | sudo-enabled non-root user |
| Network | Internet required |

---


### Step 2 — Create required `stack` user

```bash
sudo useradd -m -d /opt/stack -s /bin/bash stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo su - stack
```

---

### Step 3 — Clone DevStack repo

```bash
git clone https://opendev.org/openstack/devstack
cd devstack
```

---

### Step 4 — Create `local.conf`

```bash
nano local.conf
```

Paste the following:

```ini
[[local|localrc]]
ADMIN_PASSWORD=admin
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD

# Use your machine's IP (not 127.0.0.1 for remote access)
HOST_IP=127.0.0.1

# Core services
enable_service horizon
enable_service glance
enable_service keystone
enable_service neutron
enable_service nova
enable_service placement-api
```

Save and exit.

---

### Step 5 — Install DevStack

```bash
./stack.sh
```

This step may take a while.

---
<img width="1943" height="874" alt="image" src="https://github.com/user-attachments/assets/952fd8e7-9d0f-4a90-bb1f-442bf91db0af" />

### Step 6 — Access Horizon Dashboard

Open your browser:

```
http://<your-ip>/dashboard
```

Default login:

| Username | Password |
| -------- | -------- |
| admin    | admin    |

---

### Manage DevStack

Stop services:

```bash
./unstack.sh
```

Clean environment:

```bash
./clean.sh
```

---

### 🛠 Troubleshooting

| Problem             | Fix                                |
| ------------------- | ---------------------------------- |
| Low memory errors   | Increase RAM or add swap           |
| Instances get no IP | Restart neutron or reboot          |
| Auth failures       | Password mismatch in config        |
| Reinstall fails     | Run `./clean.sh` then `./stack.sh` |

---

### VM Settings (if using VirtualBox/VMware)

* Enable VT-x / AMD-V
* Bridged networking recommended
* At least **8GB RAM** and **4 vCPUs**

---

## 2) PackStack Quick Note (for reference)

> PackStack works on RHEL/CentOS-like systems and installs OpenStack using RPMs + Puppet.

**Basic flow:**

```bash
sudo dnf update -y
sudo dnf install -y https://repos.fedorapeople.org/repos/openstack/openstack-wallaby/rdo-release-wallaby-1.noarch.rpm
sudo dnf install -y openstack-packstack
packstack --allinone
```

Then access:

```
http://<your-ip>/dashboard
```

---

## Summary

| Use Case                  | Recommended Tool |
| ------------------------- | ---------------- |
| Learn OpenStack internals | **DevStack**     |
| Practice cloud operations | **PackStack**    |
| PoC / Classroom setup     | **PackStack**    |
| OpenStack feature dev     | **DevStack**     |

---
