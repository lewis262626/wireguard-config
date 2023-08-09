# wireguard-config

Simple WireGuard config used on an Ubuntu 22.04 VPS

## Install

1. Install necessary tools

```sh
sudo apt-get update
sudo apt-get install wireguard-tools
```

2. Generate key pairs

```sh
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

3. Enable `ipv4` and `ipv6` port forwarding

```conf
;/etc/sysctl.conf
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```

4. Apply changes

```sh
sudo sysctl -p
```

5. Allow WireGuard on INPUT chain

```sh
sudo ufw allow 51820/udp
sudo ufw allow OpenSSH
```

6. Copy across `wg0.conf` to `/etc/wireguard/wg0.conf`

6. Start server

```sh
sudo wg-quick up wg0
```

7. Add a peer to the server

```sh
sudo wg set wg0 peer peer_pub_key allowed-ips 10.8.0.2,fd0d:86fa:c3bc::2
```

## Peer Config

```conf
[Interface]
PrivateKey = <>
Address = 10.8.0.2/24, fd24:609a:6c18::2/64
DNS = 2606:4700:4700::1111

[Peer]
PublicKey = <>
AllowedIPs = 0.0.0.0/0, ::/0
```

