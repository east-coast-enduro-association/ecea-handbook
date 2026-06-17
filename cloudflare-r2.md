# Cloudflare R2 (Media Storage)

## Current Status

Cloudflare R2 was evaluated as an external media storage solution for the website but was **not implemented**. Website images are stored directly in the GitHub repository under `src/assets/` instead.

See [website.md](website.md) for how images are managed today.

## Why We Evaluated R2

The original concern was that storing images in git would bloat the repository. R2 was attractive because:

- No egress fees (unlike S3 or Cloudinary)
- S3-compatible API
- Cheap storage (~$0.015/GB/month)

## Why We Chose Git-Based Storage Instead

- TinaCMS Cloud uploads images directly to the GitHub repo via the GitHub API — no extra infrastructure needed
- Astro optimizes all images at build time (converts to WebP, generates responsive sizes), so large source files don't impact page load speed
- Keeping images in git means content and media are versioned together and deployments are fully self-contained
- The complexity of a custom Tina media store + Netlify Functions + R2 API credentials was not worth it for our scale

## If We Revisit R2 in the Future

The original implementation plan is documented in [projects/tina-r2-media-store.md](projects/tina-r2-media-store.md). The main trigger for revisiting would be if the repository grows large enough to slow down Netlify builds or GitHub operations.
