# API Reference

Complete reference for OctateCode services, types, and interfaces.

---

## 📚 Overview

OctateCode exposes services through dependency injection. This guide covers the main public APIs and how to use them.

**Key Principle:** Define interfaces in `common/`, implement in `electron-main/` or `browser/`.

---

## 🎯 Service Registration

### Pattern

Every service follows this pattern:

```typescript
// common/myServiceTypes.ts
export const IMyService = createDecorator<IMyService>('myService');
export interface IMyService {
  methodName(param: string): Promise<string>;
}

// electron-main/myService.ts (or browser/myService.ts)
class MyService implements IMyService {
  async methodName(param: string): Promise<string> {
    return `Result: ${param}`;
  }
}

registerSingleton(IMyService, MyService, InstantiationType.Eager);

// Usage (inject into constructor)
class MyClass {
  constructor(@IMyService private myService: IMyService) {}

  async doSomething() {
    const result = await this.myService.methodName('test');
  }
}
```

---

## 🔧 Core Services

### IVoidSettingsService

**Location:** `src/vs/workbench/contrib/void/common/voidSettingsTypes.ts`

**Purpose:** Manage user settings for OctateCode.

```typescript
interface IVoidSettingsService {
  // Get settings
  getSettings(): Promise<VoidSettings>;

  // Update settings
  updateSettings(settings: Partial<VoidSettings>): Promise<void>;

  // Get provider configuration
  getProviderConfig(provider: ProviderName): Promise<ProviderConfig>;

  // Update provider API key
  setProviderApiKey(provider: ProviderName, key: string): Promise<void>;

  // Listen for changes
  onSettingsChanged: IEvent<VoidSettings>;
}
```

**Types:**
```typescript
type ProviderName =
  | 'anthropic' | 'openAI' | 'deepseek' | 'ollama'
  | 'vLLM' | 'lmStudio' | // ... and more

interface VoidSettings {
  selectedProvider: ProviderName;
  selectedModel: string;
  chatModel?: string;
  editModel?: string;
  temperature: number;
  maxTokens: number;
  systemPrompt?: string;
}

interface ProviderConfig {
  provider: ProviderName;
  apiKey?: string;       // Optional, only if needed
  endpoint?: string;     // Custom endpoint
  enabled: boolean;
}
```

**Example:**
```typescript
// Get selected model
const settings = await settingsService.getSettings();
console.log(settings.selectedModel); // e.g., "gpt-4o"

// Update provider
await settingsService.setProviderApiKey('openAI', 'sk-...');
```

### IChatThreadService

**Location:** `src/vs/workbench/contrib/void/common/chatThreadServiceTypes.ts`

**Purpose:** Manage chat conversations and message history.

```typescript
interface IChatThreadService {
  // Create new thread
  createThread(): Promise<ChatThread>;

  // Get thread by ID
  getThread(threadId: string): Promise<ChatThread>;

  // Load message history
  getMessages(threadId: string): Promise<ChatMessage[]>;

  // Add message to thread
  addMessage(threadId: string, message: ChatMessage): Promise<void>;

  // Add tool result
  addToolResult(threadId: string, toolResult: ToolResult): Promise<void>;

  // Delete thread
  deleteThread(threadId: string): Promise<void>;

  // Listen for updates
  onThreadUpdated: IEvent<{ threadId: string; message: ChatMessage }>;
}
```

**Types:**
```typescript
interface ChatThread {
  id: string;
  title: string;
  createdAt: Date;
  lastModified: Date;
  model?: string;
  provider?: string;
}

interface ChatMessage {
  id: string;
  threadId: string;
  role: 'user' | 'assistant' | 'system' | 'tool';
  content: string;
  timestamp: Date;
  fileContext?: FileContext[];
  toolCalls?: ToolCall[];
}

interface FileContext {
  filePath: string;
  content: string;
  startLine?: number;
  endLine?: number;
}

interface ToolCall {
  toolName: string;
  parameters: Record<string, any>;
}

interface ToolResult {
  toolName: string;
  result: string;
  error?: string;
}
```

**Example:**
```typescript
// Create new chat
const thread = await chatService.createThread();

// Add user message
const userMsg: ChatMessage = {
  id: generateId(),
  threadId: thread.id,
  role: 'user',
  content: 'Add error handling',
  timestamp: new Date(),
  fileContext: [{
    filePath: 'src/app.ts',
    content: 'function process() { ... }'
  }]
};

await chatService.addMessage(thread.id, userMsg);
```

### IEditCodeService

**Location:** `src/vs/workbench/contrib/void/common/editCodeServiceTypes.ts`

**Purpose:** Handle code editing and application of AI suggestions.

```typescript
interface IEditCodeService {
  // Apply code changes to file
  applyChanges(params: ApplyParams): Promise<void>;

  // Get computed diffs
  getComputedDiff(params: DiffParams): Promise<ComputedDiff>;

  // Reject pending changes
  rejectChanges(fileUri: URI): Promise<void>;

  // Listen for diff updates
  onDiffUpdated: IEvent<{ fileUri: URI; diff: ComputedDiff }>;
}
```

**Types:**
```typescript
interface ApplyParams {
  fileUri: URI;
  changes: CodeChange[];
  fastMode?: boolean; // Default true
}

interface CodeChange {
  type: 'insert' | 'replace' | 'delete';
  startLine: number;
  endLine?: number;
  newText?: string;
}

interface ComputedDiff {
  fileUri: URI;
  diffs: Diff[];
  preview: string;
  canApply: boolean;
}

interface Diff {
  startLine: number;
  endLine: number;
  type: 'delete' | 'insert' | 'replace';
  oldText?: string;
  newText?: string;
}
```

**Example:**
```typescript
// Apply code changes
await editService.applyChanges({
  fileUri: URI.file('/path/to/file.ts'),
  changes: [{
    type: 'replace',
    startLine: 5,
    endLine: 8,
    newText: 'function newVersion() { ... }'
  }]
});
```

### ICollaborationService

**Location:** `src/vs/workbench/contrib/void/common/collaborationServiceTypes.ts`

**Purpose:** Manage real-time collaboration rooms and peer connections.

```typescript
interface ICollaborationService {
  // Create collaboration room
  createRoom(): Promise<CollaborationRoom>;

  // Join existing room
  joinRoom(roomId: string, userId?: string): Promise<void>;

  // Leave current room
  leaveRoom(): Promise<void>;

  // Get connected peers
  getPeers(): Peer[];

  // Send message to peers
  sendMessage(message: CollaborationMessage): Promise<void>;

  // Listen for peer events
  onPeerJoined: IEvent<Peer>;
  onPeerLeft: IEvent<{ peerId: string }>;
  onMessageReceived: IEvent<CollaborationMessage>;
  onFileChanged: IEvent<{ fileUri: URI; content: string }>;
}
```

**Types:**
```typescript
interface CollaborationRoom {
  id: string;
  createdAt: Date;
  owner: string;
  peers: Peer[];
  isPublic: boolean;
}

interface Peer {
  id: string;
  name: string;
  cursorPosition?: Position;
  selection?: Range;
  fileUri?: URI;
  status: 'connected' | 'disconnected';
}

interface Position {
  line: number;
  column: number;
}

interface Range {
  start: Position;
  end: Position;
}

interface CollaborationMessage {
  type: 'text' | 'cursor' | 'selection' | 'file-sync' | 'terminal';
  from: string;
  to?: string; // omit for broadcast
  data: Record<string, any>;
  timestamp: Date;
}
```

**Example:**
```typescript
// Create collaboration room
const room = await collab.createRoom();
console.log(`Room ID: ${room.id}`);

// Send cursor update
await collab.sendMessage({
  type: 'cursor',
  from: userId,
  data: {
    fileUri: 'file:///path/to/file.ts',
    line: 10,
    column: 5
  },
  timestamp: new Date()
});
```

### IMCPService

**Location:** `src/vs/workbench/contrib/void/common/mcpServiceTypes.ts`

**Purpose:** Model Context Protocol - execute tools and manage integrations.

```typescript
interface IMCPService {
  // List available tools
  listTools(): Promise<MCPTool[]>;

  // Execute tool
  executeTool(toolName: string, params: Record<string, any>): Promise<any>;

  // Register custom tool
  registerTool(tool: MCPTool): Promise<void>;

  // Listen for tool availability
  onToolsUpdated: IEvent<MCPTool[]>;
}
```

**Types:**
```typescript
interface MCPTool {
  name: string;
  description: string;
  parameters: ToolParameter[];
  resultType: 'string' | 'object' | 'array' | 'binary';
}

interface ToolParameter {
  name: string;
  type: 'string' | 'number' | 'boolean' | 'object' | 'array';
  description?: string;
  required: boolean;
  default?: any;
}
```

**Example:**
```typescript
// List tools
const tools = await mcpService.listTools();
console.log(tools.map(t => t.name));

// Execute tool
const result = await mcpService.executeTool('searchCode', {
  query: 'function handleClick',
  filePattern: '*.ts'
});
```

---

## 📡 IPC Channels

For communication between main process and browser process:

### sendLLMMessage

**From:** Browser
**To:** Main process
**Purpose:** Send chat message and receive AI response

```typescript
// Browser side
const response = await mainProcessService.call('sendLLMMessage', {
  threadId: string;
  messages: ChatMessage[];
  model: string;
  provider: string;
  tools?: MCPTool[];
});

// Main process receives and sends to provider
// Returns: streaming response SSE or complete response
```

### updateFileContent

**From:** Browser
**To:** Main process
**Purpose:** Notify file content changed

```typescript
channel.listen('updateFileContent', (data) => {
  const { fileUri, content, version } = data;
  // Update file, sync to peers, etc.
});
```

### sendCollaborationMessage

**From:** Browser
**To:** Main process
**Purpose:** Send P2P collaboration message

```typescript
channel.listen('sendCollaborationMessage', (message) => {
  // Route message to peer
  // Update local state
});
```

---

## 🧩 Common Types

### URI

Represents a file or resource path.

```typescript
import { URI } from '...'base/common/uri.js';

// Create from file path
const uri = URI.file('/path/to/file.ts');

// Create from string
const uri = URI.parse('file:///path/to/file.ts');

// Get file path
const path = uri.fsPath;

// Combine paths
const fileUri = uri.with({ path: uri.path + '/subfolder' });
```

### IEvent

Event emitter for pub/sub patterns.

```typescript
interface IEvent<T> {
  (listener: (e: T) => void): IDisposable;
}

// Example usage:
service.onSettingsChanged((settings) => {
  console.log('Settings changed', settings);
});
```

### Disposable

Resource cleanup pattern.

```typescript
class MyClass implements IDisposable {
  dispose(): void {
    // Clean up resources
    this._listener?.dispose();
  }
}

// Usage: auto cleanup when done
using resource = createDisposable(() => {
  // cleanup code
});
```

---

## 🔄 Data Flow

### Chat Message Flow

```
1. User types in chat UI (Browser)
   ↓
2. Browser calls sendLLMMessage IPC (Browser → Main)
   ↓
3. Main process gets API key from settings (Main)
   ↓
4. Main process calls LLM provider API (Main → Provider)
   ↓
5. Provider returns streamed response (Provider → Main)
   ↓
6. Main process streams response to browser (Main → Browser)
   ↓
7. Browser renders response in chat UI (Browser)
```

### File Sync in Collaboration

```
1. User edits file locally (Browser)
   ↓
2. Browser detects change (Browser)
   ↓
3. Browser sends updateFileContent IPC (Browser → Main)
   ↓
4. Main process receives (Main)
   ↓
5. Main process finds connected peers (Main)
   ↓
6. Main process sends to each peer (Main → Peer)
   ↓
7. Peer receives and updates local file (Peer)
```

---

## ⚙️ Configuration

### Provider Configuration

Each provider has default settings in `modelCapabilities.ts`:

```typescript
export function getModelCapabilities(
  provider: ProviderName,
  model: string
): ModelCapabilities {
  return {
    contextWindow: 128000,           // Max tokens
    reservedOutputTokens: 4000,      // Leave space for output
    supportsSystemMessage: true,
    supportsFunctionCalling: true,
    supportsVision: false,
    reasoningEnabled: false,
  };
}
```

### Custom Endpoints

Override default endpoints:

```typescript
// Browser settings
settings.customEndpoints = {
  vLLM: 'https://my-vllm.example.com',
  ollama: 'http://localhost:11435' // Non-standard port
};
```

---

## 🧪 Testing

### Mock Services

```typescript
class MockChatService implements IChatThreadService {
  async createThread(): Promise<ChatThread> {
    return { id: 'mock-1', title: 'Mock', /* ... */ };
  }
  // ... implement other methods
}

// Use in tests:
const mockService = new MockChatService();
const instance = {
  [IServiceA.toString()]: mockService
};
```

### Integration Testing

```typescript
// In browser tests:
import('../../browser/chatThreadService').then(({ ChatThreadService }) => {
  const service = new ChatThreadService(storageService, logger);
  // Test interactions
});
```

---

## 📖 Using This Reference

1. **Looking up a service?** Check Core Services section
2. **Need to call IPC?** See IPC Channels section
3. **Unsure about a type?** Check Common Types
4. **Integrating with main process?** See Data Flow

---

## 🔗 Related Documentation

- [Architecture Guide](./ARCHITECTURE.md) - System design
- [Developer Setup](./DEVELOPER_SETUP.md) - Getting started
- [Contributing](./CONTRIBUTING.md) - Making changes

---

**Last updated:** February 2026
**Version:** 1.99.3
