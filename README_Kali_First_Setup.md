::: {align="center"}
# 🐉 Kali Linux VirtualBox -- First-Time Setup

**Installing Kali Linux and configuring its network connection in Oracle
VirtualBox**
:::

```{=html}
<p align="center">
```
`<img src="https://img.shields.io/badge/OS-Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white" />`{=html}
`<img src="https://img.shields.io/badge/Virtualization-VirtualBox-183A61?style=flat-square&logo=virtualbox&logoColor=white" />`{=html}
`<img src="https://img.shields.io/badge/Network-IPv4-238F89?style=flat-square" />`{=html}
`<img src="https://img.shields.io/badge/Setup-01-C00000?style=flat-square" />`{=html}
```{=html}
</p>
```

------------------------------------------------------------------------

## 📌 Project Overview

This setup documents the first-time configuration of **Kali Linux inside
Oracle VirtualBox**. It covers opening the Kali virtual machine,
locating the NetworkManager connection editor, and configuring the wired
connection's IPv4 settings.

The aim is to prepare a working Kali Linux environment for learning
Linux networking and cybersecurity fundamentals.

> **Configuration note:** The IP values below are taken from the example
> settings in the supplied screenshot. They will work only if the
> VirtualBox virtual network uses the matching subnet and gateway. Do
> not assume they are correct for every VirtualBox network mode.

------------------------------------------------------------------------

## 🎯 Objectives

-   Start Kali Linux in Oracle VirtualBox.
-   Open the Network Connections editor.
-   Locate the wired network connection.
-   Understand Manual IPv4 configuration.
-   Set an IP address, subnet prefix, gateway, and DNS server.
-   Save and verify the network configuration.
-   Troubleshoot common save and connectivity problems.

------------------------------------------------------------------------

## 🧰 Requirements

  Component                 Requirement
  ------------------------- --------------------------
  Host computer             Windows, Linux, or macOS
  Virtualization software   Oracle VirtualBox
  Guest operating system    Kali Linux
  Network manager           NetworkManager
  Network editor            `nm-connection-editor`
  Terminal                  Kali Linux Terminal

------------------------------------------------------------------------

## 🏗️ Network Configuration

The following values match the example shown in the network-settings
screenshot.

  Setting            Example value
  ------------------ ------------------------
  Connection name    Wired connection 1
  IPv4 method        Manual
  IP address         `10.0.0.2`
  Prefix / Netmask   `24` / `255.255.255.0`
  Gateway            `10.0.0.1`
  DNS server         `10.0.0.1`

### Example network details

``` text
IP address:  10.0.0.2
Prefix:      24
Netmask:     255.255.255.0
Gateway:     10.0.0.1
DNS:         10.0.0.1
```

**Important:** Before using Manual mode, check the VirtualBox network
configuration. For example, a VirtualBox NAT Network must have a
matching network prefix and gateway. The default VirtualBox **NAT**
adapter uses DHCP and does not automatically use the example values
above. If you are using ordinary NAT and have not configured a matching
network, leave IPv4 set to **Automatic (DHCP)**.

------------------------------------------------------------------------

# 🪜 Setup Procedure

## Step 1. Start Kali Linux

1.  Open Oracle VirtualBox on your computer.
2.  Select your Kali Linux virtual machine.
3.  Click **Start**.
4.  Log in to Kali Linux.

------------------------------------------------------------------------

## Step 2. Open the Terminal

Open the Terminal from the Kali desktop, or use the keyboard shortcut:

``` text
Ctrl + Alt + T
```

Check that NetworkManager is running:

``` bash
sudo systemctl status NetworkManager
```

If it is not running, start it:

``` bash
sudo systemctl start NetworkManager
```

------------------------------------------------------------------------

## Step 3. Open Network Connections

Run:

``` bash
nm-connection-editor
```

If the command is not found, install the editor:

``` bash
sudo apt update
sudo apt install network-manager-gnome -y
```

Then run the command again:

``` bash
nm-connection-editor
```

------------------------------------------------------------------------

## Step 4. Edit the Wired Connection

1.  In the Network Connections window, select **Wired connection 1**
    (the name may differ).
2.  Click the edit/settings button.
3.  Open the **IPv4 Settings** tab.
4.  Choose the appropriate method:
    -   **Automatic (DHCP):** use this when your VirtualBox network
        provides addresses automatically.
    -   **Manual:** use this only when you have confirmed the correct
        static IP, subnet, gateway, and DNS values.

------------------------------------------------------------------------

## Step 5. Enter the IPv4 Settings

If your VirtualBox network is configured for the example subnet, choose
**Manual** and enter:

  Field              Value
  ------------------ ------------
  Address            `10.0.0.2`
  Netmask / Prefix   `24`
  Gateway            `10.0.0.1`
  DNS servers        `10.0.0.1`

Click **Save**.

If Save is unavailable or an authorization message appears, close the
editor and open it with administrator privileges:

``` bash
sudo nm-connection-editor
```

Do not change the IP values simply to make the Save button work; first
confirm that they match the VirtualBox network.

------------------------------------------------------------------------

## Step 6. Restart the Connection

Find the actual connection name:

``` bash
nmcli connection show
```

Restart the connection (replace the name if yours is different):

``` bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

If you are connected remotely to this VM, restarting the connection may
disconnect your session.

------------------------------------------------------------------------

# 🔎 Network Verification

Run these commands in the Kali Terminal.

  ---------------------------------------------------------------------------
  Test                    Command                     What to check
  ----------------------- --------------------------- -----------------------
  Show IP addresses       `ip a`                      Correct IPv4 address
                                                      appears

  Show routes             `ip route`                  Default route uses the
                                                      expected gateway

  Check NetworkManager    `nmcli device status`       Wired device is
                                                      connected

  Test gateway            `ping -c 4 10.0.0.1`        Replies from the
                                                      gateway

  Test Internet by IP     `ping -c 4 8.8.8.8`         Internet connectivity

  Test DNS                `getent hosts google.com`   A hostname resolves
  ---------------------------------------------------------------------------

A successful test depends on the actual VirtualBox network mode and its
configuration. If a test fails, check the adapter settings and gateway
rather than assuming Kali is broken.

------------------------------------------------------------------------

# 🐞 Common Problems & Solutions

## Problem 1. Save button does not save the settings

Try opening the editor with administrator privileges:

``` bash
sudo nm-connection-editor
```

If it still fails, check NetworkManager:

``` bash
sudo systemctl status NetworkManager
```

Also confirm that the connection is managed by NetworkManager:

``` bash
nmcli connection show
nmcli device status
```

## Problem 2. `nm-connection-editor` command not found

Install the package:

``` bash
sudo apt update
sudo apt install network-manager-gnome -y
```

## Problem 3. Internet stops working after setting a static IP

The IP address, prefix, gateway, and DNS must belong to the network
configured in VirtualBox.

For a normal VirtualBox **NAT** adapter, change IPv4 back to **Automatic
(DHCP)** unless you have a specific reason and a verified configuration
for static addressing.

## Problem 4. Network connection is disconnected

Check the device and connection:

``` bash
nmcli device status
nmcli connection show
```

Try reconnecting the correct connection:

``` bash
sudo nmcli connection up "Wired connection 1"
```

Use the actual connection name shown by `nmcli` if it is different.

------------------------------------------------------------------------

# 💡 What I Learned

Through this first setup, I learned:

1.  How to start Kali Linux inside VirtualBox.
2.  How to open the NetworkManager connection editor.
3.  The difference between Automatic (DHCP) and Manual IPv4
    configuration.
4.  What an IP address, subnet mask, gateway, and DNS server do.
5.  How to use `nmcli` to inspect and manage network connections.
6.  How to test network connectivity from the Kali Terminal.
7.  Why VirtualBox network settings must match the guest operating
    system's IP configuration.

------------------------------------------------------------------------

# 🔐 Security & Ethical Use

This Kali Linux virtual machine is intended for education and authorized
cybersecurity practice. Only test systems and networks that you own or
have explicit permission to assess.

------------------------------------------------------------------------

# 🔗 Tools & Resources

-   **Kali Linux:** https://www.kali.org/get-kali/
-   **Oracle VirtualBox:** https://www.virtualbox.org/wiki/Downloads
-   **Kali Linux Documentation:** https://www.kali.org/docs/
-   **NetworkManager Documentation:** https://networkmanager.dev/

------------------------------------------------------------------------

## 📸 Screenshots

Add your own screenshots to the project folder and update the image
paths below.

``` text
project-folder/
├── README.md
├── screenshots/
│   ├── 01-kali-desktop.png
│   ├── 02-network-connections.png
│   └── 03-ipv4-settings.png
```

Example Markdown image:

``` markdown
![Kali IPv4 Settings](screenshots/03-ipv4-settings.png)
```

------------------------------------------------------------------------

## 👤 Author

**Your Name**

**Project:** Kali Linux First-Time Setup\
**Environment:** Oracle VirtualBox\
**Topic:** Linux Networking and Cybersecurity Fundamentals
