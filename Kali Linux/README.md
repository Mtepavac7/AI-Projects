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

---

## Part 2 — Generating an SSH Key on Windows

Now we move to your everyday Windows machine and generate an SSH key pair, which will be used to authenticate into Kali without a password.

### Step 1 — Open PowerShell and Generate the Key

![ssh-keygen powershell](./screenshots/02_ssh_keygen.png)

```powershell
ssh-keygen
```

> Generates a public/private RSA key pair on your Windows machine using the built-in OpenSSH client.

When prompted, press **Enter** through each option to accept the defaults:

```
Enter file in which to save the key (C:\Users\username\.ssh\id_rsa): [Enter]
Enter passphrase (empty for no passphrase): [Enter]
Enter same passphrase again: [Enter]
```

This creates two files in `C:\Users\<your-username>\.ssh\`:

| File | Description |
|---|---|
| `id_rsa` | Your **private** key — never share this |
| `id_rsa.pub` | Your **public** key — this gets copied to Kali |

---

## What's Next

In **Part 3**, we'll copy the public key over to Kali and configure the MCP server so AI can assist directly with your pentesting workflows.

---

## Part 3 — Copying the Public Key to Kali Linux

Back on your Windows machine, navigate to the folder where your keys were generated:

```
C:\Users\<your-username>\.ssh\
```

Open `id_rsa.pub` with any text editor and **copy the entire contents** — you'll paste this into Kali in Step 3 below.

### Step 1 — Create the SSH Directory on Kali

![mkdir and chmod ssh directory](./screenshots/03_copy_public_key_to_kali.png)

```bash
mkdir -p ~/.ssh
```

> Creates the hidden `.ssh` directory in your home folder if it doesn't already exist; the `-p` flag prevents an error if the folder is already there.

---

### Step 2 — Set Directory Permissions to 700

```bash
chmod 700 ~/.ssh
```

> Sets the `.ssh` directory to **700** permissions — meaning only the owner can read, write, and enter the folder; no other users on the system can access it at all, which SSH requires for security.

---

### Step 3 — Paste Your Public Key into authorized_keys

```bash
nano ~/.ssh/authorized_keys
```

> Opens the `authorized_keys` file in the nano text editor, where you paste the contents of your Windows `id_rsa.pub` file so Kali knows to trust your Windows machine.

Once the file is open, paste your public key, then save and exit with `Ctrl+X` → `Y` → `Enter`.

---

### Step 4 — Set File Permissions to 600

```bash
chmod 600 ~/.ssh/authorized_keys
```

> Sets the `authorized_keys` file to **600** permissions — meaning only the owner can read and write the file; SSH will refuse to use the file if it is accessible by anyone else, as a strict security enforcement.

---

✅ **You should now be able to connect to Kali from Windows over SSH without a password:**

```powershell
ssh mirks@<kali-ip-address>
```

---

## What's Next

In **Part 4**, we'll SSH into Kali directly from Windows.

---

## Part 4 — SSH Into Kali From Windows

Almost there! Two quick steps and you're in.

### Step 1 — Get Your Kali IP Address

On your Kali machine, run:

```bash
ip a
```

> Displays all network interfaces and their assigned IP addresses; look for your IP under `eth0` or `wlan0`, it will look like `192.168.x.x`.

---

### Step 2 — Connect From Windows PowerShell

![SSH into Kali from Windows](./screenshots/04_ssh_into_kali.png)

```powershell
ssh yourusername@<kali-ip-address>
```

> Initiates an SSH connection from your Windows machine to your Kali Linux machine; enter your password when prompted and you're in.

---

✅ **You should now see the Kali prompt inside your Windows PowerShell — you are connected!**

```
(mirks@kali)-[~]
$
```

---

## What's Next

In **Part 5**, we'll set up the **MCP server** on Kali.

---

## Part 5 — Installing the MCP Server on Kali Linux

The MCP server is what allows AI to communicate directly with your Kali machine. You can run this step either from **within Kali** directly, or **remotely from your Windows machine** via the SSH session we set up in Part 4 — your choice.

### Step 1 — Install the MCP Kali Server

![sudo apt install mcp-kali-server](./screenshots/05_install_mcp_server.png)

```bash
sudo apt install -y mcp-kali-server
```

> Downloads and installs the `mcp-kali-server` package and all of its dependencies onto your Kali machine; the `-y` flag automatically confirms the installation so you don't need to approve each prompt.

> **📝 Note:** If this is your first time installing `mcp-kali-server`, `apt` will pull in a number of supporting libraries automatically — this is expected behavior, as shown in the screenshot above.

---

> ⚠️ **Security Reminder:** Always mask or blur sensitive information such as IP addresses, usernames, and hostnames in screenshots before publishing — use a tool like Paint, Flameshot, or your screenshot editor to redact them with a solid block.

---

## What's Next

In **Part 6**, we'll configure Claude Desktop to connect to the MCP server.

---

## Part 6 — Configuring Claude Desktop to Use the MCP Server

Now we wire everything together by telling Claude Desktop how to reach your Kali machine over SSH.

### Step 1 — Open Claude Desktop Settings

1. Open the **Claude Desktop** app on Windows
2. Click the **hamburger menu** (☰) in the top left
3. Go to **Settings** → **Developer**

### Step 2 — Edit the Config File

Inside the Developer section, locate and open the `claude_desktop_config.json` file. Replace the entire contents with the following:

![claude_desktop_config.json](./screenshots/06_claude_desktop_config.png)

```json
{
  "mcpServers": {
    "mcp-kali-server": {
      "command": "ssh",
      "args": [
        "yourusername@192.168.x.x",
        "mcp-server"
      ],
      "transport": "stdio"
    }
  }
}
```

> Configures Claude Desktop to connect to your Kali MCP server by opening an SSH connection using the `stdio` transport, which lets Claude send and receive commands directly through the SSH tunnel.

> **📝 Replace `yourusername@192.168.x.x`** with your actual Kali username and IP address from `ip a` in Part 4.

---

### Step 3 — Save and Restart Claude Desktop

Save the file, then **fully close and reopen Claude Desktop** so it picks up the new config.

✅ **Claude Desktop is now connected to your Kali MCP server — AI can now assist directly with your pentesting workflows!**

---

---

*Part of the AI-Assisted Pentesting & Security Engineering series.*
