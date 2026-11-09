---
name: brief
description: "Write a short video brief with the user before anything is built: the goal, the one thing sold, launched, taught or told, for whom, how it ends (an action and contact when selling, a payoff when telling or teaching), size, duration and feel. Starts from the goal, reads the brand when there is one, and asks what is missing in one batch of at most 5 questions. Use when the user wants help deciding what the video should say (bantu susun brief, bingung mau bikin video apa, briefing video)."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md"
metadata:
  brand: Mosio
---

# Video brief

This is one step of the `video` skill's flow (workspace, goal, brand, material (Notes, research, Discover, references), type, brief, look (the recipe), storyboard, build, review, preview, render, post). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

`brief.md` beside this file is the reference: read "Order", "Needed before the storyboard", "Questions: one short batch", "Push to shot level", "Combined check" and "Brief format".

1. **Goal, then brand.** Know what the video is for (sell, launch, a story, a lesson, a channel's series, an announcement). Then `list_brands`; with several, ask which one unless the request names it. Then always `get_brand` with that `brand_id`, before writing anything about the business (the examples in `brief.md` are illustrations, never this brand): what it sets is answered and never asked again; what it lists as not set goes on the question list. No brand and the video is a story, a lesson or a creator's post: go on without one.
2. **Take what the user said**, then ask what is missing in one numbered batch of at most 5 short questions, and wait: owners answer on a phone between customers, and a long list or a second round goes unanswered. A reference video the user shares: get it in and break it down first (`brief.md`, "A reference video"); public social links use `import_reference`, then `inspect_media`; unavailable links need a file or screenshots.
3. **Write the brief** in the "Brief format". Pick the video type with the table in the `video` skill and read its type file (`types/<type>.md` in the `video` skill, or `skill_library` `mosio/video/types/<type>.md`).
4. **Next:** offer the content storyboard (the `storyboard` skill). Build nothing yet.

Start straight with the questions or the brief: no preamble, no explaining this skill. Reply in the user's language; on-screen text defaults to Indonesian for Indonesian businesses.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
