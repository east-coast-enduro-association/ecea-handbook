# Cloudflare R2 (Media Storage)

Cloudflare R2 stores images, videos, and other media files for the website.

## Why Separate Storage?

- Keeps the website code repository small and fast
- Cheaper and faster than storing large files in Git
- Images load quickly for visitors via Cloudflare's global network

## Who Manages This?

The **Webmaster** handles media uploads. Regular members don't need access to R2.

## Uploading Media for the Website

If you have images or files for the website:

1. Send them to the Webmaster via:
   - Google Drive (share the file/folder)
   - Zulip (direct message with attachment)
2. Include:
   - Where the image should appear on the site
   - Alt text (description for accessibility)
   - Any cropping/sizing preferences

## Technical Details

For the webmaster and developers:

- **Service**: Cloudflare R2
- **Access**: Via Cloudflare dashboard or S3-compatible API
- **Integration**: Tina CMS configured to use R2 for media storage
- **Public URL pattern**: `https://[bucket].r2.dev/[filename]` (or custom domain)

See the [ecea-ui repository](https://github.com/ECEA/ecea-ui) for technical configuration.
