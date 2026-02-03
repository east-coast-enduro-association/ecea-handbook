# Project: Tina CMS + Cloudflare R2 Media Store

## Overview

Build a custom Tina media store that integrates with Cloudflare R2, allowing CMS users to upload, browse, and select media files directly through Tina's UI while storing files in R2 instead of the git repository.

## Status

**Not Started**

## Requirements

### Hard Requirements
- CMS users can upload files through Tina's media UI
- CMS users can browse and select files in Tina's media viewer
- Files stored in Cloudflare R2 (not git)
- CLI users can also upload directly to R2

### Nice to Have
- Image preview/thumbnails in media viewer
- Folder organization support
- Search/filter in media viewer

## Why R2?

| Service | Monthly Cost (10GB stored, 50GB bandwidth) |
|---------|-------------------------------------------|
| Cloudflare R2 | ~$0.15 |
| Cloudinary (paid) | $99+ |
| Uploadcare | $25 |
| Git (current) | Free but bloats repo |

R2 has $0 egress fees, making it extremely cost-effective.

## Architecture

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────┐
│   Tina CMS UI   │ ───► │  API Routes      │ ───► │ Cloudflare  │
│  (media viewer) │      │  (Netlify Func)  │      │     R2      │
└─────────────────┘      └──────────────────┘      └─────────────┘
     Browser                  Server                  Storage
        │                        │
        │   1. User clicks       │
        │      "upload"          │
        │                        │
        │   2. File sent to      │
        │      API endpoint      │
        │                        │
        │                        │   3. API uploads
        │                        │      to R2 via S3 SDK
        │                        │
        │   4. Returns URL       │
        │      for content       │
        ▼                        ▼
```

## Components to Build

### 1. Netlify Functions (API Layer)

Location: `netlify/functions/media/`

Functions needed:
- `upload.ts` - Handle file upload, store in R2, return URL
- `list.ts` - List files/folders in R2 bucket
- `delete.ts` - Remove file from R2

Why server-side? R2/S3 credentials cannot be exposed to browser.

### 2. Custom Tina Media Store

Location: `tina/r2-media-store.ts`

Implements Tina's media store interface:
```ts
interface MediaStore {
  persist(files: MediaUploadOptions[]): Promise<Media[]>;
  list(options: MediaListOptions): Promise<MediaList>;
  delete(media: Media): Promise<void>;
}
```

### 3. Tina Config Update

Location: `tina/config.ts`

```ts
media: {
  loadCustomStore: async () => {
    const { R2MediaStore } = await import("./r2-media-store");
    return R2MediaStore;
  },
},
```

### 4. Environment Variables

```env
# Cloudflare R2 Credentials
R2_ACCOUNT_ID=your-account-id
R2_ACCESS_KEY_ID=your-access-key
R2_SECRET_ACCESS_KEY=your-secret-key
R2_BUCKET_NAME=ecea-media

# Public URL for serving files
R2_PUBLIC_URL=https://media.ecea.org
# Or use R2's default: https://pub-xxx.r2.dev
```

## Prerequisites (Before Development)

1. **Create Cloudflare account** (if not exists)
2. **Create R2 bucket**
   - Name: `ecea-media` (or similar)
   - Enable public access (for serving images on website)
3. **Generate R2 API credentials**
   - Go to R2 > Manage R2 API Tokens
   - Create token with read/write access to the bucket
4. **Optional: Set up custom domain**
   - e.g., `media.ecea.org` pointing to R2 bucket

## Implementation Steps

### Phase 1: R2 Setup
- [ ] Create Cloudflare account
- [ ] Create R2 bucket `ecea-media`
- [ ] Configure public access
- [ ] Generate API credentials
- [ ] Test access with CLI (`aws s3 ls` or `rclone`)

### Phase 2: API Routes
- [ ] Install dependencies (`@aws-sdk/client-s3`)
- [ ] Create `netlify/functions/media/upload.ts`
- [ ] Create `netlify/functions/media/list.ts`
- [ ] Create `netlify/functions/media/delete.ts`
- [ ] Test endpoints locally with `netlify dev`

### Phase 3: Tina Media Store
- [ ] Create `tina/r2-media-store.ts`
- [ ] Implement `persist()` method
- [ ] Implement `list()` method
- [ ] Implement `delete()` method
- [ ] Update `tina/config.ts` to use custom store

### Phase 4: Testing
- [ ] Test upload via Tina UI
- [ ] Test browse/select in media viewer
- [ ] Test delete functionality
- [ ] Test image display on live site
- [ ] Test CLI upload (`rclone` or `aws s3 cp`)

### Phase 5: Migration
- [ ] Upload existing `public/assets/*` to R2
- [ ] Update existing content references (if URL format changes)
- [ ] Remove old assets from git (optional, can keep as backup)

## Dependencies

```json
{
  "@aws-sdk/client-s3": "^3.x",
  "@aws-sdk/s3-request-presigner": "^3.x"
}
```

## CLI Upload (Alternative Workflow)

For CLI users who want to upload directly:

```bash
# Using rclone (recommended)
rclone copy ./image.jpg r2:ecea-media/assets/blog/

# Using AWS CLI (R2 is S3-compatible)
aws s3 cp ./image.jpg s3://ecea-media/assets/blog/ \
  --endpoint-url https://<account-id>.r2.cloudflarestorage.com
```

rclone config:
```ini
[r2]
type = s3
provider = Cloudflare
access_key_id = xxx
secret_access_key = xxx
endpoint = https://<account-id>.r2.cloudflarestorage.com
```

## Cost Estimate

| Item | Amount | Cost |
|------|--------|------|
| Storage | 10GB | $0.15/mo |
| Class A ops (uploads) | 100/mo | ~$0.00 |
| Class B ops (reads) | 10,000/mo | ~$0.00 |
| Egress | Unlimited | $0.00 |
| **Total** | | **~$0.15/mo** |

## Resources

- [Cloudflare R2 Docs](https://developers.cloudflare.com/r2/)
- [Tina Custom Media Stores](https://tina.io/docs/reference/media/external/custom/)
- [AWS S3 SDK for JavaScript](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/)

## Related Files

- `ecea-ui/tina/config.ts` - Current Tina configuration
- `ecea-ui/public/assets/` - Current media location (git)

---

*Created: 2025-02-03*
*Last Updated: 2025-02-03*
