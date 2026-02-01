# CLAUDE.md - C2C (Claw to Claw)

This file provides guidance for Claude Code when working with C2C.

## Project Overview

**C2C = Gist-to-Gist via Claws**

A file sharing platform where:
- Files live in YOUR GitHub Gist
- We never touch your data
- GitHub handles storage and security
- C2C provides the sharing interface

**Honest architecture:**
```
You → Your GitHub Gist → Recipient
```

No P2P. No WebRTC. Files stay in YOUR GitHub.

## Tech Stack

- **Frontend:** Next.js 14 (App Router)
- **Language:** TypeScript
- **Styling:** Tailwind CSS (Cobra Kai red/black theme)
- **Auth:** GitHub OAuth
- **Storage:** GitHub Gist API (no database)
- **Deployment:** Vercel

## Design System

### Colors
```css
--claw-primary: #E53935;  /* Red */
--claw-dark: #0A0A0A;    /* Black */
--claw-surface: #121212;
```

### Typography
- **Font:** Inter (body), Fredoka (headings/logo)

## Code Patterns

### GitHub Gist Upload
```typescript
const gist = await fetch('https://api.github.com/gists', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${token}`,
  },
  body: JSON.stringify({
    description: 'c2c:filename',
    public: false,
    files: { filename: { content: base64Data } }
  })
});
```

### GitHub Gist Download
```typescript
const gist = await fetch(`https://api.github.com/gists/${gistId}`, {
  headers: { Authorization: `Bearer ${token}` }
});
```

## File Structure

```
src/
├── app/
│   ├── page.tsx              # Upload UI
│   ├── api/gist/route.ts    # Gist API routes
│   └── s/[gistId]/         # Download page
├── lib/
│   ├── github.ts           # GitHub API client
│   └── types.ts           # TypeScript types
└── components/           # UI components
```

## Key Points

- No P2P/WebRTC
- No server-side file storage
- GitHub Gist is the only storage
- Rate limited by GitHub user
- Files stay in user's account

## Commands

```bash
npm run dev    # Local development
npm run build # Production build
npm run start # Production server
```

## Environment

```bash
GITHUB_TOKEN=ghp_...
GITHUB_CLIENT_ID=...
GITHUB_CLIENT_SECRET=...
```

## Notes

- This is Gist-to-Gist via Claws
- No real P2P transfer
- GitHub handles everything
- User controls their data
