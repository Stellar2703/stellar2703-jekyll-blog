---
title: "Networking"
date: 2025-07-11 00:00:00 +0800
categories: [Networking, Infrastructure]
image: assets/images/networking.jpg
tags: [networking, ip, router, switch, tcp/ip]
---

# Networking

- IP address is a 32 bit value
- every 8 group denote an octet seperated by a .
- So Ip address range from 0.0.0.0 to 255.255.255.255

### Switch

- Sits within the LAN
- Faciliates the connection between all the devices within the LAN

### Router

- ip address of the router is nothing but the gateway
- while the switch lets one device communicate with the other within a network the router helps to communicate over globally

### Subnet (How to know whether the device is in or outside the LAN)

- Devices in the lan belong to the same ip address range

255.255.0.0   --- means 24 bits are fixed
255.255.255.0  ---- means that 16 bits are fixed

- Value 255 fixates the Octet
- Value 0 means free range

A short way of writing this subnet is called as the CIDR range
CIDR = Classless Inter-Domain Routing
 Eg : 192.168.0.0/16 or 192.168.0.0/24 where the /16 and the /24 represent the bit that are fixed

### Network Address Translation

NAT is used in the routers, when the user sends a request to outside world then the router converts the ip into a public ip and the response which comes from the public ip is converted back into the private ip and sent back to the user.

### Firewall

Protecting a network from unauthorized access

@ PORTS
- ports are like a multiple doors to the building
- there are default ports for many applications

### DNS (Domain Name Service)

-  Translates domin names to ip addresses
- top level domains
- .edu .ml .com .org .net .gov
- geographical domain like .in .de etc....


### Networking Commands

```
ifconfig

netstat   - shows active connections or ports used by the applications in computer

ps aux   - resources used by applications

nslookup   -  to get the ip  address of any domain

ping   -- to check whether an addreess or application is accessible or not
```

