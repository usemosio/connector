---
name: audio
description: "Add narration, dialogue, music and timed sound effects to a video with the workspace's own voice provider keys and the bundled SFX library. Use for voiceover, narasi, musik, efek suara."
license: Proprietary
metadata:
  brand: Mosio
---

# Video audio

This is one step of the `video` skill's flow (workspace, goal, brand, material (Notes, research, Discover, references), type, brief, look (the recipe), storyboard, build, review, preview, render, post). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

Show the spoken script before generating it. `list_voices` offers samples; `create_voiceover` uses the workspace's saved Gemini or ElevenLabs key and bills that provider account. Never request keys in chat; missing-key results link to settings. Inspect the resulting media for word times and use its asset path in the composition. Common SFX are free in the bundled library (`skill_library` `mosio/video-composition/sfx.md`); `create_sound` uses ElevenLabs for custom SFX or music. Match cues to visible motion, mix music low under speech and fade the ending. Save the script and cues in the production plan; preview before rendering.