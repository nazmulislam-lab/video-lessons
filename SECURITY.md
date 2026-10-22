# Security

## Reporting a vulnerability

Please don't open a public issue for security problems. Use GitHub's private reporting instead: **Security → Report a vulnerability** on this repository. Include what you found, how to reproduce it, and what it could affect.

## What's in scope

- `skills/video-lessons/scripts/` (handling of the Apify token, URLs, and files it writes)
- `install.sh`
- Instructions in `SKILL.md` that could lead an agent to run untrusted content (prompt injection through transcripts or web pages)

## Your Apify token

Keep it in `~/.config/video-lessons/.env` (mode 600) or the `APIFY_TOKEN` environment variable. Never paste it into an agent chat, an issue, or a commit. If you think it leaked, regenerate it at https://console.apify.com/settings/integrations; the old one stops working immediately.
