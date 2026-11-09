---
name: publish
description: "Prepare, approve and publish a finished video to connected social accounts through the content calendar. Use for upload sosmed, posting, jadwalkan konten."
license: Proprietary
metadata:
  brand: Mosio
---

# Publish a video

This is one step of the `video` skill's flow (goal, brand, type, brief, concept (the recipe), storyboard, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

Wait for `get_status` to report a finished render. Read `list_social_accounts` and `get_social_calendar`; a missing connection gives a dashboard link. Show accounts, caption and time. Save campaigns as `create_social_post` drafts, then schedule or publish the approved posts with `update_social_post`. Direct publishing requires approval too. Keep the same idempotency key when retrying; `retry_social_post` recovers uncertain submissions and failed destinations. `get_social_post` returns status and permalinks. TikTok privacy options follow the user's explicit choice.