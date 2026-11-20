# 🐉 Kali Linux VirtualBox Setup

A cybersecurity lab setup using Kali Linux and Oracle VirtualBox. This project covers virtual machine installation, network configuration, IPv4 settings, IP address assignment, gateway and DNS configuration, and network connectivity verification.

## 🎯 Objectives

- Install and configure Kali Linux in VirtualBox.
- Configure the virtual network.
- Understand IPv4 settings.
- Assign IP address, Gateway and DNS.
- Verify network connectivity.
- Prepare a virtual environment for cybersecurity practice.

## 🛠️ Tools & Technologies

- Oracle VirtualBox
- Kali Linux
- NetworkManager
- Linux Terminal
- IPv4 Networking

## ⚙️ Network Configuration

| Setting | Value |
|---|---|
| Connection | Wired Connection |
| IPv4 Method | Manual |
| IP Address | `10.0.0.2` |
| Netmask | `255.255.255.0` |
| Gateway | `10.0.0.1` |
| DNS | `10.0.0.1` |

**Note:** These are example settings. Use them only when they match your VirtualBox network configuration.


## 🔎 Network Verification

Run these commands in Kali Linux:

```bash
ip a
```

```bash
ip route
```

```bash
nmcli device status
```

```bash
ping -c 4 8.8.8.8


## 💡 What I Learned

- Virtual machine configuration.
- Kali Linux installation.
- IPv4 network configuration.
- IP address and Gateway settings.
- DNS configuration.
- Basic Linux networking commands.

## 🔐 Ethical Use

This laboratory is created for educational purposes and authorized cybersecurity practice only.

## 🔗 Resources

- [Kali Linux](https://www.kali.org/get-kali/)
- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## 👤 Author

**Sakshi ST**

GitHub: [@sakshiST93](https://github.com/sakshiST93)
