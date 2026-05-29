01 — Proxmox Setup on HP EliteDesk
Hardware

Device: HP EliteDesk 800 G2 Mini PC
Hypervisor: Proxmox VE 7.0
Install Method: Bootable USB via Rufus


What is Proxmox?
Proxmox Virtual Environment (PVE) is a free, open-source hypervisor that lets you run multiple virtual machines (VMs) and containers on a single physical machine. It has a web-based UI accessible from any browser on your local network.

Creating the Bootable USB

Downloaded Proxmox VE ISO from proxmox.com/downloads
Used Rufus to create a bootable USB drive — Used Rufus to flash the Proxmox ISO directly onto a USB drive
Booted the HP EliteDesk from USB (spam F10 or ESC on startup to get the boot menu)
Followed the Proxmox installer — set root password, configured network


Accessing the Web UI
Once installed, Proxmox runs a web interface at:
https://<proxmox-ip>:8006
Log in as root with the password set during install. The browser will warn about a self-signed certificate — this is normal, just accept and continue.

Error: KVM Virtualisation Not Available
When creating the first VM, I hit this error:
KVM virtualisation configured, but not available.
Either disable in VM configuration or enable in BIOS.
Cause
Hardware virtualisation (VTx) was disabled in the BIOS by default.
Fix

Rebooted the HP EliteDesk
Entered BIOS by pressing F10 during boot
Navigated to Security → System Security
Enabled Virtualization Technology (VTx)
Also enabled VT-d (useful for PCIe passthrough later)
Saved and exited

Verify the fix
Run this in the Proxmox shell to confirm VT-x is now visible:
bashegrep -c '(vmx|svm)' /proc/cpuinfo
Any number greater than 0 means virtualisation is active and VMs will work correctly.
