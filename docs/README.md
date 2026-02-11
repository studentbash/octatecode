# OctateCode Documentation

Welcome to the OctateCode documentation! OctateCode is an open-source AI-powered code editor built on VS Code, featuring real-time collaboration, multi-provider LLM support, and advanced code editing capabilities.

## 🚀 Start Here

**New to OctateCode?** Start with the [Quick Start Guide](./QUICK_START.md) to get up and running in 5 minutes.

**Looking for specific information?** See the [Documentation Index](./INDEX.md) for a complete guide to all documentation.

---

## 📑 Key Documentation

### For Users
- **[Quick Start](./QUICK_START.md)** - Get started in 5 minutes
- **[User Guide](./USER_GUIDE.md)** - Complete feature walkthrough
- **[Features](./FEATURES.md)** - Detailed feature showcase
- **[FAQ](./FAQ.md)** - Frequently asked questions
- **[Troubleshooting](./TROUBLESHOOTING.md)** - Common issues and solutions

### For Developers
- **[Developer Setup](./DEVELOPER_SETUP.md)** - Development environment configuration
- **[Architecture](./ARCHITECTURE.md)** - System design and components
- **[API Reference](./API_REFERENCE.md)** - Service interfaces and types
- **[Contributing](./CONTRIBUTING.md)** - Contribution guidelines

### For Specific Features
- **[Chat Features](./CHAT.md)** - AI chat and provider setup
- **[Collaboration](./COLLABORATION_QUICKSTART.md)** - Real-time collaboration
- **[Multi-Cursor](./MULTI_CURSOR.md)** - Advanced cursor editing
- **[Cursor Management](./CURSOR.md)** - Cursor behavior details

### Security & Performance
- **[Security Guide](./SECURITY.md)** - Privacy and security implementation
- **[Performance Guide](./PERFORMANCE.md)** - Optimization techniques

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| **AI Chat** | Multi-provider LLM support (15+ providers including OpenAI, Anthropic, DeepSeek, Ollama) |
| **Real-Time Collaboration** | P2P file sync, live cursors, shared terminals |
| **Smart Code Application** | Fast/slow diff modes for applying AI suggestions |
| **Model Context Protocol** | Custom tool integration and execution |
| **Multi-Cursor Editing** | Efficient simultaneous editing at multiple locations |
| **Provider Flexibility** | Choose different AI provider per feature |

---

## 📋 Complete Documentation Index

See [INDEX.md](./INDEX.md) for the complete documentation index with all files and navigation guides.

---

## 🛠️ Tech Stack

- **Electron** - Cross-platform desktop application
- **React** - Modern UI components
- **TypeScript** - Type-safe development
- **VS Code** - Editor foundation
- **Model Context Protocol** - Tool integration framework

---

## 📖 Documentation Highlights

### If You Want To...

**...use OctateCode as an editor:**
1. [Quick Start Guide](./QUICK_START.md)
2. [User Guide](./USER_GUIDE.md)
3. [Chat Features](./CHAT.md)

**...set up for development:**
1. [Developer Setup](./DEVELOPER_SETUP.md)
2. [Architecture Guide](./ARCHITECTURE.md)
3. [Contributing](./CONTRIBUTING.md)

**...troubleshoot issues:**
1. [Troubleshooting Guide](./TROUBLESHOOTING.md)
2. [FAQ](./FAQ.md)
3. [GitHub Issues](https://github.com/preetbiswas12/octatecode/issues)

**...learn about security:**
1. [Security Guide](./SECURITY.md)
2. [FAQ - Privacy Section](./FAQ.md#-privacy--data)

**...optimize performance:**
1. [Performance Guide](./PERFORMANCE.md)
2. [Architecture - Performance](./ARCHITECTURE.md#performance)

---

## 🎯 Project Structure

```
octatecode/
├── src/vs/workbench/contrib/void/
│   ├── browser/                 # Browser process UI and React
│   ├── common/                  # Shared types and interfaces
│   ├── electron-main/           # Main process and services
│   └── test/                    # Unit tests
├── docs/                        # Documentation (you are here)
├── out/                         # Compiled output
├── package.json                 # Dependencies
└── build/                       # Build configuration
```

---

## 🤝 Contributing

Contributions are welcome! See [Contributing Guide](./CONTRIBUTING.md) for:
- Development workflow
- Coding standards
- Testing requirements
- Pull request process

---

## 📞 Getting Help

- **Questions?** Check [FAQ.md](./FAQ.md)
- **Issues?** See [Troubleshooting.md](./TROUBLESHOOTING.md)
- **Found a bug?** [Report on GitHub](https://github.com/preetbiswas12/octatecode/issues)
- **Want to discuss?** [GitHub Discussions](https://github.com/preetbiswas12/octatecode/discussions)

---

## 📜 License

OctateCode is licensed under the **MIT License** - See LICENSE.txt in the repository.

---

## 🔗 Quick Links

| Resource | Link |
|----------|------|
| Repository | [github.com/preetbiswas12/octatecode](https://github.com/preetbiswas12/octatecode) |
| Latest Release | [GitHub Releases](https://github.com/preetbiswas12/octatecode/releases) |
| Issues | [GitHub Issues](https://github.com/preetbiswas12/octatecode/issues) |
| Discussions | [GitHub Discussions](https://github.com/preetbiswas12/octatecode/discussions) |

---

**Version:** 1.99.3 | **Updated:** February 2026

