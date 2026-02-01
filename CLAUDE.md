# CLAUDE.md - ClawShare P2P

This file provides guidance for Claude Code when working with this codebase.

## Project Overview

**ClawShare P2P** is a shell-to-shell, claw-to-claw peer-to-peer file transfer protocol. The UI is scaffolding for humans—crab-to-crab transfer is the product.

### Core Philosophy: Crabs First, Humans Second

**The Truth:**
- This is NOT a file-sharing app for humans
- It's a **P2P transfer protocol** where devices find each other via GitHub Gist breadcrumbs
- Humans are clumsy facilitators who drop files or paste codes
- The UI is scaffolding, not the star

**The Mantra:**
> Make the crab-to-crab transfer unbreakable and invisible.  
> Make the human UI tolerable, not distracting.  
> If a feature makes P2P slower, flakier, or more complex → delete it.

### Visual Vibe
- 🦀 or 🦞 emoji sparingly but deliberately
- Claw-red (#FF3D00) accents on key actions
- Dark mode default (shells are dark)
- Mobile-first (most transfers: phone ↔ laptop)

### Tagline
> "Claw to claw. Shell to shell. Direct. Encrypted. No servers touched."

---

## Tech Stack

- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript
- **Styling:** Tailwind CSS with Material Design 3
- **Signaling:** GitHub Gist API (metadata only)
- **P2P:** WebRTC Data Channels (chunked transfer)
- **No file content** stored anywhere—direct browser-to-browser

## Design System

### Colors
```css
--claw-primary: #FF3D00;  /* Claw Red */
--claw-dark: #1A1A1A;
--claw-surface: #FFFFFF;
```

### Typography
- **Font:** Inter (body), Fredoka (headings/logo)
- **Scale:** Material 3 type scale

---

## Code Patterns

### Tailwind Classes
```tsx
// Buttons
<button className="btn-filled">Action</button>
<button className="btn-tonal">Secondary</button>
<button className="btn-text">Tertiary</button>

// Cards
<div className="card">Content</div>

// Typography
<h1 className="text-hero">Title</h1>
<p className="text-body">Body</p>
<span className="text-label">Label</span>
```

### GitHub Gist Integration
```typescript
// Create gist for signaling
const gist = await fetch('https://api.github.com/gists', {
  method: 'POST',
  body: JSON.stringify({
    description: 'clawshare:filename:hash',
    public: false,
    files: { 'metadata.json': { content: JSON.stringify(data) } }
  })
});
```

### WebRTC Data Channel (Chunked Transfer)
```typescript
// Sender: chunk and send
const CHUNK_SIZE = 16 * 1024; // 16KB
for (let offset = 0; offset < fileSize; offset += CHUNK_SIZE) {
  const chunk = file.slice(offset, offset + CHUNK_SIZE);
  await dataChannel.send(chunk);
}

// Receiver: accumulate chunks
dataChannel.onmessage = (event) => {
  receivedChunks.push(event.data);
  if (receivedSize >= totalSize) assembleFile();
};
```

---

## File Structure

```
clawshare-p2p/
├── src/
│   ├── app/
│   │   ├── page.tsx              # Upload UI (minimal)
│   │   ├── layout.tsx            # Root layout
│   │   ├── globals.css           # Claw branding
│   │   ├── api/
│   │   │   └── gist/
│   │   │       └── route.ts      # Gist API
│   │   └── s/[gistId]/
│   │       ├── page.tsx          # Transfer page (minimal)
│   │       └── ShareClient.tsx   # Receiver UI
│   ├── lib/
│   │   ├── p2p.ts              # WebRTC logic (PRIORITY)
│   │   ├── github.ts           # GitHub API wrapper
│   │   └── types.ts            # TypeScript types
│   └── components/             # Reusable components
├── .env.example
├── PRD.md
└── README.md
```

---

## Priority Order

### Priority Zero: Shell-to-Shell Robustness
1. Bulletproof WebRTC reconnection (auto-retry on ICE disconnect, network change, sleep/wake)
2. Chunked transfer with resume (track offset, resume from last acknowledged chunk)
3. Encryption verification badge (after DTLS handshake)
4. NAT traversal (STUN/TURN)
5. Large-file handling (2GB+ support, no memory blowup)
6. Fail gracefully with auto-reconnect

### Priority 1: Human Scaffolding (Minimal)
1. Copy-to-clipboard (tiny button)
2. QR code for phone transfers
3. File previews: Only sender's drop zone
4. Receiver minimal UI: "Incoming from claw @ [code] – [size]"
5. Simple progress: "Claw gripping... 45% (8.2 MB/s)"
6. Ugly-truth rate limiting

### Kill or Defer
- Auth / GitHub login (kills zero-friction)
- Full transfer dashboard
- Confetti, success fireworks
- Fancy settings

---

## Non-Negotiables (Test These)

1. Phone on cellular → laptop on WiFi, drop 1GB file
2. Close/reopen tab mid-transfer
3. Airplane mode toggle → must recover
4. Time-to-first-byte after code entry
5. Transfer time vs direct USB benchmark
6. Lighthouse perf > 95

**After every change:** "Does this make shell-to-shell faster/more reliable? Or just prettier for humans?"

---

## Ralph Wiggum Loop

Iterate until:
1. [ ] WebRTC auto-reconnect works (network toggle, tab background)
2. [ ] Chunked transfer with resume (mid-transfer disconnect → resume)
3. [ ] Large file support (2GB+ no memory blowup)
4. [ ] Minimal human UI (no bloat)
5. [ ] Transfer completes phone ↔ laptop

---

## Commands

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## Environment Variables

Required in `.env.local`:
- `GITHUB_TOKEN` - GitHub personal access token (scopes: gist)

---

## Notes

- This is crabs-first, humans-second
- The UI is scaffolding, not the star
- Speed + reliability > pretty
- No servers touch the payload
