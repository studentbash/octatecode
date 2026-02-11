# Frequently Asked Questions (FAQ)

Answers to common questions about OctateCode.

---

## 💰 Cost & Pricing

### Is OctateCode Free?

**Yes!** OctateCode itself is completely free and open-source (MIT license).

However:
- **Cloud AI providers** charge for API usage (OpenAI, Anthropic, etc.)
- **Local models** (Ollama, vLLM) are free to use
- You control and pay for any API keys directly with providers

### How Much Will AI Cost Me?

**Varies by provider and usage:**

| Provider | Model | Cost per Chat |
|----------|-------|---------------|
| OpenAI | GPT-4o | $0.05-0.15 |
| OpenAI | GPT-4o mini | $0.001-0.003 |
| Anthropic | Claude Opus | $0.02-0.06 |
| Anthropic | Claude Haiku | $0.0008-0.0024 |
| DeepSeek | DeepSeek-V3 | $0.001-0.003 |

**Budget tips:**
- Use cheaper models like GPT-4o mini or Haiku
- Use local models (Ollama) - completely free
- Set daily API spend limits in provider console
- Use search models for quick tasks, powerful models for complex tasks

### Can I Use Local Models?

**Absolutely!** Three options:

1. **Ollama** (easiest)
   - Download: https://ollama.ai
   - Free models: LLaMA, Mistral, Phi, etc.
   - No internet needed, data stays local

2. **vLLM** (more powerful)
   - Requires Python and GPU (or CPU)
   - Run any HuggingFace model
   - Better performance than Ollama

3. **LM Studio** (convenient)
   - GUI for downloading models
   - Simpler than vLLM
   - Good for beginners

---

## 🔐 Privacy & Data

### Does OctateCode Track My Code?

**No tracking.**
- No telemetry collection by default
- Your code never sent to OctateCode servers
- Code only sent to your chosen AI provider (if you ask)
- You see exactly what's being sent

### How Do I Keep My Code Private?

**Your data, your control:**
1. Use **local models** (Ollama) - code never leaves your computer
2. **Choose what to share** - only include files you select
3. **Never auto-index** - OctateCode doesn't scan your workspace
4. **Review before sending** - see exactly what goes to AI

### Is My API Key Safe?

**Yes, with caveats:**

✅ **OctateCode does:**
- Store keys encrypted locally
- Never logs keys
- Never sends keys to OctateCode servers
- Only sends to your chosen provider

⚠️ **You should:**
- Use provider-specific API keys (not master account)
- Keep your computer secure
- Rotate keys periodically
- Monitor API usage for suspicious activity

See [Security Guide](./SECURITY.md) for full details.

### What Happens to My Code in Collaboration?

**Peer-to-peer (default):**
- Code syncs between your computer and collaborators' computers
- No central server stores your code
- Peer must be online to see changes
- Delete local copy = delete from peer's view

**Private room with specific users:**
- Share Room ID only with trusted people
- Anyone with Room ID can join (can add password)
- All users can see all shared code

---

## 🚀 Getting Started

### How Long Does Setup Take?

**Quick setup: 5-10 minutes**
- Download app: 2 minutes
- Install: 1 minute
- Configure provider: 2-5 minutes
- Start using: immediately

See [Quick Start](./QUICK_START.md).

### What Do I Need to Start?

**Minimum:**
- Computer (Windows, macOS, Linux)
- A code project or create new folder
- That's it!

**Optional (for AI):**
- API key from provider (OpenAI, Anthropic, etc.)
- Or install Ollama for local models
- Internet connection (for cloud models)

### Which Provider Should I Use?

**For beginners:** Start with **Ollama** (free, no setup)
```bash
ollama pull llama2
ollama serve
# Then select Ollama in OctateCode Settings
```

**For best quality:** **GPT-4o** (costs ~$0.05-0.15 per chat)
- Get key: https://platform.openai.com/api-keys
- Excellent code generation
- Fast and reliable

**For budget-conscious:** **DeepSeek V3** (costs ~$0.001 per chat)
- Get key: https://platform.deepseek.com
- Surprisingly good quality
- Very cheap

**For enterprise:** **Claude Opus** (costs ~$0.02-0.06 per chat)
- Get key: https://console.anthropic.com
- Best for complex reasoning
- Highest quality

---

## 🎯 Features

### Can I Use Multiple AI Providers?

**Yes!** Each feature can use a different provider:
- **Chat**: Use GPT-4o for quality
- **Quick Edit**: Use DeepSeek for speed
- **Search**: Use Ollama for privacy

Switch in Void: Settings.

### Does Collaboration Work Offline?

**Partially:**
- P2P file sync works offline (same local network)
- Chat requires internet
- Terminal sharing works offline
- AI suggestions require internet (for cloud AI)

### Can I Use OctateCode Without Internet?

**Yes, with limitations:**
- File editing: ✅ Works fully
- Local collaboration: ✅ Works on same network
- Themes/extensions: ✅ Already downloaded
- Cloud AI: ❌ Requires internet
- Local AI (Ollama): ✅ Works entirely offline

### How Many People Can Collaborate?

**Theoretically unlimited**, practically depends on:
- Network bandwidth
- CPU for syncing
- Stability of connections

**Recommended: 2-10 concurrent users per room**
- More users = more network traffic
- Large user counts may need optimization
- Use multiple rooms if needed

### Can I Use OctateCode for Teams?

**Absolutely!**

**For small teams (up to 10):**
- P2P collaboration rooms
- Works great for pair programming
- No central server needed

**For larger organizations:**
- Deploy your own MCP servers
- Configure team API keys
- Use organization GitHub accounts
- Host on your own infrastructure

---

## 🔧 Technical

### What Operating Systems Are Supported?

- **Windows** 10/11 (x86-64, ARM64)
- **macOS** 10.15+ (Intel, Apple Silicon)
- **Linux** Ubuntu, Fedora, Debian, and more

### Does OctateCode Work in Web Browser?

**Not currently.** OctateCode is a desktop app using Electron.

However:
- VS Code (base editor) has web version
- OctateCode adds features to VS Code
- Web version would require rewrite

### Can I Use OctateCode with Extensions?

**Yes!**
- VS Code extensions generally work
- Popular extensions compatible
- Some extensions may not be optimized
- Create custom extensions if needed

### What Languages Are Supported?

All languages VS Code supports:
- JavaScript, TypeScript
- Python, Go, Rust
- Java, C++, C#
- PHP, Ruby, Swift
- HTML, CSS, JSON, YAML
- And 50+ more!

---

## 🆘 Troubleshooting

### Chat Isn't Working. What Do I Do?

**First steps:**
1. Check internet connection
2. Verify API key in settings
3. Try different provider/model
4. Restart OctateCode

See [Troubleshooting Guide](./TROUBLESHOOTING.md#chat--ai-issues).

### Can't Connect to Collaboration Room

**Check:**
1. Room ID is exactly correct
2. Other user is online
3. Firewall isn't blocking connection
4. Both using same version

See [Troubleshooting - Collaboration](./TROUBLESHOOTING.md#-collaboration-issues).

### Performance is Slow. Any Tips?

**Try:**
1. Close unused tabs
2. Disable unused extensions
3. Reduce open files
4. Switch to faster model
5. Use local models instead

See [Performance Guide](./PERFORMANCE.md).

### Where Are My Settings Saved?

**Locations:**
- **Windows**: `%APPDATA%\Code\User\settings.json`
- **macOS**: `~/Library/Application Support/Code/User/settings.json`
- **Linux**: `~/.config/Code/User/settings.json`

API keys stored in secure storage (encrypted).

---

## 🎓 Learning & Help

### How Do I Learn to Use OctateCode?

1. **[Quick Start](./QUICK_START.md)** - 5 minute intro
2. **[User Guide](./USER_GUIDE.md)** - Feature walkthrough
3. **[Chat Guide](./CHAT.md)** - AI chat details
4. **[Collaboration Guide](./COLLABORATION_QUICKSTART.md)** - Team features

### Where Can I Get Help?

1. **This FAQ** - Check here first
2. **[Troubleshooting Guide](./TROUBLESHOOTING.md)** - Common issues
3. **[GitHub Issues](https://github.com/preetbiswas12/octatecode/issues)** - Search existing issues
4. **[GitHub Discussions](https://github.com/preetbiswas12/octatecode/discussions)** - Ask community

### How Do I Report a Bug?

1. Search existing issues first
2. Create new GitHub issue with:
   - OS and OctateCode version
   - Steps to reproduce
   - Expected vs actual behavior
   - Error message
   - Screenshots if applicable

### Can I Contribute?

**Yes, please!**
- Report bugs
- Help with documentation
- Write code features
- Create extensions
- Share ideas

See [Contributing Guide](./CONTRIBUTING.md).

---

## 🌍 Community

### How Active Is the Community?

**Growing community!**
- Active GitHub discussions
- Feature requests welcomed
- Contributors recognized
- Regular updates and improvements

### Can I Fork OctateCode?

**Absolutely!**
- MIT license allows it
- Source on GitHub: preetbiswas12/octatecode
- Build your own version
- Contribute improvements back

### Where Can I Discuss Ideas?

- **GitHub Discussions** - Main community space
- **GitHub Issues** - For bugs and features
- **Discussion Threads** - For questions

---

## 📊 Development

### Is OctateCode Open Source?

**Yes!**
- License: MIT (very permissive)
- Source: https://github.com/preetbiswas12/octatecode
- Contributions welcome
- Built on VS Code (also open source)

### How Can I Contribute Code?

1. Fork repository
2. Create feature branch
3. Make changes
4. Write tests
5. Submit pull request

See [Contributing Guide](./CONTRIBUTING.md) for detailed steps.

### Can I Use OctateCode As a Base for My Project?

**Yes!** MIT license allows:
- Commercial use
- Modifications
- Distribution
- Private use

Just include original license.

### Where Do I Find the Source Code?

**GitHub:** https://github.com/preetbiswas12/octatecode
- `src/vs/workbench/contrib/void/` - OctateCode specific code
- `src/vs/` - VS Code base code
- `build/`, `test/` - Build and test files

See [Architecture Guide](./ARCHITECTURE.md) for structure details.

---

## 🎁 Advanced Features

### What's Model Context Protocol (MCP)?

**MCP enables custom tools:**
- Create tools in Python/JavaScript
- OctateCode calls your tools
- Tools execute locally
- Results returned to AI

Useful for:
- Custom code analyzers
- Database queries
- API integrations
- System commands

### Can I Create Custom AI Tools?

**Yes!** Use Model Context Protocol:
1. Implement tool in Python/JavaScript
2. Register with MCP server
3. OctateCode detects and lists
4. AI can call your tool

Details in [Architecture - MCP](./ARCHITECTURE.md#model-context-protocol).

### Can I Use Custom LLM Endpoints?

**Yes!**
- vLLM - run custom models locally
- Ollama - run open models
- LiteLLM - proxy multiple providers
- Self-hosted endpoints

Configure in settings.

---

## 📝 Licensing & Legal

### What License Does OctateCode Use?

**MIT License**
- Very permissive open source license
- Commercial and personal use allowed
- Can modify and redistribute
- Must include license file

See LICENSE.txt in repository.

### Can I Use OctateCode Commercially?

**Yes!** MIT license permits commercial use.

Just:
- Include MIT license text
- Attribute original authors
- No liability for issues

### What About VS Code License?

**VS Code is also MIT licensed**
- OctateCode builds on VS Code
- Both MIT allows combination
- Same permissive terms apply

---

## 🚧 Future

### What's Coming in OctateCode?

**Planned features:**
- Team collaboration improvements
- More AI providers
- Performance optimizations
- Better MCP tools
- Web version (long term)

Check GitHub issues for current roadmap.

### Can I Request a Feature?

**Absolutely!**
1. Check existing requests
2. Create GitHub issue with "Feature Request" label
3. Describe use case and benefit
4. Vote on existing requests

---

Still have questions?

Check [Documentation Index](./INDEX.md) for more guides, or [create a GitHub discussion](https://github.com/preetbiswas12/octatecode/discussions) to ask the community!

**Happy coding! 🚀**
