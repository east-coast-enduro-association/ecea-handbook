# Website (ecea.org)

Our public website is where we share information with the community.

## Overview

- **URL**: [ecea.org](https://ecea.org)
- **Hosted on**: Netlify (automatic deployment on every push to `master`)
- **Built with**: Astro (static site generator) + TinaCMS Cloud (content editor)
- **Source code**: [east-coast-enduro-association/ecea-ui](https://github.com/east-coast-enduro-association/ecea-ui)

## Who Manages the Website?

The **Webmaster** (see [Contacts](contacts.md)) handles:
- Content updates
- Technical maintenance
- Adding new pages

## Requesting Changes

If you need something updated on the website:

1. Post in the **#website** Zulip stream
2. Include:
   - What page needs updating
   - What the change should be
   - Any images or documents to include

## How Content Gets Updated

1. Webmaster logs into TinaCMS at [ecea.org/admin](https://ecea.org/admin) (requires GitHub authentication)
2. Makes the changes and saves
3. TinaCMS commits the change directly to the GitHub repository
4. Netlify detects the new commit and rebuilds the site automatically (~1 minute)

For code changes (new features, layout changes), the webmaster edits the [ecea-ui repository](https://github.com/east-coast-enduro-association/ecea-ui) directly and pushes to `master`.

## Images and Media

Images are stored directly in the website's GitHub repository under `src/assets/`. When you upload an image through the TinaCMS editor, it is committed to the repo automatically — no separate media service is needed.

Supported upload locations by content type:

| Content | Upload Location |
|---------|----------------|
| Event flyers | `src/assets/events/flyers/` |
| Club logos | `src/assets/clubs/logos/` |
| Board photos | `src/assets/board/photos/` |
| Sponsor logos | `src/assets/sponsors/logos/` |
| Blog images | `src/assets/blog/` |

All images are automatically optimized (converted to WebP, resized for different screens) when the site builds.

> **Always add alt text to images.** When you add an image to a blog post, the
> editor has an "alt" field for a short description of the picture. This is
> **required** — if you add an image but leave the alt text blank, the next
> website build will fail and no changes (yours or anyone else's) will go live
> until it's fixed. Alt text is also what screen readers announce, so it helps
> visually impaired visitors. One short sentence describing the image is enough.

## What You Can Edit via TinaCMS

- **Blog Posts** — News, announcements, event recaps
- **Events** — Add events with flyers, registration links, and MotoTally IDs
- **Clubs** — Club info, logos, contacts
- **Board Members** — Executive board and Board of Trustees
- **Staff Contacts** — Series directors, referees, coordinators
- **Sponsors** — Add/remove sponsors, set title sponsor
- **Team Results** — Upload CSV results after each enduro team event
- **Series Pages** — Edit series descriptions and documents
- **Get Started / FAQ** — Static page content

## Technical Maintenance Notes

For the webmaster / anyone maintaining the site code. Fuller detail lives in the
repo's [`CODEBASE.md`](https://github.com/east-coast-enduro-association/ecea-ui/blob/master/CODEBASE.md)
and [`CMS_SETUP.md`](https://github.com/east-coast-enduro-association/ecea-ui/blob/master/CMS_SETUP.md).

- **Framework is pinned to Astro 5 on purpose.** The Tailwind integration caps at
  Astro 5, and Astro 6 removes the content-collections API the site uses
  everywhere. Moving to Astro 6/7 is a planned migration, not a routine upgrade —
  don't bump it casually.
- **Netlify builds on Node 22** (`NODE_VERSION` in `netlify.toml`), running
  `npm run build` — which is `astro build` **only**. It does **not** rebuild the
  TinaCMS `/admin` page.
- **The `/admin` editor is served from committed files** under `public/admin/`.
  If you ever run `tinacms build` locally, be careful committing the result:
  TinaCMS 3 writes a `.gitignore` that hides its own freshly built files, and
  committing that half-state makes `/admin` break in production. See `CMS_SETUP.md`.
- **Verifying a deploy**: production deploys from `master` do not report status
  back to GitHub (only pull-request previews do). Confirm a deploy by loading the
  live site or checking the Netlify dashboard.

## Planned Changes

- **Moving off Tina Cloud hosting** is intended but **not done yet**. Today the
  site still uses Tina Cloud for CMS login and saving. TinaCMS (the editor itself)
  is expected to stay; what changes is where its backend is hosted. This section
  will be updated when the move actually happens.
- **Media storage stays in git for now.** Cloudflare R2 was evaluated and
  **nothing is stored there** — see [cloudflare-r2.md](cloudflare-r2.md).

## Technical Documentation

For developers working on the website code, see the [ecea-ui repository README](https://github.com/east-coast-enduro-association/ecea-ui).
