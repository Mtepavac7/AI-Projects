# AI-Assisted Pentesting & Security Engineering
## Part 1 — Kali Linux Setup & SSH Configuration

> **Series Goal:** Learn how to use AI (via MCP server) to assist with Linux Kali and penetration testing / security engineering workflows — bridging your Windows machine directly to Kali over SSH.

---

## Prerequisites

- Kali Linux installed (bare metal, VM, or WSL2)
- A user account with `sudo` privileges
- Network connectivity between your Windows host and Kali machine

---

## Step 1 — Update Package Lists

![sudo apt update](./screenshots/01_apt_update.png)

```bash
sudo apt update
```

> Refreshes the local package index so `apt` knows about the latest available versions of all installable software.

---

## Step 2 — Install OpenSSH Server

![sudo apt install openssh-server](./screenshots/01_apt_install_openssh.png)

```bash
sudo apt install -y openssh-server
```

> Downloads and installs the OpenSSH server package, which allows other machines to connect to Kali remotely over an encrypted SSH connection; the `-y` flag auto-confirms the installation prompt.

---

## Step 3 — Enable & Start the SSH Service

![sudo systemctl enable --now ssh](./screenshots/01_systemctl_enable_ssh.png)

```bash
sudo systemctl enable --now ssh
```

> Enables the SSH service to start automatically on every boot **and** starts it immediately in a single command, so you don't need to reboot before connecting.

---

## Verify SSH is Running

You can confirm the service is active with:

```bash
sudo systemctl status ssh
```

You should see `Active: active (running)` in the output.

---

## Find Your Kali IP Address

You'll need this to connect from Windows:

```bash
ip a
```

Look for the IP under your network interface (e.g., `eth0` or `wlan0`) — it will look like `192.168.x.x`.

---

## What's Next

In **Part 2**, we'll configure the **MCP server on Windows** and connect it to this Kali SSH session so AI can assist directly with your pentesting workflows.

---

*Part of the AI-Assisted Pentesting & Security Engineering series.*
