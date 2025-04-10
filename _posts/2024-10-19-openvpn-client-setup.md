---
title: "OvenVPN-client"
date: 2024-10-19 00:00:00 +0800
categories: [VPN]
image: assets/images/mysql.png
tags: [OpenVPN Client]
---  
# OpenVPN client setup:

1. If you going to use OpenVPN inside LXC conatiner add these lines at the end of the  `lxc-container-id.conf` file which can be located in the `/etc/pve/lxc/100.conf`
```sh
# allow /dev/tun access
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```
2. Create a openvpn conf file for client. Sample template can be found on this file [[openvpn-server-and-pki-setup]]

## OpenVPN installation on clients:
OpenVPN client config file naming convention. 

| machine                  | filename                         |
|     --------             |       -------------              |
| openvpn server           | /etc/openvpn/server.conf         |
| openvpn client (linux)   | /etc/openvpn/client.conf         |
| openvpn client (windows) | smounesh-homelab.ovpn            |

## Ubuntu (apt)
1. Install openvpn using cmd `apt insatll openvpn -y` and place the client.conf file in the `/etc/opencpn` directory
2. Set openvpn to start on boot using cmd `systemctl enable --now openvpn`

### Windows:
1. Download and install the openvpn windows client from [openvpn.net]([OpenVPN Connect Client | Our Official VPN Client | OpenVPN](https://openvpn.net/vpn-client/)) and import the smounesh-homelab.ovpn file

## Refernces:
* [[OpenVPN in LXC - Proxmox VE]]