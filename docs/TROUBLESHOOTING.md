# Troubleshooting Guide

Solutions to common OctateCode issues and problems.

---

## 🔴 Chat & AI Issues

### Chat Window Shows "Loading..." Indefinitely

**Possible Causes:**
- API key not set or invalid
- Network connectivity issue
- API provider is down
- Model selection is incorrect

**Solutions:**
1. Verify API key:
   - `Ctrl+Shift+P` → "Void: Settings"
   - Check provider is selected
   - Confirm API key is pasted correctly
2. Test internet connection:
   - Try accessing https://www.google.com in browser
   - If offline, collaborate feature works locally
3. Switch provider:
   - Try a different model or provider
   - Test with Ollama (local, no internet needed)
4. Check provider status:
   - Visit provider's status page (OpenAI.com/status, etc.)
   - Wait if provider is experiencing outages

### "Invalid API Key" Error

**Solutions:**
1. Regenerate your API key:
   - OpenAI: https://platform.openai.com/api-keys
   - Anthropic: https://console.anthropic.com
   - DeepSeek: https://platform.deepseek.com
2. Copy key again (avoid typos):
   - Use provider's "Copy" button if available
   - Paste into OctateCode settings
3. Clear old key:
   - Go to settings
   - Clear the key field completely
   - Paste new key
   - Save settings

### Chat Response is Gibberish or Nonsensical

**Causes:**
- Model selection too small/weak
- Temperature setting too high
- Context window exceeded
- Provider API issue

**Solutions:**
1. Switch to a stronger model:
   - Try GPT-4o instead of GPT-4 mini
   - Try Claude Opus instead of Haiku
2. Reduce context size:
   - Include smaller files in chat
   - Remove unnecessary code context
   - Start a new chat thread
3. Lower temperature:
   - Settings → Advanced → Temperature
   - Set to 0.7 (more deterministic responses)
4. Clear chat history:
   - Right-click chat tab → Delete
   - Start fresh conversation

### "Context window exceeded" Error

**Solutions:**
1. Remove files from context:
   - Don't include entire large files
   - Select specific functions instead
   - Use smaller snippets
2. Start new chat:
   - Create new chat thread (Ctrl+Shift+L)
   - Old context won't carry over
3. Switch to larger context model:
   - Claude Opus: 200K tokens
   - GPT-4 Turbo: 128K tokens
   - Upgrade from GPT-4 mini

### Apply Button Grayed Out or Not Working

**Causes:**
- No code changes in response
- File not editable (read-only)
- File is binary or special type
- Model didn't generate code

**Solutions:**
1. Check response contains code:
   - Verify LLM provided code suggestions
   - Ask more specific code change requests
   - Provide code examples in your question
2. Verify file is editable:
   - File should be plain text (.ts, .js, .py, etc.)
   - Right-click file → "Make Editable"
3. Try different model:
   - Use a model known for good code generation
   - Try GPT-4o or Claude Opus

---

## 🔗 Collaboration Issues

### Can't Connect to Room

**Possible Causes:**
- Room ID is incorrect
- Room doesn't exist
- Firewall blocking P2P connections
- Both users not online simultaneously

**Solutions:**
1. Double-check Room ID:
   - Exact match (case-sensitive)
   - No extra spaces
   - Share via copy button in UI
2. Verify host is online:
   - Room creator must have OctateCode open
   - Check with host via chat/phone first
3. Check firewall:
   - Windows: Settings → Firewall → Allow app through firewall
   - macOS: System Preferences → Security & Privacy → Firewall
   - Linux: Check `ufw` or `firewalld` status
4. If still failing:
   - Try again after 30 seconds (connection retry)
   - Restart OctateCode
   - Restart computer

### Collaborator's Changes Not Appearing

**Causes:**
- Network interruption
- Too many conflicts
- File sync lag
- File was edited locally after peer edit

**Solutions:**
1. Check connection status:
   - Bottom status bar should show "Connected"
   - If red, connection was lost
2. Refresh files:
   - Close and reopen the file
   - Check File Explorer to see latest version
3. Resolve conflicts:
   - If both edited same line, later edit wins
   - Manually merge if needed
4. Restart sync:
   - Leave room (click "Leave Room")
   - Rejoin room (click "Join Room")
   - Files will re-sync

### Terminal Not Syncing Across Peers

**Causes:**
- Terminal owned by different peer
- Complex terminal output too large
- Some terminal commands not portable

**Solutions:**
1. Only terminal owner can execute:
   - Only room creator can run commands
   - Other users can see output only
   - This is by design for security
2. If you're not owner:
   - Ask room creator to run commands
   - Or become room creator (leave and recreate)
3. Keep terminal commands simple:
   - Complex colored output might not display
   - Use plain text commands
   - Avoid interactive prompts

### Chat in Collaboration Room Not Working

**Causes:**
- Chat requires internet (even in P2P mode)
- Different provider selected than room host
- Chat message too large

**Solutions:**
1. Verify internet connection:
   - Even P2P collaboration needs some internet
   - Check if web pages load
2. Coordinate provider:
   - Both users should use same model
   - Agree on provider before starting room
3. Keep messages concise:
   - Very large files might not sync
   - Split into multiple smaller messages

---

## ⚙️ Settings & Configuration Issues

### Settings Won't Save

**Causes:**
- Insufficient permissions
- Storage service corrupted
- Settings file locked

**Solutions:**
1. Check permissions:
   - Ensure user folder is writable
   - Windows: Right-click folder → Properties → Security
   - macOS/Linux: `chmod u+w ~/.config/` (or equivalent)
2. Restart OctateCode:
   - Close completely (`Alt+F4` or Cmd+Q)
   - Reopen application
3. Reset settings:
   - Delete settings file:
     - Windows: `%APPDATA%\Code\user-data\`
     - macOS: `~/Library/Application Support/Code/`
     - Linux: `~/.config/Code/`
   - Restart OctateCode (will recreate defaults)

### Provider Not Appearing in Dropdown

**Causes:**
- Provider not installed
- Configuration incomplete
- Wrong version of OctateCode

**Solutions:**
1. Update OctateCode:
   - Check for updates: Help → Check for Updates
   - Download latest from GitHub
2. Verify provider support:
   - Check [Chat.md](./CHAT.md) for supported providers
   - Some providers require extensions
3. Manually add provider:
   - Open settings.json
   - Add provider configuration manually
   - Check [API_REFERENCE.md](./API_REFERENCE.md) for format

---

## 🖥️ Performance Issues

### OctateCode Running Slowly

**Causes:**
- Too many open files
- Large files being edited
- Insufficient RAM
- Background extensions
- Slow computer

**Solutions:**
1. Close unnecessary files:
   - Close tabs you're not using
   - Ctrl+K Ctrl+W to close all tabs
2. Disable extensions:
   - Ctrl+Shift+P → "Extensions: Disable All"
   - Re-enable one by one to find culprit
3. Reduce file size:
   - Split large files into multiple smaller files
   - Consider compression if working with data files
4. Increase available memory:
   - Close other applications
   - Restart browser/other memory hogs
5. Upgrade hardware:
   - More RAM helps significantly
   - SSD much faster than HDD
   - Faster CPU helps

### Chat Responses Very Slow

**Causes:**
- Slow internet connection
- Model is powerful but slow (e.g., GPT-4)
- Large context window (many files)
- Provider overloaded

**Solutions:**
1. Switch to faster model:
   - Use GPT-4o mini instead of GPT-4
   - Use Haiku instead of Opus
   - Use vLLM with local GPU
2. Reduce context:
   - Don't include entire files
   - Only add necessary code snippets
   - Start fresh chat thread
3. Check internet speed:
   - Test at speedtest.net
   - Needs at least 1 Mbps for streaming
   - Unstable connections cause delays

### High CPU Usage

**Causes:**
- Active compilation/watch process
- Real-time linting
- Too many file watchers
- Collaboration with many peers

**Solutions:**
1. Pause watchers:
   - Stop npm build tasks if running
   - Disable auto-save if enabled
2. Disable linting:
   - Settings → Linting → Enabled (toggle off)
   - Re-enable when needed
3. Limit file watchers:
   - Settings → Files: Exclude
   - Add folders with many files
   - `.git`, `node_modules`, `dist` are good candidates
4. Reduce collaboration syncing:
   - Limit number of simultaneously editing users
   - All users should close large files

---

## 🐛 UI/Visual Issues

### Text Overlapping or Rendering Poorly

**Causes:**
- Font issue
- DPI scaling on Windows
- GPU acceleration problem

**Solutions:**
1. Change font:
   - Settings → Font Family
   - Try "Monospace" or "Consolas"
2. Disable GPU acceleration:
   - Close OctateCode
   - Start with flag: `code.exe --disable-gpu`
3. Adjust zoom:
   - Ctrl+= to zoom in
   - Ctrl+- to zoom out
   - Ctrl+0 to reset

### Colors Not Displaying Correctly

**Causes:**
- Theme not applied
- Color extension issue
- Wrong color scheme

**Solutions:**
1. Reset theme:
   - Settings → Theme
   - Choose theme and apply
2. Try different theme:
   - Settings → Theme → Available themes
   - Pick a different one
3. Try light vs dark:
   - Settings → Theme → Light/Dark theme
   - See if colors improve

### Chat Sidebar Not Appearing

**Causes:**
- Sidebar hidden
- Layout corrupt
- Sidebar tab not visible

**Solutions:**
1. Toggle sidebar:
   - Press Ctrl+B to show/hide sidebar
   - Or View → Sidebar → Toggle Sidebar
2. Reset layout:
   - Close OctateCode
   - Delete layout cache (in user settings folder)
   - Restart application
   - Sidebar should reappear

---

## 🔑 API Key Problems

### "Unauthorized" When Using API Key

**Causes:**
- Invalid API key
- API key for wrong provider
- API key has insufficient permissions
- API key rate limited

**Solutions:**
1. Verify correct API key:
   - Generate new key from provider console
   - Copy the entire key (no spaces)
   - Check provider matches setting
2. Check usage limits:
   - OpenAI: https://platform.openai.com/usage
   - Anthropic: https://console.anthropic.com
   - See if you're out of credits or rate limited
3. Wait and retry:
   - Rate limits reset after minute/hour
   - Try again after waiting

### API Key Not Being Saved

**Causes:**
- Settings dialog closed before saving
- Permissions issue
- Browser cache issue

**Solutions:**
1. Explicitly save:
   - After entering key, click "Save Settings" button
   - Confirm in UI that key was saved
2. Clear cache:
   - Settings → Clear Provider Cache
   - Re-enter key and save
3. Check storage:
   - Verify storage service is working
   - Try test with another API key

---

## 📁 File & Project Issues

### Cannot Create/Edit Files

**Causes:**
- Project folder read-only
- Insufficient permissions
- Disk full

**Solutions:**
1. Check permissions:
   - Right-click folder → Properties → Security
   - Ensure user has write access
2. Check disk space:
   - Free up space on hard drive
   - Delete temporary files
3. Move to writable location:
   - Copy project to new folder with write access
   - Open new folder in OctateCode

### Files Not Appearing in Explorer

**Causes:**
- Folder not added to workspace
- Files in excluded folders
- Hidden files

**Solutions:**
1. Add folder to workspace:
   - File → Add Folder to Workspace
   - Select folder
   - Folder appears in Explorer
2. Check exclude settings:
   - Settings → Files: Exclude
   - Your files might be excluded here
3. Show hidden files:
   - Settings → Files: Exclude
   - Find `.*` pattern and remove it

---

## 🌐 Network Issues

### No Internet Connection

**Impacts:**
- Cloud AI providers won't work
- Collaboration won't sync
- Settings won't save to cloud

**Workarounds:**
1. Use local models:
   - Install Ollama (offline models)
   - Run vLLM locally
   - Use LM Studio
2. Use offline collaboration:
   - P2P collaboration still works
   - Other peer must be on local network
3. Save work locally:
   - All files saved to disk
   - Syncs when internet returns

### Connection Keeps Dropping

**Causes:**
- Unstable WiFi
- VPN disconnection
- ISP issues
- Network timeout settings

**Solutions:**
1. Switch to stable connection:
   - Use ethernet instead of WiFi
   - Switch to different WiFi network
   - Restart router
2. Check network stability:
   - Run `ping google.com` (continuous)
   - Look for packet loss (should be 0%)
3. Increase timeout:
   - Settings → Network Timeout
   - Set to 30+ seconds for unreliable networks
4. Use local mode:
   - For collaboration, ensure local network is stable
   - Use local models that don't need internet

---

## 📞 Getting More Help

**For issues not listed here:**

1. **Check the FAQ**: [FAQ.md](./FAQ.md) - Common questions
2. **Read the Docs**: [User Guide](./USER_GUIDE.md) - Comprehensive guide
3. **GitHub Issues**: https://github.com/preetbiswas12/octatecode/issues
   - Search existing issues first
   - Create new issue with details
   - Include error messages and OS info
4. **GitHub Discussions**: https://github.com/preetbiswas12/octatecode/discussions
   - Ask community for help
   - Share solutions with others

---

## 🆘 Debug Information to Gather

When reporting bugs, collect this info:

```
- OS (Windows 10/11, macOS version, Linux distribution)
- OctateCode version (Help → About)
- Provider/Model being used
- API Key status (valid / invalid / not set)
- Error message (exact text)
- Steps to reproduce (clear sequence)
- Screenshots (if visual issue)
- Console logs (Help → Toggle Developer Tools)
```

---

**Most issues can be resolved with the steps above. Feel free to ask for help!** 💪

For more details, see [Documentation Index](./INDEX.md).
