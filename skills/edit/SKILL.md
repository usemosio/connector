---
name: edit
description: "Edit the user's own footage with Mosio: get the clip in (an upload card in the chat when it is not in Mosio yet, e.g. just filmed on their phone), look at it (stills, scene cuts, silences), cut and trim it, tighten a talking head, reframe landscape to vertical, add zooms, subtitles and text over the footage, then preview. Use whenever the user mentions their own video, photo or recording: ini videonya, video dari HP, baru aku rekam, mau upload video, edit video aku, potong video, buang bagian yang diem, kasih subtitle, jadiin Reels dari video ini, zoom in pas ngomong."
license: Proprietary
metadata:
  brand: Mosio
---

# Edit the user's footage

This is one step of the `video` skill's flow (workspace, goal, brand, material (Notes, research, Discover, references), type, brief, look (the recipe), storyboard, build, review, preview, render, post). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

The type file is `types/footage-edit.md` in the `video` skill (or `skill_library` `mosio/video/types/footage-edit.md`): read it first.

1. **Goal and brand**, as in the `video` skill: what the edit is for and where it is posted, then `list_brands`, `get_brand`.
2. **The clips.** `list_media` lists what is ready. Not there yet: `upload_media` shows the upload card in the chat, where the user picks the clips from their device (attachments passed as files: `import_media`; agents with a shell: `create_media_upload` then `complete_media_upload`), and wait until `list_media` lists them; never pretend to have footage.
3. **Look before planning.** `inspect_media` on every clip you might use: numbered stills with their times, hard cuts, silences, loudness, and what was said (an automatic transcript: phrases with times, and each word's start and end with `words`). Tell the user in one line per clip what you see and hear. You cannot watch or hear the clip: the words come from that transcript or from the user (a script, or an SRT from CapCut or YouTube; theirs wins); never invent them. The transcript can mishear names, prices and brand words: check those with the user before they go on screen. A file still being transcribed says so: inspect it again in a few minutes.
4. **Plan, then wait.** The edit plan in 4-8 plain sentences (shots by clip, frame and time; cuts; reframing; zooms; subtitles; music level; length) and at most one batch of questions. Wait for the user's OK. One exception: text behind a person on a clip and a stretch the user already named ("tulisan di belakang orang di detik 2-5") needs `make_cutout`, which takes 20-25 s per second of footage, so start it right away on that stretch and say it is being made, then plan while it runs.
5. **Build** a composition: `get_composition_guide`, then the craft in `skill_library` `mosio/video-composition/craft/`: `editing.md` (clips, cuts, rhythm, sound), `reframe.md`, `zooms.md`, `subtitles.md`, `text-effects.md` (text behind a person: `make_cutout` on that stretch first). A recipe only styles the graphics layer (titles, end card).
6. **Check** with `inspect_composition` at every cut and at the start of the longest subtitle, fix, then `preview_composition`. The user renders from the preview card, or agrees in chat.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
- On-screen quotes and subtitles are the speaker's words; never claim the footage shows something it does not.
