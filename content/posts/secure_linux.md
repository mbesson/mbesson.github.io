---
title: "Secure your linux VM in the cloud"
date: 2022-08-18T09:17:00+02:00
draft: false
---

## 1. Enable auto update

For manual
apt update
apt dist-update

sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades

-> Yes

## 2. Limited User Account

adduser <username>

prompt for password

usermod -aG sudo <username>

## 3. Password are for suckers

auth key pair

Give the publci key to the server
Use the private key 

on the linux server : mkdir ~/.ssh && chmod 700 ~/.ssh

on the machine : ssh-keygen -b 4096 

scp ~/.ssh/id_rsa.pub mathieu@192.168.1.38:~/.ssh/authorized_keys

## 4. Lockdown logins (harden ssh)

sudo vim /etc/ssh/sshd_config

-> change de port : by default, it is 22. Use Something random like 717
AddressFamily inet

PermitRootLogin must be change to no
PasswordAuthentication to no

sudo systemctl restart sshd

No to connect, use ssh user@ip -p 717

## 5. Firewall it up
sudo ss -tupln  if something is wierd, uninstall it.

firewaal it up

sudp apt install ufw
sudo ufw allow 717
sudo ufw enable

block pings

sudo vim /etc/ufw/before.rules
on the paragraphe for ok icmp codes for input
  -A ufw-before-input -p icmp --icmp-type echo-request -j DROP 

  and sudo reboot
