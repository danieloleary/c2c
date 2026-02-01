# C2C (Claw to Claw) Skill

*Share files Gist-to-Gist via GitHub*

---

## Description

Upload and share files using C2C. Files stored in your GitHub Gist — transparent, auditable, yours. Free tier: 100MB files, 10 transfers/day.

## Usage

### Upload and Share

```bash
# Upload a file and create a share link
teddy: "Upload report.pdf to C2C"

# View-only mode
teddy: "Upload file.png, view-only link"
```

### Download

```bash
# Download from share link
teddy: "Download c2c.app/s/abc123"
```

### List & Manage

```bash
# List your transfers
teddy: "List my C2C transfers"
```

### CLI

```bash
# Upload file
python3 skills/c2c/upload.py report.pdf

# Download file
python3 skills/c2c/download.py abc123 --output ./
```

## Requirements

- GitHub account
- GitHub personal access token (scopes: gist)

## Configuration

Set GitHub token:
```bash
export GITHUB_TOKEN="ghp_your_token_here"
export C2C_URL="https://c2c.app"
```

## Features

- **GitHub Storage** — Files in your Gist, your control
- **Transparent** — See files on GitHub directly
- **GitHub Security** — GitHub's infrastructure
- **No Server Storage** — Files stay in YOUR GitHub
- **Free Tier** — 100MB files, 10 transfers/day

## Architecture

```
You → Your GitHub Gist → Recipient
```

Files stored in YOUR private GitHub Gist. We never touch your data.

## File Types

- Documents: PDF, DOC, DOCX, XLS, XLSX, PPT, PPTX
- Images: JPG, PNG, GIF, WebP
- Text: TXT, CSV, JSON, MD

## See Also

- Web UI: https://c2c.app
- Source: https://github.com/danieloleary/c2c

## License

MIT
