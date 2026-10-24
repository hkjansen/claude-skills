---
name: "jan-vidiq-researcher"
description: "Pull YouTube data from VidIQ for the @JansenLu channel: a quick check (search volume, title scores, competitors) or a full dated research brief. Use whenever Jansen mentions VidIQ, search demand, title scores, or competitor checks for a video idea."
---

# YT VidIQ Research

## What this skill does

Gets measured VidIQ data for a YouTube video idea and saves it as a dated HTML research brief in Jansen's local folder. The HTML must include a visual evidence gallery containing a screenshot for every successful VidIQ data call used in the brief. It is the data-gathering step of the channel pipeline:
1. `jan-vidiq-researcher` (this skill): pull the data.
2. `jan-vidiq-analyzer`: read the data, check it against the channel, stress-test, decide.
3. `yt-video-story-builder`: write the story doc. Its Step 1 expects a brief made by this skill.

Four modes. Pick from the request, and ask only if it is unclear:

| Mode | Use when Jansen says... | Output |
|---|---|---|
| **Quick pull** | "how much do people search X", "score these titles", "who has covered Y", "is this still rising" | Short dated file `VidIQ_QuickPull_<Topic>_<YYYY-MM-DD>.html`, or a dated UPDATE section in the topic's existing HTML brief |
| **Full brief** | "research this idea with VidIQ", "compare these ideas", before a new story doc | `VidIQ_Research_<Topic>_<YYYY-MM-DD>.html` |
| **Second pass** | Jansen changes the angle after reading a brief (example: Gemini TTS reshaped into a Cantonese shootout vs MiniMax and ElevenLabs) | A dated `UPDATE (second pass)` section in the same HTML brief, with screenshots for the new calls |
| **Refresh** | "recheck", "is it still rising", "before I publish" (same angle, later date) | A dated UPDATE section with old vs new numbers side by side and screenshots for the refresh calls |

The core discipline: every claim about search demand or competition traces to a VidIQ number or a named video with a view count. Never write "zero competition". Write "no competitor found in this angle/language", which can be checked.

## Fixed details (do not re-fetch)

- Channel: @JansenLu, channel ID `UC-lhqFJ5Rvs7A7-n7Wge3kQ`. Lane: AI tools for HK/TW marketers.
- Local folder (all output goes here, nowhere else): `/Users/tsanchulu/Documents/Cowork/01_Active Projects/Jansen YT Channel/` (in device_bash: `$HOME/mnt/Jansen YT Channel`). Write files there directly with device_bash. Never leave output only in the cloud workspace or the claude.ai Project.
- Writing rules for visible copy: no em dashes and no double dashes; British English; Chinese text is standard written Traditional Chinese (no Cantonese); product and feature names stay in English inside Chinese text; be critical, not just agreeable.
- The deliverable is HTML, not Markdown. Use semantic headings, readable tables, responsive layout, accessible image alt text and high contrast. Keep the HTML understandable without relying on the screenshots alone.

## Step 0: Prepare (always, takes a minute)

1. **Read what already exists in the folder** before pulling anything:
   - The newest `handover*.md` file (working notes from the last session).
   - The newest `VidIQ_Research_*.html` and any older `.md` brief on the same topic (including `Claude outputs/`). Do not re-pull keywords pulled in the last few days unless momentum is the question.
   - `JansenLu_YouTube_Channel_Audit_2026-07-26.md` for the channel's baseline, title formula, retention and traffic mix. Cite it; do not re-derive it from a short analytics window.
2. **Get these from Jansen** (one AskUserQuestion call; if he is away, make the obvious assumption and state it at the top of the file):
   - The topic or shortlist, and the angle he has in mind.
   - Exact product names and version numbers (for example "Nano Banana 2.1" vs "2.1 Thinking").
   - Whether a Shorts cut is planned (Shorts titles are scored with `type: "short"`).
   - Any VidIQ screenshots he already took. These count as real VidIQ data.
3. **Pin down what the topic actually is** if it is new. Example: Grok Bot is xAI's agent product, not the Grok chatbot; "grok ai" is a different, falling keyword. Product facts can come from web research, but web research never fills a VidIQ number.
4. **Write the query plan** before calling anything: seed keyword, 3 to 6 variants (product name, Chinese phrasing, broader category, long-tail, a "vs" phrasing), 2 to 4 competitor search phrasings in both English and Chinese, and 3 to 6 candidate titles.
5. **Load the tools in one ToolSearch call:**
   `select:mcp__VidIQ__vidiq_user_channels,mcp__VidIQ__vidiq_balance,mcp__VidIQ__vidiq_keyword_research,mcp__VidIQ__vidiq_score_title,mcp__VidIQ__vidiq_youtube_search,mcp__VidIQ__vidiq_similar_videos,mcp__VidIQ__vidiq_channel_analytics,mcp__VidIQ__vidiq_video_stats`
6. **Start the evidence manifest before the first call.** For every VidIQ call, record the tool name, exact inputs, date and time, returned data, credits charged if shown, success or failure, and the screenshot filename. Keep the exact connector output available until the HTML has been verified. Do not rely on memory or a prose summary.
7. **Health check and credits:** call `vidiq_user_channels` (0 credits) and `vidiq_balance`. If either fails for a reason other than a fixable parameter mistake, STOP and tell Jansen first, in plain words, which tool failed and which part of the data is missing. Do not continue quietly, and do not bury it in a footnote.
8. **Estimate the cost and say it** before a big batch. Read the current tool descriptions because prices can change. Compare the opening and closing balance and record the actual credits used at the top of the HTML. If the tool-reported costs and balance change disagree, keep both figures and explain the discrepancy.

## Step 1: Call recipes (copy and fill in)

**A. Search demand**
```
vidiq_keyword_research  mode="research"  keyword="<seed>"  country="HK"
   (repeat for each variant; country="HK" adds in-country volume, use "TW" for Taiwan angles)
vidiq_keyword_research  mode="country_search"  keyword="<product name>"  country="HK"
   (every HK keyword containing the product name, ranked by HK volume)
vidiq_keyword_research  mode="matching_terms"  keyword="<seed>"
   (long-tail variants that keep the seed words)
```
From the seed's related keywords, pick the variants worth a follow-up call.

**B. Title scores** (one call per title)
```
vidiq_score_title  title="<title>"  type="long"  channelId="UC-lhqFJ5Rvs7A7-n7Wge3kQ"
```
`type` is required; leaving it out errors (retry with it, that is not a connector failure). Vary the titles on language (ZH vs EN), structure (numbered, curiosity gap, benefit-led) and whether the product name is in the title.

**C. Competitors**
```
vidiq_youtube_search  query="<phrasing>"  type=["video"]  order="date"  regionCode="HK"
                      publishedAfter="<launch date>T00:00:00Z"
   (2 to 4 phrasings; at least one English and one Chinese; for Chinese try 怎麼用 / 實測 / 教學 / 教程 / 比較, not just one)
vidiq_similar_videos  videoId="<strongest hit>"
   (widens the set from one strong competitor)
```
For each relevant hit record channel, subscribers, views, VPH and outlier score.

**D. Channel pace**
```
vidiq_channel_analytics  channelId="UC-lhqFJ5Rvs7A7-n7Wge3kQ"  report="top_videos"
   (default window is the last 28 days; add startDate/endDate as needed)
vidiq_channel_analytics  ...  dimensions=["video"]  metrics=["engagedViews","views","averageViewDuration"]
   (for comparing the channel's own videos; since 27 Aug 2026 use engagedViews for cross-date comparisons)
```
This is a pace check, not a replacement for the channel audit.

### Evidence capture during every call

Capture one complete visual record immediately after each successful VidIQ call that contributes data to the brief. The image must show the tool name or a clear call label, the query or parameters, every returned row or value used in the brief, and the call date. Do not crop out relevant data.

Use this priority order:

1. A native screenshot of the VidIQ connector result card when the host allows it.
2. If the host does not allow a native tool-card screenshot, render the exact connector output into a clean PNG data card. Label it visibly: `VidIQ connector snapshot, rendered from exact result. Not a native VidIQ website screenshot.`

Never use image generation, OCR, web-search numbers or hand-reconstructed values to make evidence. If the exact output is no longer available, mark the evidence as missing and rerun the call only when the credit cost is justified. A screenshot supplied by Jansen counts as native evidence and should be preserved with its original date.

Failed calls do not require a screenshot unless the failure explains missing data. Record failures and retries in the call manifest. A corrected retry is a separate call. Repeated one-row title score calls may share one evidence image only when every title, score, call label and cost is fully visible. Otherwise use one image per call.

## Step 2: Second pass (when the angle changes)

When Jansen reshapes the idea after reading a brief, the first pass's keywords and competitors no longer answer the right question. Run the steps again for the new angle only:
1. New keywords specific to the new angle (Cantonese example: "cantonese tts", "廣東話 AI 配音", "minimax tts", "elevenlabs cantonese").
2. New competitor searches. Widen the date window if the new angle is not tied to a launch (the Cantonese pass searched back to Oct 2025).
3. New title scores for the new framing.
4. Reuse the channel snapshot if it is under about a week old.
5. Add a dated `<section>` headed `UPDATE [YYYY-MM-DD] (second pass): [new angle]` in the same HTML brief, state what it supersedes, and end with a side-by-side "original angle vs new angle" comparison. Add the new screenshots to the evidence gallery and manifest without removing the earlier evidence.

## Step 3: Known tool quirks

- `vidiq_outliers`: its topic field is `keyword`, and it does not filter reliably by topic. Its language list has no Chinese, so it cannot find HK/TW competitors. Do not use it for competitor claims; use `vidiq_youtube_search` and `vidiq_similar_videos`.
- `vidiq_youtube_search` sometimes errors on a malformed `publishedAt`. Retry with a slightly different query before deciding the tool is down.
- `vidiq_channel_analytics` authorisation error: the YouTube link needs re-authorising. Tell Jansen; do not work around it.
- Ambiguous keywords: disambiguate before citing. Bare "muse" (1.15M/mo) was mostly the band, so use "meta muse" / "muse ai".
- Narrow phrasing hides competitors. The Grok Bot pass searched only 教學-style Chinese titles, wrongly concluded "no Chinese coverage", and missed about 15 videos. Always try several phrasings and order="date".
- Keywords under 750/mo come back as "below threshold": unmeasured, not zero. New products often have no growth baseline; compare our own dated pulls instead.

## Step 4: Hard rules

- VidIQ is the only source for search volume, competition, title scores and competitor numbers. A web-search guess dressed up as a number is worse than no number. No stand-ins, no estimates, no guesses. Missing data stays visibly marked as missing.
- If some tools fail, warn first, name the missing part, and mark that section incomplete.
- This rule covers this skill's numbers only. It does not stop `yt-video-story-builder` from using web search to verify primary-source facts.
- Reused numbers keep their original date ("HK volume, VidIQ 9 Sep 2026"). If Jansen says to skip VidIQ, reuse the latest brief and say plainly that no new pull was made.
- Do not use VidIQ's generation, upload or voiceover tools in this skill unless Jansen asks.

## Step 5: Build the HTML brief and visual evidence gallery

Record the data faithfully here; the reading and the verdict belong to `jan-vidiq-analyzer`. A short "first read" line per section is fine. The HTML is the primary deliverable and must contain both machine-readable data and visual proof.

### Required HTML structure

1. **Header:** topic, channel, pull date, opening balance, closing balance, actual credits used and the question or angle.
2. **Answer or executive snapshot:** 2 to 4 sentences using only measured data.
3. **Visual evidence gallery:** place this near the top. Include every screenshot from the evidence manifest. Each figure needs a numbered caption with the tool, query, call date and whether it is native or rendered from exact connector output. Make images clickable to open at full resolution.
4. **Accessible data tables:** reproduce all numbers in HTML tables or definition lists. Screenshots are evidence, not a substitute for selectable text. Include every keyword row, every title tested, every quantified competitor, momentum values and the channel snapshot that appears in the call results.
5. **Gaps and caveats:** below-threshold terms, unavailable baselines, failed calls, missing screenshots, ambiguous keywords and anything not measured.
6. **Call manifest and credit reconciliation:** one row per attempted call with tool, inputs, status, screenshot number, stated cost if available and notes. Show the opening-to-closing balance calculation.
7. **Analysis insertion point:** finish the research content with an empty `<section id="analysis"></section>` so `jan-vidiq-analyzer` can append a dated analysis without disturbing the evidence.
8. **Source line:** `Data pulled via VidIQ on [date].`

For a Quick pull, keep sections 2 and 4 short, but do not omit sections 3, 5 or 6. For a Full brief, group the accessible tables by idea: keywords, title scores, competitors, momentum and channel snapshot.

### Screenshot completeness rules

- Screenshot every successful VidIQ data call used in the brief, including the opening and closing balance, keyword research calls, every title score, every competitor search, similar-video calls, channel analytics and video stats. Include `vidiq_user_channels` when it establishes channel identity or authorisation.
- The evidence gallery count must reconcile with the successful calls in the manifest. If a call returned several pages or boxes, capture enough images to show all data used.
- Do not claim that a rendered connector snapshot is a native VidIQ website or tool-card screenshot. Preserve the disclosure label on the image and in its caption.
- Do not beautify, round, omit or reorder values in a way that changes the meaning of the exact result. Use `N/A` for absent fields.
- Prefer a self-contained HTML file with the PNG screenshots embedded as data URIs. If that makes the file impractically large, save images in `VidIQ_<Topic>_<YYYY-MM-DD>_assets/` beside the HTML and use relative paths. Never depend on temporary files, remote image URLs or a cloud-only workspace.

### Verification before saving

1. Match the number of evidence figures against the successful VidIQ calls represented in the manifest.
2. Confirm every image loads at full resolution and every caption identifies its call.
3. Compare the HTML tables against the screenshots, including one keyword call, one title call and one competitor call at minimum. For a smaller run, compare every call.
4. Confirm the opening balance minus closing balance equals the recorded credits used, or explain why it does not.
5. Open the finished HTML in a browser and check desktop and narrow-width readability before handoff.

**Refresh:** read the existing brief first, then add a dated `<section>` headed `UPDATE [YYYY-MM-DD]: [what was rechecked]` with old vs new numbers side by side and what changed and why. Append the new screenshots and manifest rows. Keep the previous evidence intact.

## Step 6: Save and hand off (final task on every run)

1. Save the `.html` file in the local folder above with device_bash. Save any sidecar screenshot assets beside it in the named assets folder. Never save the final output anywhere else.
2. Next step: run `jan-vidiq-analyzer` on the new data (it appends a dated ANALYSIS section with the verdict), then `yt-video-story-builder`.
3. Last item on the task list, every time: give Jansen clickable links to the output HTML AND its containing folder on his local drive. If the HTML uses a sidecar assets folder, link that folder too. For example:
   - [VidIQ_Research_<Topic>_<date>.html](computer:///Users/tsanchulu/Documents/Cowork/01_Active%20Projects/Jansen%20YT%20Channel/VidIQ_Research_<Topic>_<date>.html)
   - [Jansen YT Channel folder](computer:///Users/tsanchulu/Documents/Cowork/01_Active%20Projects/Jansen%20YT%20Channel/)
