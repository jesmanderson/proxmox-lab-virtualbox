<p align="center">
  I Built a Datacenter Inside My Laptop — Here’s the Lab 
</p>
<p align="center">
  <img src="https://img.shields.io/badge/VirtualBox-7.x-blue" />
  <img src="https://img.shields.io/badge/Proxmox-8.x-red" />
  <img src="https://img.shields.io/badge/Ubuntu-22.04-orange" />
  <img src="https://img.shields.io/badge/Linux-Kernel--Based-informational" />
</p>

# Proxmox VE Lab (Nested Virtualization in VirtualBox)

## Purpose
This project simulates a real-world virtualization environment using **Proxmox VE** installed inside **VirtualBox**. It is a foundational lab for learning DevOps, Infrastructure Engineering, Virtualization, and Cloud Concepts using a completely local setup.

---

## Tools Used
- **VirtualBox** – Virtualization platform for host machine
- **Proxmox VE ISO** – Enterprise-grade hypervisor installed in the VM
- **Ubuntu Server ISO** – Operating system for guest VM inside Proxmox
- **VBoxManage** – CLI tool for configuring VirtualBox VMs
- **Proxmox Web UI** – Management dashboard for inner virtual machines

---

## Lab Architecture

```
Your PC (Host)
└── VirtualBox
    └── Proxmox VE (Nested VM)
        └── Ubuntu Server (Guest VM inside Proxmox)
```

This simulates how large companies manage multiple servers from a single control panel.

---

## What Was Built

- Installed Proxmox VE inside a VirtualBox VM
- Configured networking to access Proxmox Web UI from browser
- Uploaded Ubuntu Server ISO to Proxmox
- Created and launched Ubuntu VM inside Proxmox
- Resolved KVM virtualization error for nested environment

---

## Network Configuration

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

## Upload ISO

1. Go to `Datacenter > proxmox > local > ISO Images`
2. Click **Upload** and choose Ubuntu ISO file
3. ISO will be available for new VM creation

---

## Create a New VM in Proxmox

1. Click **"Create VM"**
2. Select ISO and name the VM
3. Set CPU, RAM, storage, and network settings
4. Disable KVM virtualization with:
```bash
qm set 100 --kvm 0
```
5. Start VM and install Ubuntu normally

---

## VM Configuration

This lab includes a sample Proxmox VM config file: [`vm-100.conf`](./config/vm-100.conf)

This file defines how the Ubuntu guest VM is configured in Proxmox, including:

- `cores` and `memory`: how much CPU/RAM is allocated
- `net0`: virtual NIC using `virtio`, attached to `vmbr0`
- `scsi0`: the disk image location and size (10G)
- `ide2`: the ISO attached as a CD-ROM
- `kvm: 0`: disables KVM because we’re using nested virtualization in VirtualBox

> You can recreate or script VM builds using this file format with `qm` commands.

---

##  Common Issues & Fixes

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

## Screenshots
- `screenshots/proxmox-install.png`
- `screenshots/network-setup.png`
- `screenshots/ubuntu-vm-booted.png`

---

## Credits & Reflection

This lab was built from scratch to gain hands-on experience with virtualization and infrastructure. It’s a great practice ground for cloud engineers, DevOps students, and IT pros transitioning into server and VM administration roles.

Built with sweat, troubleshooting, and a whole lot of command-line magic 

---

## Next Steps (Phase 2 Ideas)

Want to expand this lab? Here are some ideas to keep leveling up:

- [ ] Create a second VM and test internal networking (ping, SSH)
- [ ] Add static IPs to guest VMs and configure internal DNS
- [ ] Create a Windows Server VM with RDP access
- [ ] Turn Ubuntu VM into a web server (Apache/Nginx + firewall rules)
- [ ] Backup the VM using Proxmox snapshot and restore features
- [ ] Learn cloud-init to automate guest OS setup
- [ ] Explore Ansible or Terraform to deploy future VMs via script

> Everything I learn in this lab maps directly to cloud (AWS, Azure, GCP) and DevOps/Infra roles.
