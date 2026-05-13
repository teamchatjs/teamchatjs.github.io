# Team Chat

A beautiful, self-contained team messenger that runs entirely in the browser. Create rooms, chat in channels, send direct messages, and share files — all with real-time sync across browsers and devices. No server setup, no accounts, no installs.

🌐 **Live demo:** [https://teamchatjs.github.io](https://teamchatjs.github.io)

## Features

### 💬 Messaging
- **Channels** — Create public channels for topic-based conversations
- **Direct Messages** — Private 1-on-1 chats with any team member
- **Edit messages** — Fix typos or update content after sending
- **Delete messages** — Mark messages as deleted with optional "View content" reveal for full transparency
- **Edit history badges** — See when messages were edited or deleted
- **Online indicators** — Green dots show who's currently active

### 📎 File Sharing
- **Drag & drop attachments** directly into the composer
- **Paste images** from clipboard (Ctrl/Cmd+V)
- **Multi-file support** — attach multiple files per message
- **Image previews** with full-size lightbox viewer
- **File type icons** for documents, archives, code, media, and more
- **Edit attachments** — add or remove files when editing a message
- **2 MB per file** limit (configurable in source)

### 🔄 Real-Time Sync
- **Cross-browser sync** via [jsonblob.com](https://jsonblob.com) cloud storage
- **Automatic merge** of concurrent edits from multiple devices
- **Offline-first** — messages queue locally and sync when connection returns
- **Visual sync indicator** with color-coded status (green/yellow/red)
- **Manual refresh** — click the status dot to force a sync

### 🏢 Team Management
- **Instant rooms** — just enter a team code, no registration
- **Shareable invite links** — one-click copy with embedded room ID
- **Switch teams** on the fly without losing data
- **Multi-room history** — local data preserved per team
- **Persistent sessions** — resume where you left off

### 🎨 UI/UX
- **Dark theme** inspired by modern messengers
- **Fully responsive** — mobile, tablet, desktop, landscape
- **Keyboard shortcuts** — Enter to send, Esc to cancel, Shift+Enter for newline
- **Touch-friendly** with tap-to-reveal message actions on mobile
- **No dependencies** — pure HTML/CSS/JS in a single file

## Quick Start

### Option 1: Use the Hosted Version
Visit [https://teamchatjs.github.io](https://teamchatjs.github.io) and enter your name + a team code to start chatting immediately. Share the invite link with teammates.

### Option 2: Self-Host
1. Download `index.html` from the [repository](https://github.com/teamchatjs/teamchatjs.github.io)
2. Upload it anywhere (GitHub Pages, Netlify, S3, your own server, or just open the file locally)
3. That's it — no build step, no server, no database

### Option 3: Fork on GitHub
```bash
git clone https://github.com/teamchatjs/teamchatjs.github.io.git
cd teamchatjs.github.io
# open index.html in a browser
```

## How It Works

### Storage Architecture
Team Chat uses a **serverless storage model**:

1. **localStorage** — Each browser caches the full chat history locally for instant loads and offline support
2. **jsonblob.com** — A free public JSON storage service holds the shared "source of truth" for each room
3. **Sync loop** — Every 3 seconds, the app fetches the remote state, merges it with local changes, and pushes the result back

Each room gets its own unique blob ID, which is embedded in the invite link. This means:
- ✅ No collisions between teams sharing similar room names
- ✅ Teammates don't need to configure anything
- ✅ The app works from any static host

### Merge Strategy
When the same message is edited on two devices simultaneously, the newer `editedAt` timestamp wins. Deletions always take precedence and preserve the original content for auditability. Channels, users, and messages are merged by ID, so no data is ever lost on conflict.

### Data Retention
- **Local** — data persists in your browser until you clear site data
- **Cloud** — jsonblob keeps blobs indefinitely as long as they're actively read or written. Inactive rooms (no activity for 30+ days) may be purged; in that case, a new blob is created automatically on next use

## Usage Tips

### Creating a Team
1. Enter your display name
2. Enter a team code (e.g. `acme-engineering`, `family-chat`)
3. You'll land in the `#general` channel with an empty room
4. Click **📋 Invite** to copy a shareable link for your teammates

### Joining a Team
1. Click the invite link someone shared with you
2. Enter your name when prompted
3. You'll automatically connect to the existing room with full history

### Keyboard Shortcuts
| Shortcut | Action |
|---|---|
| `Enter` | Send message / save edit |
| `Shift + Enter` | New line |
| `Esc` | Cancel edit / close modal / close lightbox |

### Mobile Tips
- **Tap the ☰ menu** to open the sidebar
- **Tap your own message** to reveal Edit/Delete actions
- **Pinch or tap images** to open the lightbox

## Limitations

- **Max file size:** 2 MB per attachment (encoded as base64 in storage)
- **Max room size:** ~5 MB total (jsonblob limit) — enough for thousands of text messages or dozens of attachments
- **No end-to-end encryption** — data is stored in plaintext on jsonblob. Don't use for sensitive information
- **No server-side push** — messages arrive within 3 seconds of the polling interval, not instantly
- **Public storage** — anyone with a blob ID can read the room contents (treat invite links like passwords)

## Configuration

Key constants at the top of the `<script>` section in `index.html`:

```js
const MAX_FILE_SIZE = 2 * 1024 * 1024;  // 2 MB
const JSONBLOB_API = 'https://jsonblob.com/api/jsonBlob';
const SYNC_THRESHOLD_MS = 800;  // delay before showing "syncing" indicator
// Sync interval (3000ms) in initApp()
```

## Repository

**GitHub:** [https://github.com/teamchatjs/teamchatjs.github.io](https://github.com/teamchatjs/teamchatjs.github.io)

Contributions, bug reports, and feature requests are welcome via GitHub Issues and Pull Requests.

## License

MIT
