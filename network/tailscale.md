# Tailscale Installation on Linux

## Download and install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

```bash
# start
sudo tailscale up

# status and ip
tailscale status
tailscale ip

# check if service is enabled
systemctl is-enabled tailscaled
```