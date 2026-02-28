# Web Serial Terminal

A browser-based serial terminal with real-time keyword filtering — no installation required.

Share a single URL and your whole team can use it instantly.

<img width="1970" height="1339" alt="image" src="https://github.com/user-attachments/assets/dde58013-8294-49b2-9ee0-6e5baeffb345" />





---

## Why This Exists

Tools like PuTTY and MobaXterm work fine for basic serial monitoring, but lack decent keyword filtering. When you're staring at a flood of embedded system logs, being able to highlight and isolate specific lines in real time makes a huge difference.

This terminal runs entirely in the browser using the **Web Serial API** — no backend, no install, no runtime dependencies. Just open the page and connect.

---

## Features

- **Serial port connect / disconnect** with configurable baud rate
- **Real-time log view** with auto-scroll (pauses when you scroll up)
- **Keyword filtering** — matched lines appear in a separate panel instantly
- **Per-keyword options**: Match case, Whole word
- **Keyword on/off toggle** — click the tag to temporarily disable without removing
- **Color-coded highlights** — each keyword gets its own highlight color
- **Demo mode** — test the UI without any hardware (Slow / Normal / Fast / Flood)
- **Memory management** — main log capped at 10,000 lines, filtered at 5,000

---

## Browser Support

| Browser | Supported |
|---------|-----------|
| Chrome 89+ | ✅ |
| Edge 89+ | ✅ |
| Firefox | ❌ (Web Serial API not supported) |
| Safari | ❌ (Web Serial API not supported) |

---

## Getting Started

### Option 1 — Local file server (recommended)

```bash
# Python (usually pre-installed)
python -m http.server 8080

# or Node.js
npx serve .
```

Then open `http://localhost:8080` in Chrome or Edge.

> **Why not just open `index.html` directly?**
> The Web Serial API requires a secure context (HTTPS or localhost). Opening via `file://` may not work in some browsers.

### Option 2 — GitHub Pages

Enable GitHub Pages on this repo and share the URL with your team. No server setup needed.

---

## Usage

### Connecting to a device

1. Click **Select Port** — the browser shows a list of available serial ports
2. Choose the baud rate matching your device (default: 115200)
3. Click **Connect**

> The browser requires you to manually select a port each time (Web Serial API security policy — auto-connect is not allowed).

### Adding keywords

1. Type a keyword in the input box
2. Pick a highlight color
3. Set options if needed:
   - **Match case** — `ERROR` won't match `error`
   - **Whole word** — `err` won't match inside `error`
4. Click **Add** or press Enter

Matched lines appear in the **Filtered Log** panel on the right in real time.

### Keyword tags

| Action | Result |
|--------|--------|
| Click keyword text | Toggle on / off |
| `×` button | Remove keyword |
| `Aa` badge | Case-sensitive mode active |
| `W` badge | Whole word mode active |

### Demo mode

Click **▶ Demo** in the toolbar to replay a set of sample embedded system logs (Linux boot + sensor data mix). Useful for testing keyword filters without hardware.

Speed options: Slow (800ms) · Normal (200ms) · Fast (50ms) · Flood (10ms)

---

## Line Ending Support

Handles all common line endings automatically:

| Format | Used by |
|--------|---------|
| `\r\n` | Windows |
| `\n` | Linux / Unix |
| `\r` | micro:bit, some Arduino sketches |

---

## Project Structure

```
WebSerialTerminal/
└── index.html    ← entire app (HTML + CSS + JS, single file)
```

No build tools. No dependencies. No node_modules.

---

## Tested Devices

| Device | Baud Rate | Notes |
|--------|-----------|-------|
| BBC micro:bit | 115200 | Uses `\r` line endings |

---

## Roadmap (v2)

- [ ] Log file save (`.txt` / `.log`)
- [ ] Timestamp display option
- [ ] Per-keyword match counter
- [ ] Custom baud rate input
- [ ] Resizable panel divider
