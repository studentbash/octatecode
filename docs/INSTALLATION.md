# Installation & Configuration Guide

Complete guide to installing OctateCode and configuring for your first use.

---

## 📥 Installation

### Windows

1. **Download Installer**
   - Visit [OctateCode Releases](https://github.com/preetbiswas12/octatecode/releases)
   - Download `OctateCode-Setup-x.x.x.exe` (Windows 10/11)
   - Or `OctateCode-Setup-arm64-x.x.x.exe` for ARM64

2. **Run Installer**
   - Double-click the `.exe` file
   - Follow the installer wizard
   - Choose installation location (default: `C:\Users\USERNAME\AppData\Local\Programs\OctateCode`)
   - Allow Windows to handle the installation

3. **Launch**
   - Installer creates Start Menu shortcut
   - Or run from Start Menu → "OctateCode"

4. **Uninstall**
   - Settings → Apps → Apps & Features
   - Find "OctateCode"
   - Click → Uninstall

### macOS

1. **Download**
   - Visit [OctateCode Releases](https://github.com/preetbiswas12/octatecode/releases)
   - Download `OctateCode-x.x.x-universal.dmg` (Intel + Apple Silicon)

2. **Install**
   - Double-click the `.dmg` file
   - Drag "OctateCode" to Applications folder
   - Wait for copy to complete (~1 minute)

3. **Launch**
   - Open Applications folder
   - Double-click OctateCode
   - Or use Spotlight: Cmd+Space → type "OctateCode" → Enter

4. **First Launch**
   - macOS may show security warning
   - Click "Open" to proceed (required for first launch only)

5. **Uninstall**
   - Drag OctateCode from Applications to Trash
   - Empty Trash

### Linux

**Ubuntu/Debian:**
```bash
# Download .deb file
wget https://github.com/preetbiswas12/octatecode/releases/download/...OctateCode-x.x.x.deb

# Install
sudo dpkg -i OctateCode-x.x.x.deb

# Launch
octatecode
# Or find in Applications → Development → OctateCode

# Uninstall
sudo apt remove octatecode
```

**Fedora/RHEL:**
```bash
# Download .rpm file
wget https://github.com/preetbiswas12/octatecode/releases/download/...OctateCode-x.x.x.rpm

# Install
sudo rpm -i OctateCode-x.x.x.rpm

# Launch
octatecode

# Uninstall
sudo rpm -e octatecode
```

**Arch Linux:**
```bash
# Via AUR (if available)
yay -S octatecode

# Or install .tar.gz manually
tar -xzf OctateCode-x.x.x-linux-x64.tar.gz
./OctateCode/bin/octatecode
```

---

## ⚙️ First Configuration

### 1. Open OctateCode

Launch the app. You'll see the Welcome screen with setup options.

### 2. Select Workspace Folder

**Option A: Open Existing Folder**
- Click "Open Folder"
- Navigate to your project directory
- Click "Select"
- Your project loads in the left sidebar

**Option B: Create New Workspace**
- Click "Create New Folder"
- Choose location and name
- New empty workspace opens

**Option C: Clone from GitHub**
- Click "Clone Repository"
- Paste GitHub URL
- Choose local folder
- Wait for clone to complete

### 3. Configure AI Provider (Optional)

Press `Ctrl+Shift+P` (Cmd+Shift+P on macOS) or click gear icon in bottom left.:

1. Type "Void: Settings"
2. Click on "Void: Settings"
3. Settings panel opens on the right

### 4. Select/Add AI Provider

Choose from dropdown:
- **OpenAI** (GPT-4o, GPT-4 Turbo, GPT-4)
- **Anthropic** (Claude 3.5 Opus/Sonnet)
- **DeepSeek** (DeepSeek-V3, Chat)
- **Google** (Gemini 2.0, 1.5 Pro)
- **Ollama** (Local models)
- **vLLM** (Custom local models)
- And 8+ others

### 5. Add API Key (if using cloud provider)

**For OpenAI:**
1. Go to https://platform.openai.com/api-keys
2. Click "Create new secret key"
3. Copy the key
4. Paste into OctateCode settings
5. Click Save

**For Anthropic:**
1. Go to https://console.anthropic.com
2. Navigate to API Keys
3. Create and copy key
4. Paste into OctateCode settings
5. Click Save

**For Other Providers:**
- See provider's documentation link in OctateCode settings
- Each provider has "Get API Key" link

### 6. Select Model

After adding provider:
1. Choose Model dropdown
2. Select a model (e.g., "gpt-4o")
3. Settings auto-detect model capabilities
4. Click Save

### 7. Test Configuration

1. Open a code file
2. Press `Ctrl+L` to open chat
3. Ask: "Hello, can you help?"
4. If AI responds, you're configured! ✅

---

## 🛠️ Recommended Settings

### For General Use

```
Provider: OpenAI or Anthropic
Model: gpt-4o (or claude-3.5-sonnet)
Temperature: 0.7
Max Tokens: 4000
```

### For Fast Responses

```
Provider: DeepSeek or Ollama
Model: DeepSeek-V3 or Llama2
Temperature: 0.5
Max Tokens: 2000
```

### For Local/Offline Use

```
Provider: Ollama
Model: llama2 or mistral
Temperature: 0.7
Max Tokens: 2000
```

### For High Quality

```
Provider: Anthropic or OpenAI
Model: Claude Opus or GPT-4 Turbo
Temperature: 0.3
Max Tokens: 8000
```

---

## 🌐 Local AI Setup

### Ollama (Easiest)

**Install:**
1. Go to https://ollama.ai
2. Download for your OS
3. Run installer
4. Open Terminal and run: `ollama serve`

**Download a Model:**
```bash
ollama pull llama2      # ~4GB
ollama pull mistral     # ~4GB
ollama pull neural-chat # ~4GB
```

**Use in OctateCode:**
1. Keep `ollama serve` running
2. Open OctateCode settings
3. Select "Ollama" provider
4. Select model name
5. Save and test

### vLLM (Advanced)

**Requirements:**
- Python 3.10+
- GPU recommended (NVIDIA, AMD, or Intel Arc)
- 16+ GB RAM (for local models)

**Install:**
```bash
pip install vllm
```

**Run:**
```bash
# With GPU (NVIDIA)
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-2-7b-hf \
  --gpu-memory-utilization 0.9

# CPU only (slower)
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-2-7b-hf \
  --device cpu
```

**Use in OctateCode:**
1. Keep vLLM running in terminal
2. Open OctateCode settings
3. Select "vLLM" provider
4. Endpoint: `http://localhost:8000`
5. Save and test

### LM Studio (GUI)

**Install:**
1. Visit https://lmstudio.ai
2. Download LM Studio
3. Run installer
4. Open LM Studio app

**Download Model:**
1. Click Search/Browse
2. Search for model (e.g., "Mistral")
3. Click Download
4. Wait for completion

**Start Server:**
1. Click Local Server
2. Select model
3. Click "Load model"
4. Server starts automatically on `http://localhost:1234`

**Use in OctateCode:**
1. Open OctateCode settings
2. Select "LM Studio" provider
3. Endpoint: `http://localhost:1234`
4. Save and test

---

## 🔧 Troubleshooting Installation

### App Crashes on Launch

**Windows:**
1. Uninstall via Control Panel
2. Delete `%APPDATA%\OctateCode` folder
3. Reinstall fresh

**macOS:**
1. Drag OctateCode from Applications to Trash
2. Delete `~/Library/Application Support/OctateCode`
3. Reinstall fresh

**Linux:**
1. Uninstall: `sudo apt remove octatecode` (or equivalent)
2. Delete `~/.config/OctateCode`
3. Reinstall

### Permission Denied (Linux)

```bash
# Make binary executable
chmod +x ~/OctateCode/bin/octatecode

# Or use package manager (recommended)
sudo apt install octatecode
```

### API Key Not Working

1. Verify key is correct at provider website
2. Check API key has sufficient quota/credits
3. Copy key again (avoid typos)
4. Save settings
5. Restart OctateCode
6. Test in chat

### Can't Connect to Local Models

1. Ensure server is running:
   - Ollama: `ollama serve` running in terminal
   - vLLM: Python process running
   - LM Studio: App open with Local Server started
2. Check endpoint:
   - Ollama: `http://127.0.0.1:11434`
   - vLLM: `http://localhost:8000`
   - LM Studio: `http://localhost:1234`
3. Verify firewall allows localhost connections

---

## 📁 Configuration Files

### Settings Location

**Windows:**
```
%APPDATA%\Code\User\settings.json
C:\Users\USERNAME\AppData\Roaming\Code\User\settings.json
```

**macOS:**
```
~/Library/Application Support/Code/User/settings.json
```

**Linux:**
```
~/.config/Code/User/settings.json
```

### Backup Settings

1. Locate your settings file (above)
2. Make a copy: `settings.json.backup`
3. Store safely

### Restore Settings

1. Replace current `settings.json` with backup
2. Restart OctateCode

---

## 🔐 Security Notes

- ✅ API keys are never logged
- ✅ Keys only sent to your chosen provider
- ✅ Keys stored encrypted on your computer
- ✅ OctateCode has no access to your keys
- ⚠️ Keep your computer secure
- ⚠️ Don't share your API keys
- ⚠️ Rotate keys periodically

See [Security Guide](./SECURITY.md) for full details.

---

## ✅ Verification Checklist

After installation:

- [ ] App launches without errors
- [ ] Can open a project folder
- [ ] Can create a new file
- [ ] Can edit and save files
- [ ] (If AI enabled) Chat sends messages
- [ ] (If AI enabled) AI responds with suggestions

If any item fails, check [Troubleshooting](./TROUBLESHOOTING.md).

---

## 🚀 Next Steps

1. **Learn Basics**: [Quick Start](./QUICK_START.md)
2. **Explore Features**: [User Guide](./USER_GUIDE.md)
3. **Try AI Chat**: Press `Ctrl+L` and ask a question
4. **Collaborate**: See [Collaboration Guide](./COLLABORATION_QUICKSTART.md)

---

**Installation complete! You're ready to code with AI assistance.** 🎉
