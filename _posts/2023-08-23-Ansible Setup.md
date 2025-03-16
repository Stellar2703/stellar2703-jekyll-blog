---
title: "Ansible playbook"
date: 2023-08-23 00:00:00 +0800
categories: [Automation]
tags: [Ansible]
---


```
sudo apt update
sudo apt install ansible
sudo apt install sshpass
```


- Inventory file for communication in the local machine

- A new file in VS code
```
[ubuntu]  // group name
server-01 // either name
server-02
192.168.0.100 // either address
192.168.0.1002
```

- Command to ping the users in the group
```
ansible -i ./hosts ubuntu -m ping --user someuser --ask-pass
```