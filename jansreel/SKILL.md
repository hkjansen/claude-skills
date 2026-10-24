---
name: jansreel
description: JansReel — Jansen's casual summer version of the travel reel series style. Builds 9:16 travel reels and vertical shorts with relaxed pacing, playful sun-and-sea coloured sticker captions, a big type-only location opener, and an ending info card with rating/transport/advice rows. No logo or local creative files — every graphic is drawn in the composition. Use when the user asks for JansReel, a summer / care-free / holiday-feel travel reel, a casual-caption version of the travel reel style, or wants the series look without a logo.
---

# JansReel

## Overview

Create bright, easy-going 9:16 travel reels that feel like a summer holiday: natural footage, unhurried pacing, short playful captions, and a signature opener and ending card. The mood is "no plans, just go" — sunny, light, a little cheeky, still readable and save-worthy.

This is a variant of `travel-reel-series-style`. Two differences:

1. **No logo or local creative.** Do not look for, ask for, or apply a logo, series mark, sticker pack, overlay PNG, template, or any other creative from the local drive. All graphics (pills, stickers, doodles, cards) are built from type, CSS and inline SVG inside the composition. The only media used are the photos, videos and music the user supplies for the reel.
2. **Casual summer captions.** Replace the calm dark-strip captions with the sunny sticker system below.

## Production Workflow

1. Inspect every source image/video before editing. List unused assets and either include them or explain why they do not fit.
2. Build a 25-30 second reel when enough footage exists. Prefer using more of each original clip over artificial slow motion.
3. Use a strong hero photo or bright hero shot as the opener with a 2-3 second zoom-in.
4. Keep all text inside the IG Reels safe zone defined in `references/layout-and-timing.md` (fixed numbers — no safe-zone image needed).
5. Animate text with a soft bounce: pop in, gentle float, fade out. Bouncy, never jarring.
6. Render and extract proof frames for opener, at least one caption, and final info card. Iterate if text covers faces, is too small, or the colours clash with the footage.

## Visual System

- Canvas: `1080x1920`, 30fps, H.264, AAC audio.
- Mood: sunny, breezy, care-free, holiday, playful but tidy.
- Footage look: keep it natural. Optionally lift it slightly toward summer — brightness +3%, saturation +8-10%, a touch warmer. Never orange-wash skin or blow out skies.
- Typography: rounded Traditional Chinese sans. First choice is Jansen's `MYuenHK` (Xbold for titles and captions, Medium for card rows) from `~/Library/Fonts`. It is an old CFF font: subset it and convert CFF→TrueType woff2 before embedding, then confirm it loaded (see `references/layout-and-timing.md`). Fallback: `PingFang TC` Semibold.
- Motion: slow zooms, 0.35-0.5s crossfades, soft bounce on text.
- Avoid: dense paragraphs, tiny captions, more than one doodle per caption, emoji images, neon colours, text over faces, fast cuts under 2 seconds, dark heavy boxes.

### Summer Palette

| Token | Hex | Use |
|-------|-----|-----|
| Sunshine | `#FFD447` | tag stickers, save cue, stars |
| Coral | `#FF6F59` | tag stickers, keyword underline |
| Lagoon | `#17BEBB` | tag stickers, row labels |
| Sky | `#8FD8F8` | doodles, soft accents |
| Sand | `#FFF6E5` | caption pill and info card fill |
| Ink | `#12324A` | text on light fills (use instead of black) |
| White | `#FFFFFF` | text on footage, sticker outlines |

Rotate the tag colour (Coral → Lagoon → Sunshine) from caption to caption so the reel feels lively without looking random. Use at most two accent colours on screen at once.

## Opener

The opener is the series signature, built from type only:

- Background: the strongest personal/place photo, ideally with the traveler or destination clearly visible, zooming gently for 2-3 seconds.
- Overlay: no dim box. Use a soft warm gradient (Ink at 0-35% opacity) only behind the text area if the footage is too busy to read white type.
- Series label: a small Sunshine pill with Ink text, rotated about -4°, e.g. `Jan's Trip` or a short label the user gives.
- Location name: the largest text on screen — MYuenHK Xbold, white, with a thick Ink or Coral offset shadow so it reads like a sticker, tilted -2° to -3°.
- Doodle (optional, one only): a hand-drawn wavy line, small sun, or sparkle in inline SVG, drawn on with a stroke animation.
- Place the lockup over torso, sky, water, sand, wall, or another low-detail area — never across eyes or faces.

## Captions

Keep captions short and breezy. One thought at a time.

Sticker caption pattern:

- **Tag sticker**: rounded pill in the rotating accent colour, Ink or white text, 4px white outline, rotated -6° to -3°, overlapping the top-left corner of the main pill. Examples: `交通`, `秘境感`, `消暑`, `親子`, `散步`, `收藏`, `夏日`, `海邊`, `放空`.
- **Main pill**: Sand fill (about 94% opacity), Ink text, fully rounded corners (radius 32-40px), slight tilt between -2° and +2° (alternate per caption), flat offset shadow `0 6px 0 rgba(18,50,74,0.18)`.
- **Keyword accent (optional)**: a Coral hand-drawn underline under one keyword, drawn on after the pill lands.
- Duration: 3-4 seconds per caption.
- Animation: pop in (scale 0.85→1, y +24→0, alpha 0→1, about 0.45s, back-out ease), a gentle ±4px float while holding, fade and drift up about 12px on exit, finished before the next transition. Keep all motion seek-safe on the main timeline.

Caption writing principles:

- Write like a relaxed friend on holiday: light, warm, a little playful.
- Say what makes the place easy, memorable, seasonal, scenic, or worth saving.
- Prefer concrete access facts, sensory details (sea breeze, shade, cold drink), and simple recommendations.
- Avoid long explanations that require pausing the reel.
- Standard Written Chinese in Traditional characters only — never Cantonese. Scan every caption for 嘅、冇、俾、佢、哋、咁、喺、唔、睇、諗、咩、啲、嘢、呢 and rewrite before rendering.

## Ending Info Card

End with a collectible info card over a calm background photo/video. No logo.

- Save cue: a large Sunshine sticker reading `收藏` (optionally with a small SVG sparkle), tilted about -5°, above the card.
- Card: Sand fill, radius about 40px, flat offset shadow, slight -1° tilt.
- Rows: label in a small Lagoon or Coral pill, value in Ink. Typical labels: `建議`, `適合`, `交通`, `推薦`, `易去` — adapt to the destination.
- Stars: `★★★★☆` style in Sunshine with an Ink outline when ratings are requested.
- Rows slide in one after another (about 0.12s stagger). Keep transport text compact. The card stays on screen about 3.5-4.5 seconds.

## Reference Details

Read `references/layout-and-timing.md` when implementing or tuning the edit. It has the safe-zone numbers, coordinates, timing, font conversion steps and QA checks for JansReel.
