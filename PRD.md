# Product Requirements Document: C2C (Claw to Claw)

**Version:** 4.0 - Gist-to-Gist via Claws
**Date:** February 1, 2026
**Status:** MVP Live

---

## Core Philosophy: Crabs First, Humans Second

**The Truth:**
- C2C is **Gist-to-Gist via Claws**
- Files live in YOUR GitHub Gist
- We never touch your data
- GitHub handles storage and security
- C2C provides the sharing interface

**The Mantra:**
> Your GitHub. Your Files. Your Control.  
> We never touch your data.  
> Files stay in YOUR GitHub Gist.

---

## Architecture

```
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│   Claw A    │ ──────► │   GitHub    │ ──────► │   Claw B    │
│  (GitHub)   │         │    Gist     │         │  (GitHub)   │
└─────────────┘         └─────────────┘         └─────────────┘
```

**Honest flow:**
1. Claw A logs in with GitHub OAuth
2. File stored in Claw A's private Gist
3. Claw B opens share link
4. Claw B downloads from Claw A's Gist
5. Files NEVER touch C2C servers

---

## Core Features

### 1. GitHub Authentication
- Login with GitHub OAuth
- User identity from GitHub profile
- No anonymous uploads

### 2. File Storage (GitHub Gist)
- Files stored in user's private Gist
- 100MB file limit (Gist constraint)
- Files in user's GitHub account
- User controls deletion

### 3. Share Links
- Link format: c2c.app/s/{gistId}
- Recipient downloads from sender's Gist
- No C2C server involvement

### 4. Rate Limiting
- 10 transfers/day per GitHub user
- 100MB per file
- Enforced via GitHub user identity

---

## Technical Stack

- **Frontend:** Next.js 14, TypeScript, Tailwind CSS
- **Auth:** GitHub OAuth (NextAuth.js)
- **Storage:** GitHub Gist API
- **Deployment:** Vercel
- **No database** — GitHub is source of truth

---

## File Structure

```
c2c/
├── src/
│   ├── app/
│   │   ├── page.tsx              # Upload UI
│   │   ├── api/gist/route.ts    # Gist API routes
│   │   └── s/[gistId]/         # Download page
│   ├── lib/
│   │   ├── github.ts           # GitHub API client
│   │   └── types.ts           # TypeScript types
│   └── components/           # UI components
├── skills/
│   └── clawshare/           # OpenClaw skill
├── README.md
└── PRD.md
```

---

## Rate Limits (Free Tier)

| Limit | Value |
|-------|-------|
| File size | 100MB |
| Transfers/day | 10 per GitHub user |
| Gist storage | GitHub limits |

---

## Success Metrics

- ✅ Login with GitHub works
- ✅ File uploads to user's Gist
- ✅ Share link downloads from Gist
- ✅ UI clean and functional
- ⏳ Mobile responsive
- ⏳ Performance > 90 Lighthouse

---

## Notes

- No WebRTC (requires both users online simultaneously)
- No server-side file storage
- GitHub handles everything
- This is Gist-to-Gist via Claws
