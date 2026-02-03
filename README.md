# ECEA Technology Handbook

This handbook documents the technology systems used by ECEA and how they work together. It's written for board members and volunteers who need to understand our tools without diving into technical details.

## Our Tech Stack

| System | Purpose | Who Uses It |
|--------|---------|-------------|
| [Google Workspace](google-workspace.md) | Documents, email, file storage | All members |
| [Zulip](zulip.md) | Team chat and discussions | All members |
| [Website (ecea.org)](website.md) | Public-facing website | Webmaster |
| [Cloudflare R2](cloudflare-r2.md) | Website images and media | Webmaster |

## How It All Connects

```
┌─────────────────────────────────────────────────────┐
│                    Members                          │
└─────────┬───────────────────┬───────────────────────┘
          │                   │
          ▼                   ▼
┌─────────────────┐   ┌───────────────┐
│ Google Workspace│   │    Zulip      │
│ (docs, email)   │   │   (chat)      │
└─────────────────┘   └───────────────┘

┌─────────────────────────────────────────────────────┐
│                  Public Website                     │
│            (Astro + Tina CMS + Netlify)            │
└─────────────────────┬───────────────────────────────┘
                      │
                      ▼
              ┌───────────────┐
              │ Cloudflare R2 │
              │ (media files) │
              └───────────────┘
```

## Quick Links

- [Getting Started (New Members)](getting-started.md)
- [Who to Contact](contacts.md)

## Technical Documentation

For developers and technical contributors:

- [ecea-ui Repository](https://github.com/ECEA/ecea-ui) - Website source code and technical docs
