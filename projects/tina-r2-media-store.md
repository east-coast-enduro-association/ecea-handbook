# Project: Tina CMS + Cloudflare R2 Media Store

## Status

**Decided Against — Not Implemented**

After evaluating the complexity of a custom Tina media store + Netlify Functions + R2 credentials, we chose to store images directly in the GitHub repository under `src/assets/`. TinaCMS Cloud uploads images via the GitHub API, and Astro optimizes them at build time.

See [cloudflare-r2.md](../cloudflare-r2.md) for the full rationale.

---

## Original Plan (Archived)

The notes below are kept for reference in case the repo grows large enough that git-based storage becomes a problem.

### Overview

Build a custom Tina media store that integrates with Cloudflare R2, allowing CMS users to upload, browse, and select media files directly through Tina's UI while storing files in R2 instead of the git repository.

### Requirements

**Hard Requirements**
- CMS users can upload files through Tina's media UI
- CMS users can browse and select files in Tina's media viewer
- Files stored in Cloudflare R2 (not git)

**Nice to Have**
- Image preview/thumbnails in media viewer
- Folder organization support

### Why R2?

| Service | Monthly Cost (10GB stored, 50GB bandwidth) |
|---------|-------------------------------------------|
| Cloudflare R2 | ~$0.15 |
| Cloudinary (paid) | $99+ |
| Uploadcare | $25 |
| Git (current) | Free but bloats repo |

R2 has $0 egress fees.

### Architecture

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────┐
│   Tina CMS UI   │ ───► │  API Routes      │ ───► │ Cloudflare  │
│  (media viewer) │      │  (Netlify Func)  │      │     R2      │
└─────────────────┘      └──────────────────┘      └─────────────┘
```

### Implementation Outline

1. **Netlify Functions** — `upload.ts`, `list.ts`, `delete.ts` wrapping the R2 S3-compatible API
2. **Custom Tina Media Store** — `tina/r2-media-store.ts` implementing `persist()`, `list()`, `delete()`
3. **Tina Config** — `media.loadCustomStore` pointing to the custom store
4. **Environment Variables** — `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET_NAME`, `R2_PUBLIC_URL`

### Resources

- [Cloudflare R2 Docs](https://developers.cloudflare.com/r2/)
- [Tina Custom Media Stores](https://tina.io/docs/reference/media/external/custom/)

---

*Created: 2025-02-03 | Decided against: 2026*
