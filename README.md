# Raspberry Pi Homelab with Pi-hole and Unbound
## Summary
This project provides an Ansible playbook for setting up a Raspberry Pi as a network ad blocker using Pi-hole, with Unbound serving as a DNS resolver. It's tailored for home lab environments, aiming to enhance network performance and security.

## Pre-requisites
- Raspberry Pi Hardware: You need a Raspberry Pi device.
- Raspberry Pi OS: Install the Raspberry Pi 64-bit OS.
- Network Connection: Connect your Raspberry Pi to your network.

## Initial Raspberry Pi Setup

1. **Flash Raspberry Pi OS:**
   - Download and use the Raspberry Pi Imager to flash the 64-bit OS onto an SD card.
   - Insert the SD card into your Raspberry Pi and boot it up.

1. **Initial Configuration:**
    - Complete the on-screen setup instructions on first boot.
    - Make sure your Pi is connected to the internet.

1. **Add a Sudo User:**
   - SSH into your Raspberry Pi: `ssh pi@<raspberry-pi-ip>`
   - Create a new user: `sudo adduser <username>`
   - Add the user to the sudo group: `sudo usermod -aG sudo <username>`
   - Edit the sudoers file for passwordless sudo: `sudo visudo`
   - Append this line: `<username> ALL=(ALL) NOPASSWD: ALL`

1. **Update the System:**
   - Run: ```sudo apt update && sudo apt upgrade -y```

## Ansible Setup on Your Computer
1. Install Ansible:
    - Install Ansible following the official instructions.
1. Clone the Project:
    - Use `git clone <repository-url>` to clone the project.
1. Update Ansible Inventory:
    - Edit inventories/hosts in the project.
    - Update it with your Raspberry Pi's IP address.
1. Create an Ansible Vault for Pi-hole Password:
    - Hash the password: ```echo -n 'P@ssw0rd' | sha256sum | awk '{printf "%s",$1 }' | sha256sum```
    - Create the vault: ```ansible-vault create homelab_vault.yml```
    - Add the hashed password gnerated to the vault:
    ```pihole_password: <Hashed Password>```
    - Save and exit the file.

## Running the Playbook
1. Execute the Playbook:
   - Run: ansible-playbook -i inventories/hosts playbooks/main.yml --ask-vault-pass
   - Enter the vault password when prompted.
1. Verify Installation:
   - Check the Pi-hole admin interface at http://<raspberry-pi-ip>/admin/.
   - Verify Unbound: ```ssh <username>@<raspberry-pi-ip> 'systemctl status unbound'```

## Configure Router to Use Pi-hole
1. Router DNS Settings:
   - Log into your router's settings.
   - Set the DNS server to your Raspberry Pi's IP.
   - Apply and save changes.

## Conclusion
Your Raspberry Pi should now be running Pi-hole with Unbound, serving as an ad blocker and secure DNS resolver for your network.

Remember to replace ```<username>, <raspberry-pi-ip>, and <repository-url>``` with your actual user name, Raspberry Pi's IP address, and the repository URL.
