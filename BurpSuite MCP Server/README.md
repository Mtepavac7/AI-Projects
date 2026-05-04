# 🔌 Connecting Burp Suite Community Edition to Claude via MCP

This guide walks you through setting up a **Model Context Protocol (MCP)** connection between **Burp Suite Community Edition** and **Claude Desktop**, enabling Claude to interact with Burp tools directly for security testing tasks.

### Prerequisites
- A machine running macOS, Windows, or Linux
- [Claude Desktop](https://claude.ai/download) installed

---

## Step 1: Download & Install Burp Suite Community Edition

Before we can set up the MCP connection, you need Burp Suite Community Edition installed on your machine.

1. Go to the official download page: [https://portswigger.net/burp/communitydownload](https://portswigger.net/burp/communitydownload)
2. Select your operating system (Windows, macOS, or Linux)
3. Download the installer and run it
4. Follow the installation wizard — default settings are fine
5. Launch Burp Suite and accept the license agreement
6. Choose **"Temporary Project"** → click **"Use Burp Defaults"** → click **"Start Burp"**

---

## Step 2: Install the MCP Server Extension via BApp Store

Burp Suite has a built-in extension marketplace called the **BApp Store** where you can find the official MCP Server extension by PortSwigger.

1. Open Burp Suite and click on the **"Extensions"** tab in the top navigation bar
2. Click on the **"BApp Store"** sub-tab
3. In the search box, type **`mcp`**
4. Select **"MCP Server"** from the results — authored by *Daniel S, PortSwigger & Daniel Allen, PortSwigger*
5. Click **"Install"** on the right-hand panel to install the extension
6. Wait for the installation to complete — it will appear under your **"Installed"** tab when done

![BApp Store MCP Search](./images/bapp-store-mcp.png)

> ℹ️ The MCP Server extension integrates Burp Suite with AI clients using the Model Context Protocol (MCP), exposing Burp functionality so AI assistants can perform security testing tasks and interact with Burp tools programmatically.

---

## Step 3: Verify Installation & Connect to Claude Desktop

### 3a: Verify the MCP Server Extension is Loaded

1. In Burp Suite, click the **"Extensions"** tab in the top navigation bar
2. Click the **"Installed"** sub-tab
3. Confirm that **"MCP Server"** appears in the list with a blue checkmark under **"Loaded"**
4. The extension type should show as **"Java"** with a **"Low"** system impact

> ✅ If you see "Extension loaded" in the Details panel at the bottom, you're good to go!

![MCP Server Installed](./images/mcp-installed.png)

### 3b: Connect to Claude Desktop

1. Click the **"MCP"** tab in the top navigation bar (added by the extension after install)
2. Scroll down to the **"Advanced Options"** section — note the default server settings:
   - **Server host:** `127.0.0.1`
   - **Server port:** `9876`
3. Scroll further down to the **"Installation"** section
4. Click **"Install to Claude Desktop"** — this automatically generates and injects the JSON config that links Burp Suite to Claude Desktop

> ℹ️ You can optionally add domains to the **"Auto-Approved HTTP Targets"** section to allow Claude to interact with specific hosts without manual approval each time.

![MCP Tab Config](./images/mcp-tab-config.png)

---

## Step 4: Restart Claude Desktop & Verify the Connection

1. Fully **quit and restart** your Claude Desktop application
2. Open Claude Desktop and navigate to **Settings → Connectors**
3. Look for **"burp"** listed as a connector with the **"LOCAL DEV"** badge — this confirms the MCP connection is active and ready
4. Click **"Configure"** if you need to adjust any connection settings

![Burp Connector in Claude](./images/burp-connector.png)

> 🎉 You're all set! Claude can now communicate directly with Burp Suite. Happy hunting!

---

## 🛠️ Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP Server not loading in Extensions | Ensure Java is installed on your machine |
| "burp" connector not appearing in Claude Desktop | Make sure you clicked "Install to Claude Desktop" **before** restarting |
| Connection refused on port 9876 | Check that Burp Suite is running and the MCP Server extension is loaded |
| Auto-approval not working | Add the target domain to the "Auto-Approved HTTP Targets" list in the MCP tab |

---

## 📚 Resources

- [Burp Suite Community Edition Download](https://portswigger.net/burp/communitydownload)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io)
- [Claude Desktop Download](https://claude.ai/download)
- [PortSwigger BApp Store](https://portswigger.net/bappstore)
