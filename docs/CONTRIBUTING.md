# Contributing to OctateCode

Thank you for your interest in contributing to OctateCode! This guide will help you get started.

---

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Making Changes](#making-changes)
- [Testing](#testing)
- [Submitting Changes](#submitting-changes)
- [Coding Standards](#coding-standards)
- [Commit Messages](#commit-messages)

---

## 🤝 Code of Conduct

By participating in OctateCode, you agree to:
- Be respectful and inclusive
- Give constructive feedback
- Respect differing opinions
- Report misconduct appropriately
- Focus on what's best for the community

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** 18+
- **npm** 9+
- **git**
- Familiarity with TypeScript and React
- Understanding of Electron architecture

### Fork & Clone
```bash
# Fork on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/octatecode.git
cd octatecode

# Add upstream remote
git remote add upstream https://github.com/preetbiswas12/octatecode.git
```

### Set Up Development Environment
```bash
# Install dependencies
npm install

# Download Electron binary
npm run electron

# Verify setup
npm run test-node
```

See [Developer Setup](./DEVELOPER_SETUP.md) for detailed instructions.

---

## 🛠️ Development Workflow

### Start Development Watchers

Open three terminals and run:

**Terminal 1 - Core TypeScript:**
```bash
npm run watch-clientd
```

**Terminal 2 - React Components:**
```bash
npm run watchreactd
```

**Terminal 3 - Extensions:**
```bash
npm run watch-extensionsd
```

### Launch Dev Window
```bash
./scripts/code.sh   # Linux/macOS
.\scripts\code.bat  # Windows
```

### Create Feature Branch
```bash
# Keep main clean
git checkout main
git pull upstream main

# Create feature branch
git checkout -b feature/your-feature-name
```

### Reload Changes
In the dev window:
- **Reload**: `Ctrl+R` (Cmd+R on macOS)
- **Toggle DevTools**: `Ctrl+Shift+I` (Cmd+Option+I on macOS)
- **View Logs**: Help → Toggle Developer Tools

---

## 📝 Making Changes

### Project Structure

```
src/vs/workbench/contrib/void/
├── browser/                    # UI and React components
│   ├── react/                 # React source code
│   ├── *Service.ts            # Browser-side services
│   └── ...
├── common/                    # Shared types and interfaces
│   ├── voidSettingsTypes.ts   # Settings schema
│   ├── modelCapabilities.ts   # Model definitions
│   ├── *ServiceTypes.ts       # Type definitions
│   └── ...
├── electron-main/             # Main process (Node.js)
│   ├── llmMessage/           # LLM provider callers
│   ├── mcp/                  # MCP server
│   ├── *Service.ts           # Main-side services
│   └── ...
└── test/                      # Unit tests
```

### Key Concepts

**Process Model:**
- **Main Process** (Node.js) - File I/O, APIs, background tasks
- **Browser Process** (Renderer) - UI, React, DOM
- **Communication** - IPC channels between processes

**Shared Types:**
- Define interfaces in `common/`
- Implement on both sides if needed
- Keep `common/` lightweight

**Services:**
Every feature is a service (singleton):
```typescript
// common/myServiceTypes.ts
export const IMyService = createDecorator<IMyService>('myService');
export interface IMyService { ... }

// electron-main/myService.ts
class MyService implements IMyService { ... }
registerSingleton(IMyService, MyService, InstantiationType.Eager);

// Usage anywhere with DI:
constructor(@IMyService private myService: IMyService) {}
```

### Common Changes

**Adding a new AI provider:**
1. Add to `voidSettingsTypes.ts` - provider name and models
2. Add capabilities to `modelCapabilities.ts`
3. Create caller in `electron-main/llmMessage/llmProviderCallers.ts`
4. Update tests

**Adding a UI feature:**
1. Create React component in `browser/react/src/`
2. Register command in `browser/sidebarActions.ts`
3. Add keybinding in manifest
4. Connect to service via IPC if needed

**Adding a service:**
1. Define interface in `common/*ServiceTypes.ts`
2. Implement in appropriate process (`electron-main/` or `browser/`)
3. Register as singleton
4. Inject where needed
5. Add tests in `test/`

---

## ✅ Testing

### Run Tests
```bash
# Node.js tests (unit tests, no browser)
npm run test-node

# Browser tests (Playwright, full integration)
npm run test-browser

# All tests
npm run test-node && npm run test-browser
```

### Write Tests

**Node test structure:**
```typescript
// test/unit/node/myService.test.ts
import { describe, it, assert } from 'node:test';
import { MyService } from '../../../src/.../myService';

describe('MyService', () => {
  it('should do something', () => {
    const service = new MyService();
    assert.ok(service.doSomething());
  });
});
```

**Test guidelines:**
- One test file per service/module
- Clear test names describing what's being tested
- Test both happy path and error cases
- Mock external dependencies
- Keep tests isolated and fast

### Running Specific Tests
```bash
# Run specific test file
npx mocha test/unit/node/myService.test.ts

# Run tests matching pattern
npx mocha test/unit/node/**/*.test.ts --grep "MyService"
```

---

## 📤 Submitting Changes

### What to Include

**Good PRs have:**
- Clear, descriptive title
- Detailed description of changes
- Link to related issues (#123)
- Tests for new functionality
- No breaking changes (or justified)
- Updated documentation if needed

**Example PR description:**
```
Add support for DeepSeek provider

This PR adds DeepSeek as a new LLM provider option, supporting
both DeepSeek-V3 and DeepSeek Chat models. Users can now select
DeepSeek from the provider dropdown in settings.

Fixes #456

### Changes
- Added 'deepseek' to provider list in voidSettingsTypes.ts
- Added DeepSeek models and capabilities in modelCapabilities.ts
- Created DeepSeekCaller in llmProviderCallers.ts
- Added tests for DeepSeek provider
- Updated docs/CHAT.md with DeepSeek setup

### Testing
- [ ] Manual testing with API key
- [ ] Model list populates correctly
- [ ] Responses stream as expected
- [ ] Error handling works
```

### Pull Request Process

1. **Push to your fork:**
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create PR on GitHub:**
   - Go to your fork
   - Click "New Pull Request"
   - Select base: `upstream main`, compare: `your-branch`
   - Fill in title and description
   - Click "Create Pull Request"

3. **Wait for CI checks:**
   - Tests must pass
   - Linting must pass
   - Code review may be requested

4. **Address feedback:**
   - Make requested changes
   - Push to same branch (PR auto-updates)
   - Reply to comments

5. **Merge:**
   - Once approved and tests pass, maintainer will merge
   - Your changes are now part of OctateCode! 🎉

### Updating from Upstream
```bash
# Fetch latest changes
git fetch upstream

# Rebase your branch (keep history clean)
git rebase upstream/main

# Force push your updated branch
git push --force-with-lease origin feature/your-feature-name
```

---

## 💻 Coding Standards

### TypeScript Guidelines

**Strict mode enabled:**
```typescript
// Good - explicit types
const getName = (user: User): string => {
  return user.name;
};

// Bad - types inferrable but still good
const getName = (user: any): any => {
  return user.name;
};
```

**Files end with .ts or .tsx:**
- `.ts` for non-React code
- `.tsx` for React components
- Always include `.js` extension in imports (ES modules)

**Imports pattern:**
```typescript
// ✅ Good - relative with .js
import { MyClass } from '../myClass.js';
import { IMyService } from './myServiceTypes.js';

// ❌ Bad - no extension or bare imports
import { MyClass } from '../myClass';
import MyClass from 'myClass';
```

### React Best Practices

**Functional components:**
```typescript
// Good
const MyComponent: React.FC<{ name: string }> = ({ name }) => {
  return <div>{name}</div>;
};

// Avoid class components unless necessary
class MyComponent extends React.Component { ... }
```

**Hooks usage:**
```typescript
// Use hooks for state and side effects
const [value, setValue] = useState('');
const [data, setData] = useEffect(async () => {
  const result = await fetchData();
  setData(result);
}, []);
```

**Props interface:**
```typescript
interface MyComponentProps {
  name: string;
  onAction?: (value: string) => void;
  children?: React.ReactNode;
}

const MyComponent: React.FC<MyComponentProps> = ({ name, onAction, children }) => {
  // ...
};
```

### Naming Conventions

- **Files**: `camelCase.ts` or `PascalCase.tsx` for components
- **Functions**: `camelCase()`
- **Classes**: `PascalCase`
- **Interfaces**: `IPascalCase` (leading 'I' for service interfaces)
- **Constants**: `SCREAMING_SNAKE_CASE` for module-level
- **Variables**: `camelCase`

### Error Handling

**Always handle async errors:**
```typescript
// Good
try {
  const response = await callAPI();
  processResponse(response);
} catch (error) {
  logger.error('Failed to call API', error);
  this.notifyError('API call failed');
}

// Bad - silently swallows errors
const response = await callAPI().catch(() => {});
```

**Meaningful error messages:**
```typescript
// Good
throw new Error('Failed to apply code changes: could not find target function');

// Bad
throw new Error('Error');
```

### Code Organization

**Keep files under 500 lines:**
- Split larger files into multiple modules
- Group related functions together
- One main export per file

**Comments for why, not what:**
```typescript
// Good - explains reasoning
// We retry 3 times because OpenAI API is sometimes rate-limited
const MAX_RETRIES = 3;

// Bad - obvious from code
// Set max retries to 3
const MAX_RETRIES = 3;
```

---

## 📝 Commit Messages

### Format

```
<type>: <subject> (<scope>)

<body>

<footer>
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style (formatting, etc.)
- `refactor`: Refactoring existing code
- `perf`: Performance improvement
- `test`: Test additions/changes
- `chore`: Build, dependencies, etc.

### Examples

```
feat: Add DeepSeek provider support

Add DeepSeek-V3 and DeepSeek Chat models as new AI provider options.
Users can now configure DeepSeek API key in settings.

Implements model selection dropdown for DeepSeek models including
automatic capabilities detection.

Fixes #456
```

```
fix: Prevent chat window freeze on large file context

Limit maximum context file size to 100KB when auto-detecting context.
Prevents UI freeze when user includes very large files.

Adds warning dialog if file exceeds 100KB.
```

```
docs: Update CHAT.md with provider setup instructions

Add step-by-step setup guides for all supported providers.
Include cost estimates and model selection recommendations.
```

### Best Practices

- **Descriptive subjects** (50 chars max)
- **Explain why**, not what
- **Reference issues** with "Fixes #123" or "Refs #456"
- **One logical change** per commit
- **Test before committing**

---

## 🐛 Bug Reporting

Found a bug? Report it on GitHub:

1. **Check existing issues** - might already be reported
2. **Create new issue** with title like "Chat freezes when..."
3. **Include details:**
   - OS and OctateCode version
   - Steps to reproduce (clear sequence)
   - Expected vs actual behavior
   - Error messages (exact text)
   - Screenshots or video

---

## 🧠 Architecture Decisions

When making architectural changes:

1. **Check existing patterns** - consistency matters
2. **Discuss first** - create GitHub discussion or issue
3. **Document decisions** - why this approach over alternatives
4. **Consider maintainability** - will someone else understand this?
5. **Get feedback** - senior contributors review

---

## 🎯 Priority Issues for New Contributors

Looking for something to work on?

1. **Good First Issue** label - scoped, well-documented
2. **Help Wanted** label - need community contribution
3. **Documentation** - always needs improvement
4. **Tests** - add tests for untested code
5. **Performance** - optimize slow operations

---

## 📞 Need Help?

- **Questions?** Create GitHub Discussion
- **Stuck?** Comment on issue or PR
- **Unsure about approach?** Ask in discussion first
- **Want feedback before PR?** Share draft PR early

---

## ✨ Recognition

Contributors are recognized in:
- GitHub contributor graphs
- Release notes for major features
- Project README (upon request)
- Community spotlight

Thank you for contributing to OctateCode! 🙏

---

**Ready to contribute? Start with [Developer Setup](./DEVELOPER_SETUP.md)!**
