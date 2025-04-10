---
title: "Change IP Address in Ubuntu Server System"
date: 2023-03-03 00:00:00 +0800
categories: [Linux]
image: assets/images/ubuntu-linux.jpg
tags: [IP Address]
---
 
To configure a static IP address on your Ubuntu server you need to modify a relevant **netplan** network configuration file within `/etc/netplan/` directory.

> [!note] Carefully check the `renderer` stanza in your configuration file. If the renderer configuration is set to `NetworkManager` then your Ubuntu’s system network configuration is managed by GUI Network Manager. Change the renderer to `renderer: networkd` if you do not wish to configure your network IP address via Graphical User Interface ( desktop ).

```
# File: /etc/netplan/50-cloud-init.yaml
# Purpose: Configure network setting for the host
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: false
      addresses: [10.10.111.15/22]
      routes:
        - to: default
          via: 10.10.108.1
      nameservers:
        - addresses: [172.16.16.16,172.16.16.17]
```

Apply changes
```
sudo netplan apply
	```

Check ethernet port using `ip a` command
```
ip a

ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether b2:54:db:85:1d:75 brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    inet 172.16.16.60/19 brd 172.16.31.255 scope global ens18
       valid_lft forever preferred_lft forever
    inet6 fe80::b054:dbff:fe85:1d75/64 scope link
       valid_lft forever preferred_lft forever
```