# Hallway — Frontend

> A minimal, dark-themed real-time chat UI built with vanilla HTML, CSS, and JavaScript.

![Hallway UI](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-c8f03a?style=flat-square&labelColor=111)
![WebSocket](https://img.shields.io/badge/transport-WebSocket-c8f03a?style=flat-square&labelColor=111)
![Mobile Ready](https://img.shields.io/badge/mobile-responsive-c8f03a?style=flat-square&labelColor=111)

---

## Overview

Hallway's frontend is a single `index.html` file — no build step, no framework, no dependencies beyond two Google Fonts. It connects to the Hallway Go backend over WebSocket and handles real-time messaging, user count updates, and system announcements.

---

## Features

- **Real-time messaging** via WebSocket — messages appear instantly across all connected clients
- **Chat history replay** — on connect, the last 100 messages are loaded automatically
- **Live user count** — the header updates as people join and leave
- **System announcements** — join/leave events are shown inline in a distinct style
- **Anonymous fallback** — if no username is entered, the server assigns an `Anon_HHmmss` name
- **Smart auto-scroll** — only scrolls to the bottom if you're already near it; scrolling up to read history won't interrupt you
- **Mobile responsive** — full-screen layout on small screens, safe area insets for notched phones, landscape support
- **No build step** — open the file directly or serve it statically

---

## Getting Started

### 1. Serve the file

You can open `index.html` directly in a browser, or serve it via the Go backend:

```
hallway/
    └── index.html
```

### 2. Configure the WebSocket URL

In `index.html`, find this line near the bottom:

```js
const socket = new WebSocket(`${protocol}://hallwayserver.fly.dev/chat?username=...`);
```

Change the hostname to match your backend:

| Environment | URL |
|---|---|
| Local dev | `localhost:8080` |
| Fly.io deploy | `hallwayserver.fly.dev` |
| Custom domain | `your-domain.com` |

The protocol (`ws://` vs `wss://`) is selected automatically based on whether the page is served over HTTP or HTTPS.

---


## Message Protocol

The frontend speaks a simple JSON protocol over WebSocket.

**Sending a message:**
```json
{
  "type": "chatMessage",
  "payload": { "username": "alice", "message": "Hello!" }
}
```

**Receiving — chat message:**
```json
{ "type": "chatMessage", "payload": { "username": "alice", "message": "Hello!" } }
```

**Receiving — system event:**
```json
{ "type": "system", "payload": { "text": "alice joined" } }
```

**Receiving — user count:**
```json
{ "type": "userCount", "payload": { "count": 3 } }
```

> The server always overwrites the `username` field with the server-resolved name, so anonymous users always display correctly.

---

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). Requires WebSocket support — available in every browser released after 2012.

---

## Built by

[DeleMike](https://github.com/DeleMike)