# Quick Start Guide

Get OctateCode up and running in 5 minutes.

## ⚡ Quick Start (5 minutes)

### Step 1: Download & Install
- Go to [OctateCode Releases](https://github.com/preetbiswas12/octatecode/releases)
- Download the version for your OS (Windows, macOS, Linux)
- Install like any other application

### Step 2: First Launch
1. Open OctateCode
2. You'll see a welcome screen
3. Choose a folder to open or create a new workspace

### Step 3: Configure an AI Provider (Optional)
1. Press `Ctrl+Shift+P` (Cmd+Shift+P on macOS)
2. Type "Void: Settings"
3. Choose an LLM provider:
   - **OpenAI** - GPT-4o, GPT-4 Turbo (requires API key)
   - **Anthropic** - Claude 3.5 (requires API key)
   - **Ollama** - Run free models locally (no key needed)
4. Paste your API key (if using cloud provider)
5. Click "Save Settings"

### Step 4: Use AI Chat
1. Press `Ctrl+L` to open chat with current file
2. Or press `Ctrl+Shift+L` for a new chat
3. Type your question, e.g., "Add error handling to this function"
4. Press Enter or click Send
5. AI responds with suggestions
6. Click "Apply" to accept code changes

### Step 5: Try Collaboration (Optional)
1. Open the "Collaboration" tab in the left sidebar
2. Click "Start Room"
3. Share your Room ID with a friend
4. They click "Join Room" and enter your ID
5. Now you can edit together in real-time!

---

## 🎮 Common Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+L` | Open chat with current file |
| `Ctrl+Shift+L` | Start new chat thread |
| `Ctrl+K` | Quick code edit |
| `Ctrl+Shift+P` | Open settings/commands |
| `Ctrl+/` | Toggle comment |
| `Alt+Z` | Toggle word wrap |

---

## 🔌 AI Provider Quick Reference

### Cloud Providers (Require API Key)

**OpenAI (Recommended for beginners)**
- Models: GPT-4o, GPT-4 Turbo, GPT-4
- Get API key: https://platform.openai.com/api-keys
- Cost: Pay per use (~$0.01-0.10 per chat)

**Anthropic Claude**
- Models: Claude Opus, Claude Sonnet
- Get API key: https://console.anthropic.com
- Cost: Pay per use (~$0.01-0.05 per chat)

**DeepSeek**
- Models: DeepSeek-V3, DeepSeek Chat
- Get API key: https://platform.deepseek.com
- Cost: Very cheap (~$0.001-0.002 per chat)

### Local Providers (Free, No Key Needed)

**Ollama (Easiest for local)**
1. Install: https://ollama.ai
2. Run: `ollama pull llama2` then `ollama serve`
3. Select "Ollama" in OctateCode settings
4. Works offline, your code never leaves your computer

**vLLM**
- More advanced, runs custom models locally
- Requires Python and GPU (or CPU)

---

## 📁 First Steps with Your Project

### Opening a Project
1. File → Open Folder
2. Select your project root directory
3. OctateCode loads your project tree on the left

### Using Chat with Your Code
1. Click a file in the explorer
2. Press `Ctrl+L`
3. Describe what you want: "Add error handling" or "Explain this function"
4. AI reads the current file and responds

### Applying Code Changes
1. AI suggests code in chat
2. Click the "Apply" button
3. Review the diff (red = removed, green = added)
4. Click "Accept" to apply or "Reject" to undo

### Real-Time Collaboration
1. Open Collaboration tab (bottom left)
2. Click "Start Room"
3. Share the Room ID with teammates
4. They join and you edit together in real-time

---

## 💡 Tips & Tricks

### Getting Better AI Responses
- **Be specific**: "Add validation for email format" vs. "Make this better"
- **Show context**: Include relevant code snippets in your question
- **Use example patterns**: "Similar to how we validate passwords..."
- **Break down complex tasks**: Multiple specific questions work better than one huge request

### Using Local AI (Offline)
- Install Ollama: https://ollama.ai
- Download a model: `ollama pull llama2` (takes 5-10 minutes)
- Your code stays on your computer
- No API keys needed

### Managing Multiple Chat Threads
- Each tab in chat sidebar is a separate conversation
- Right-click a tab to rename, delete, or export
- Switch between threads by clicking tabs

### Keyboard-Only Workflow
- `Ctrl+Shift+P` - Open command palette
- `Ctrl+L` - Chat with current file
- `Ctrl+K` - Quick code edit
- `Tab` - Switch between panels
- `Escape` - Close dialogs

---

## 🆘 Troubleshooting Quick Fixes

**Chat not working?**
- Ensure API key is set (Ctrl+Shift+P → "Void: Settings")
- Check internet connection (if using cloud provider)
- Verify model is selected in settings

**Collaboration not connecting?**
- Room ID is shared correctly
- All users on same version of OctateCode
- Check firewall (P2P requires inbound connections)

**Performance issues?**
- Close unused tabs and files
- Disable real-time features if not needed
- Run on a machine with 4+ GB RAM

**AI responses are slow?**
- Try a faster model (e.g., GPT-4 mini instead of GPT-4)
- Check internet connection speed
- Reduce file context size

---

## 📚 Next Steps

- **Learn more**: Read [User Guide](./USER_GUIDE.md)
- **Collaborate**: See [Collaboration Guide](./COLLABORATION_QUICKSTART.md)
- **Set up development**: See [Developer Setup](./DEVELOPER_SETUP.md)
- **Find solutions**: Check [Troubleshooting](./TROUBLESHOOTING.md)

---

**You're all set! Start coding with AI assistance.** 🚀

For detailed documentation, see [Documentation Index](./INDEX.md).
