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

## Technical Documentation

For developers working on the website code, see the [ecea-ui repository README](https://github.com/east-coast-enduro-association/ecea-ui).
