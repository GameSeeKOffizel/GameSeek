# 🏗️ Architecture Overview

GameSeek is a full-stack real-time communication platform with a desktop app, web app, and a Python backend.

-----

## Tech Stack

|Layer        |Technology        |
|-------------|------------------|
|Desktop App  |Electron          |
|Frontend     |React + TypeScript|
|Backend      |Python (asyncio)  |
|Real-time    |WebSockets        |
|Voice / Video|WebRTC            |
|Database     |SQLite            |
|Server       |Debian VPS        |
|Reverse Proxy|Nginx             |
|Email        |SendGrid          |

-----

## High-Level Architecture

```
┌─────────────────────────────────────┐
│           Client (Frontend)         │
│                                     │
│  ┌──────────────┐  ┌─────────────┐  │
│  │  Electron    │  │  Browser    │  │
│  │  (EXE)       │  │  (Web App)  │  │
│  └──────┬───────┘  └──────┬──────┘  │
│         │                 │         │
│    ws://:8765        wss://         │
│         │           /ws (Nginx)     │
└─────────┼───────────────┬───────────┘
          │               │
          ▼               ▼
┌─────────────────────────────────────┐
│         Python Backend              │
│                                     │
│  WebSocket Server  │  HTTP Server   │
│  (port 8765)       │  (port 8080)   │
│                                     │
│         SQLite Database             │
└─────────────────────────────────────┘
```

-----

## Key Design Decisions

### Electron vs Browser Detection

The frontend detects its environment at runtime:

```typescript
const isElectron = !!(window as any).require;
```

This controls which WebSocket URL is used and enables platform-specific features like native Windows notifications.

### WebRTC Voice & Screen Share

- Peer-to-peer connections via WebRTC
- The Python backend acts as a **signaling server** (no media relay)
- Audio processing: noise suppression via browser APIs
- Speaking detection via `setInterval` + `useRef` (prevents stream flicker)

### Real-time Messaging

- All chat messages, edits, and deletes are broadcast over WebSocket
- Right-click context menus allow inline edit/delete with live broadcast

### Email & Support

- Support tickets use `GS-XXXXXXXX` format IDs
- Email sent via SendGrid from `support@gameseekapp.xyz`
- 6-digit email verification codes for register/login

-----

## Folder Structure (planned)

```
GameSeek/
├── frontend/          # Electron + React app
│   └── src/
│       ├── components/
│       └── pages/
├── backend/           # Python WebSocket + HTTP server
├── docs/              # This documentation
├── CHANGELOGS/        # Version history
└── README.md
```
