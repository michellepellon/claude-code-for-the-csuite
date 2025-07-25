# Claude Code (Windows & macOS) Installation

### 1.1 Windows Installation via WSL2

```bash
# Step 1: Enable WSL2 (run in PowerShell as Administrator)
wsl --install
wsl --set-default-version 2
wsl --install -d Ubuntu-22.04

# Step 2: Launch WSL2 Ubuntu
wsl

# Step 3: Update system packages
sudo apt update && sudo apt upgrade -y

# Step 4: Install Python and pip
sudo apt install python3.10 python3-pip python3.10-venv -y

# Step 5: Install Node.js via NodeSource
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Step 6: Install Claude-Code
npm install -g @anthropic/claude-code

# Step 7: Login (via OAuth) to Claude
claude login

# Step 8: Verify installation
claude doctor
```

### 1.2 macOS Installation

```bash
# Step 1: Install Homebrew if not present
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Step 2: Install Python 3.10+
brew install python@3.10

# Step 3: Install uv
brew install uv

# Step 4: Install Node.js
brew install node@24

# Step 5: Install Claude-Code
npm install -g @anthropic/claude-code

# Step 6: Login (via OAuth) to Claude
claude login

# Step 7: Verify installation
claude doctor
```
