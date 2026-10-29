# Networkwalks Cybersecurity Internship

My weekly projects from the Cybersecurity at Networkwalks internship.

## Projects

| Week | Project | Folder |
|------|---------|--------|
| 01 | PM1: Lab Setup (VirtualBox + Kali Linux) | [week-01/pm1-lab-setup](week-01/pm1-lab-setup/) |

## Week 1: Lab Setup

### Project Objective

- Set up VirtualBox and Kali Linux as an attacker machine
- Build a private NAT Network (`10.0.0.0/24`) for the lab
- Give Kali the static IP `10.0.0.2/24` with full Internet access
- Enable clipboard, drag-and-drop and a shared Downloads folder
- Take a clean snapshot as a recovery baseline

### Overview

A small virtual lab for ethical hacking practice. Kali Linux runs in VirtualBox on a private NAT Network, so target VMs can be added later and tested safely.

### Steps

1. **Install 7-Zip.**
   Kali's VirtualBox image comes as a `.7z` archive. 7-Zip is needed to extract it.

2. **Install VirtualBox.**
   Download the latest version from virtualbox.org and install with the default options. This is the hypervisor that runs the lab VMs.

3. **Create the NAT Network.**
   In VirtualBox go to File > Tools > Network > NAT Networks and create `NatNetwork` with prefix `10.0.0.0/24` and DHCP enabled. All lab VMs will share this network and still reach the Internet.

4. **Import Kali Linux.**
   Download the VirtualBox image from kali.org, extract it, and open the `.vbox` file. Then set the VM's network adapter to NAT Network (`NatNetwork`) and memory to 2048 MB.

5. **Enable clipboard, drag-and-drop and shared folder.**
   Set Shared Clipboard and Drag'n'Drop to Bidirectional in the VM's General > Advanced tab. Add the host `Downloads` folder under Shared Folders with Auto-mount on.

6. **Set a static IP in Kali.**
   Open Edit Connections > Wired connection 1 > IPv4 Settings and set Manual with `10.0.0.2/24`, gateway `10.0.0.1`, DNS `8.8.8.8`. A fixed address keeps the lab easy to document and reference.

7. **Test connectivity.**
   Run `ping 10.0.0.1`, `ping 8.8.8.8` and `nslookup networkwalks.com` in Kali. These confirm the gateway, Internet and DNS all work.

8. **Take a snapshot.**
   Create a snapshot named `Clean Kali - Network Setup` once everything works. It gives a clean baseline to restore if a later experiment breaks the VM.

Full write-up with screenshots: [week-01/pm1-lab-setup](week-01/pm1-lab-setup/README.md)

## Resources

- 7-Zip: https://7-zip.org/download.html
- VirtualBox: https://virtualbox.org/wiki/Downloads
- Kali Linux: https://kali.org/get-kali

## Author

**Kiran Nawaz**
