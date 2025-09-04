# Node.js Installation Guide 🚀

Welcome to the complete Node.js installation guide! This handbook will walk you through installing Node.js on different operating systems with step-by-step instructions and helpful examples.

## What is Node.js? 🤔

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. It allows you to run JavaScript on your computer (outside of a web browser) and build server-side applications.

> **Fun Fact:** Node.js was created in 2009 and has become one of the most popular platforms for web development!

## System Requirements 📋

| Operating System | Minimum Requirements |
|------------------|---------------------|
| **Windows** | Windows 10 or later (64-bit) |
| **macOS** | macOS 10.15 Catalina or later |
| **Linux** | Most modern distributions |

**Hardware Requirements:**
- 🖥️ **RAM:** 4GB minimum (8GB recommended)
- 💾 **Storage:** 200MB free space
- 🔧 **Architecture:** 64-bit processor

## Installation Methods 🛠️

There are several ways to install Node.js:

| Method | Best For | Pros | Cons |
|--------|----------|------|------|
| **Official Installer** | Beginners | Easy, straightforward | Single version |
| **Package Managers** | Advanced users | Quick updates | Requires package manager |
| **Version Managers** | Developers | Multiple versions | More complex setup |

## Windows Installation 🪟

### Method 1: Official Installer (Recommended for beginners)

1. **Download Node.js**
   - Visit [nodejs.org](https://nodejs.org)
   - Click on the **LTS version** (Long Term Support)
   
   ![Download Page Example](https://via.placeholder.com/600x300/4CAF50/FFFFFF?text=nodejs.org+Download+Page)

2. **Run the Installer**
   - Double-click the downloaded `.msi` file
   - Click "Next" through the installation wizard
   
   ```
   ✅ Accept License Agreement
   ✅ Choose Installation Location (default is fine)
   ✅ Select Components (keep all selected)
   ✅ Install
   ```

3. **Installation Progress**
   - Wait for the installation to complete
   - Click "Finish"

### Method 2: Using Chocolatey

```powershell
# Install Chocolatey first (if not installed)
# Run PowerShell as Administrator
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install Node.js
choco install nodejs
```

### Method 3: Using Winget

```cmd
# Open Command Prompt or PowerShell
winget install OpenJS.NodeJS
```

## macOS Installation 🍎

### Method 1: Official Installer

1. **Download Node.js**
   - Visit [nodejs.org](https://nodejs.org)
   - Download the `.pkg` file for macOS

2. **Install**
   - Double-click the `.pkg` file
   - Follow the installation wizard
   - Enter your password when prompted

### Method 2: Using Homebrew (Recommended)

```bash
# Install Homebrew (if not installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Node.js
brew install node
```

### Method 3: Using MacPorts

```bash
sudo port install nodejs18 +universal
```

## Linux Installation 🐧

### Method 1: Using Package Managers

#### Ubuntu/Debian
```bash
# Update package index
sudo apt update

# Install Node.js
sudo apt install nodejs npm

# Or install latest version
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

#### CentOS/RHEL/Fedora
```bash
# For DNF (Fedora)
sudo dnf install nodejs npm

# For YUM (CentOS/RHEL)
sudo yum install nodejs npm
```

#### Arch Linux
```bash
sudo pacman -S nodejs npm
```

### Method 2: Using Snap
```bash
sudo snap install node --classic
```

## Version Managers (All Platforms) 🔄

### Node Version Manager (NVM)

**For Linux/macOS:**
```bash
# Install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Reload terminal
source ~/.bashrc

# Install latest LTS Node.js
nvm install --lts
nvm use --lts
```

**For Windows (nvm-windows):**
1. Download from [GitHub releases](https://github.com/coreybutler/nvm-windows/releases)
2. Run the installer
3. Open new Command Prompt:

```cmd
# Install latest LTS
nvm install lts
nvm use lts
```

## Verification ✅

After installation, verify Node.js and npm are working:

### Check Versions
```bash
# Check Node.js version
node --version
# Expected output: v18.17.0 (or similar)

# Check npm version
npm --version
# Expected output: 9.6.7 (or similar)
```

### Test Node.js
Create a simple test file:

```javascript
// test.js
console.log("Hello, Node.js!");
console.log("Node.js version:", process.version);
```

Run the test:
```bash
node test.js
```

**Expected Output:**
```
Hello, Node.js!
Node.js version: v18.17.0
```

### Test npm
```bash
# Check npm configuration
npm config list

# Test npm by installing a package globally
npm install -g http-server
```

## Troubleshooting 🔧

### Common Issues and Solutions

#### Issue 1: "node" command not found
```bash
# Check if Node.js is in PATH
echo $PATH

# For Windows, restart Command Prompt
# For Linux/macOS, reload terminal or run:
source ~/.bashrc
```

#### Issue 2: Permission errors (Linux/macOS)
```bash
# Fix npm permissions
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### Issue 3: Old version installed
```bash
# Uninstall old version
# Windows: Use Control Panel or installer
# macOS: Use Homebrew or delete manually
# Linux: Use package manager

# Then reinstall using methods above
```

### Getting Help

- 📚 **Documentation:** [nodejs.org/docs](https://nodejs.org/docs)
- 💬 **Community:** [nodejs.org/community](https://nodejs.org/community)
- 🐛 **Issues:** [GitHub Issues](https://github.com/nodejs/node/issues)

## Next Steps 🎯

Congratulations! You've successfully installed Node.js. Here's what to do next:

### 1. **Learn the Basics**
```javascript
// Create your first Node.js app
const fs = require('fs');
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('<h1>Hello from Node.js!</h1>');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

### 2. **Initialize Your First Project**
```bash
# Create project directory
mkdir my-first-node-app
cd my-first-node-app

# Initialize npm project
npm init -y

# Install a package
npm install express
```

### 3. **Explore Package Management**
```bash
# Install packages locally
npm install package-name

# Install packages globally
npm install -g package-name

# Save as development dependency
npm install --save-dev package-name
```

### 4. **Useful Commands to Remember**
```bash
# Run JavaScript files
node filename.js

# Check npm packages
npm list
npm list -g

# Update packages
npm update

# Check for outdated packages
npm outdated
```

---

## 🎉 You're Ready to Code!

You now have Node.js installed and ready to use! Start building amazing applications with JavaScript on the server-side.

### Quick Reference Card

| Command | Purpose |
|---------|---------|
| `node -v` | Check Node.js version |
| `npm -v` | Check npm version |
| `node file.js` | Run JavaScript file |
| `npm init` | Create new project |
| `npm install` | Install dependencies |

---

**Happy Coding!** 💻✨

> **Tip:** Bookmark this guide for future reference and share it with fellow developers who are starting their Node.js journey!
