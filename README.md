# ECEA Technology Handbook

This handbook documents the technology systems used by ECEA and how they work together. It's written for board members and volunteers who need to understand our tools without diving into technical details.

## Our Tech Stack

| System | Purpose | Who Uses It |
|--------|---------|-------------|
| [Google Workspace](google-workspace.md) | Documents, email, file storage | All members |
| [Zulip](zulip.md) | Team chat and discussions | All members |
| [Website (ecea.org)](website.md) | Public-facing website | Webmaster |

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
│            (Astro + TinaCMS + Netlify)              │
│                                                     │
│  Content editing → TinaCMS Cloud → GitHub commit   │
│  Netlify detects push → builds and deploys (~1 min)│
└─────────────────────────────────────────────────────┘
```

## Quick Links

- [Getting Started (New Members)](getting-started.md)
- [Who to Contact](contacts.md)

## Technical Documentation

For developers and technical contributors:

- [ecea-ui Repository](https://github.com/east-coast-enduro-association/ecea-ui) — Website source code
- [ecea-tools Repository](https://github.com/east-coast-enduro-association/ecea-tools) — Admin scripts (member roster, MotoTally scraper)
- [ecea-handbook Repository](https://github.com/east-coast-enduro-association/ecea-handbook) — This handbook
