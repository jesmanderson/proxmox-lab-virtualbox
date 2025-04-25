# 🧱 Proxmox VE Lab (Nested Virtualization in VirtualBox)

## 🎯 Purpose
This project simulates a real-world virtualization environment using **Proxmox VE** installed inside **VirtualBox**. It is a foundational lab for learning DevOps, Infrastructure Engineering, Virtualization, and Cloud Concepts using a completely local setup.

---

## 🛠️ Tools Used
- **VirtualBox** – Virtualization platform for host machine
- **Proxmox VE ISO** – Enterprise-grade hypervisor installed in the VM
- **Ubuntu Server ISO** – Operating system for guest VM inside Proxmox
- **VBoxManage** – CLI tool for configuring VirtualBox VMs
- **Proxmox Web UI** – Management dashboard for inner virtual machines

---

## 🧱 Lab Architecture

```
Your PC (Host)
└── VirtualBox
    └── Proxmox VE (Nested VM)
        └── Ubuntu Server (Guest VM inside Proxmox)
```

This simulates how large companies manage multiple servers from a single control panel.

---

## ✅ What Was Built

- Installed Proxmox VE inside a VirtualBox VM
- Configured networking to access Proxmox Web UI from browser
- Uploaded Ubuntu Server ISO to Proxmox
- Created and launched Ubuntu VM inside Proxmox
- Resolved KVM virtualization error for nested environment

---

## 📡 Network Configuration

- **Adapter 1**: NAT (for outbound internet access)
- **Adapter 2**: Host-Only (for internal communication with Proxmox from host)

Inside Proxmox terminal:
```bash
ip link set enp0s8 up
dhclient enp0s8
```

Access Proxmox Web UI from browser:
```
https://192.168.56.X:8006
```

---

## 💾 Upload ISO

1. Go to `Datacenter > proxmox > local > ISO Images`
2. Click **Upload** and choose Ubuntu ISO file
3. ISO will be available for new VM creation

---

## 🖥️ Create a New VM in Proxmox

1. Click **"Create VM"**
2. Select ISO and name the VM
3. Set CPU, RAM, storage, and network settings
4. Disable KVM virtualization with:
```bash
qm set 100 --kvm 0
```
5. Start VM and install Ubuntu normally

---

## 🧰 Common Issues & Fixes

### ❌ `TASK ERROR: KVM virtualization configured, but not available`
> Fix:
```bash
qm set <vm-id> --kvm 0
```

### ❌ Can't Access GUI in Browser
> Fix:
- Ensure IP is `192.168.56.X` (Host-Only adapter)
- Use `https://<ip>:8006`
- Accept certificate warning

---

## 📸 Screenshots
- `screenshots/proxmox-install.png`
- `screenshots/network-setup.png`
- `screenshots/ubuntu-vm-booted.png`

---

## 📁 Suggested Repo Structure

```
proxmox-lab-virtualbox/
├── README.md
├── screenshots/
├── notes/
│   └── fix-kvm-error.md
├── config/
│   └── vm-100.conf
```

---

## 🙌🏾 Credits & Reflection

This lab was built from scratch to gain hands-on experience with virtualization and infrastructure. It’s a great practice ground for cloud engineers, DevOps students, and IT pros transitioning into server and VM administration roles.

Built with sweat, troubleshooting, and a whole lot of command-line magic 💻🔥