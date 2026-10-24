# JansReel Layout And Timing Reference

## Example Timeline

For a 25-30 second reel with multiple videos and photos:

- Opener hero image/video: 3.0-3.5s
- Main clips: 3.5-5.5s each, depending on available footage
- Ending photo/info card: 4.0-5.0s
- Crossfades: 0.35-0.5s

Text should be readable for at least 3 seconds after its entrance animation.

## IG Reels Safe Zone (fixed)

For `1080x1920`, from the IG Reels safe-zone guide:

- Top margin: 14% → keep text below `y = 270`
- Left and right margins: 6% → keep text between `x = 65` and `x = 1015`
- Bottom: 35% → keep text above `y = 1248`
- Right-side button column: the bottom 40% (`y > 1152`) is covered by like/comment/share buttons — below that line keep text left of `x ≈ 870`

Main captions and the info card live in the band `y = 270..1248`.

## Opener Coordinate Pattern

For `1080x1920`:

- Series label pill: font `34..40`, padding `12px 28px`, placed just above the location name, left-aligned with it or centered.
- Location name: font `120..160` (shrink for long names), max width about `900`, white with an offset shadow around `8px 8px 0` in Ink or Coral.
- Lockup center: around `x = 540`, `y = 900..1150`; move lower or higher to keep faces clear.
- Optional warm gradient: Ink, 0% at the top of the lockup area to about 35% behind the text.
- Doodle: stroke width `6..8`, Sky or Sunshine, drawn on over about 0.6s after the title lands.

## Caption Coordinate Pattern

Use large text. For `1080x1920`:

- Left margin around `80..100`
- Caption y around `650..850`, adjusted for safe zone and subject placement
- Tag sticker font around `34..38`, padding `8px 22px`, offset about `-18px` above and `-10px` left of the main pill
- Main pill font around `52..58`, padding `22px 40px`
- Keep single-line main captions when possible; two short lines maximum.

Animate overlay text with:

- Pop in: 0.4-0.5s, scale `0.85 → 1`, y `+24 → 0`, alpha `0 → 1`, back-out ease (e.g. GSAP `back.out(1.6)`)
- Tag sticker lands about 0.1s after the main pill
- Hold: ±4px vertical float, sine ease, one slow cycle
- Exit: 0.3-0.4s, alpha `1 → 0`, y `0 → -12`

## Ending Card Coordinate Pattern

For `1080x1920`:

- Optional light Ink overlay (15-25%) only if the background is busy.
- `收藏` save sticker: font `72..88`, around `y = 330..420`.
- Sand info card: width about `880`, inside the safe band, radius about `40`.
- Row label pill font around `32..36`; value font around `36..40`.
- Long transport/access rows may need value font around `30..34`.
- Row stagger: about 0.12s each.

## Font Setup (MYuenHK)

MYuenHK-Xbold / Medium / Light are in `~/Library/Fonts` as old CFF OTFs. Raw OTFs exceed the 2 MB inline limit, and a subset CFF woff2 still fails in Chrome. Working recipe:

1. Subset to the characters actually used (`pyftsubset`).
2. Convert CFF outlines to TrueType (`glyf`) with fontTools `Cu2QuPen` + `TTGlyphPen`.
3. Save as woff2 (about 85 KB each). Run with `uvx --from 'fonttools[woff]'`.
4. Verify in the browser that `document.fonts` shows the face as `loaded` before rendering.

## QA Checklist

Before delivery:

- Verify final duration is within user target.
- Extract proof frames for opener, one caption, and final card.
- Confirm no logo, series mark, or local creative file was used.
- Check no text covers face unless intentionally over torso/background.
- Check all text stays inside the safe-zone numbers above.
- Check captions can be read on a phone screenshot and the accent colours don't vanish into the footage (e.g. Sunshine on sand, Lagoon on sea) — swap the tag colour if they do.
- Check captions are Standard Written Chinese with no Cantonese characters.
- Check the rounded font actually loaded (not a fallback).
- Check the final file is vertical `1080x1920`, H.264, AAC.
