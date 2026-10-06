# Hardware Installation

## Always-on PC Setup

Installed a fresh Ubuntu Server 26.04.1 LTS onto a mini-PC with a decent AMD CPU, 16 GB RAM, and a 256 GB SSD.

Over SSH:
```sh
# Update OS
sudo apt update && sudo apt full-upgrade -y

# Setup Tailscale
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

# (SSH to the system) Reboot for kernel update
sudo reboot

# (After setting up an SSH key) Disable SSH password auth
echo -e "PasswordAuthentication no\nKbdInteractiveAuthentication no" | sudo tee /etc/ssh/sshd_config.d/01-keys-only.conf
sudo systemctl restart ssh
```

Ran various memory stress checks to ensure the PC itself is reliable.
