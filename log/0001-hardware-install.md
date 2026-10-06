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

# Setup Docker & webtop
curl -fsSL https://get.docker.com | sudo sh
mkdir -p ~/webtop && cd ~/webtop && vim compose.yaml
# Contents:
services:
  webtop:
    image: lscr.io/linuxserver/webtop:ubuntu-xfce
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Los_Angeles
      - CUSTOM_USER=bob
      - PASSWORD=REPLACE_ME_WITH_PASSWORD
    volumes:
      - ./config:/config
    devices:
      - /dev/dri:/dev/dri
    ports:
      - "REPLACE_ME_WITH_TAILSCALE_IP:REPLACE_ME_WITH_SAME_PORT:REPLACE_ME_WITH_SAME_PORT"
    shm_size: "1gb"
    restart: unless-stopped

# Init webtop after Tailscale on reboot
sudo systemctl edit docker
# Contents:
[Unit]
After=tailscaled.service
Wants=tailscaled.service
```

Ran various memory stress checks to ensure the PC itself is reliable.
