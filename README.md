# Claude Skills by Jansen Lu

A collection of custom Claude (Cowork) skills for digital marketing and SEO — built for Hong Kong and Taiwan markets.

## How to Install a Skill

1. Download the `.skill` file from the folder you want
2. In the Claude desktop app (Cowork mode), go to **Settings → Plugins & Skills → Install from file**
3. Select the `.skill` file
4. The skill will appear automatically in your Claude session

---

## Skills

### 🔍 First Page Ranker

**What it does:** Full Google SERP analysis for any keyword. Give it a keyword and it will:
- Pull the top 10 ranking URLs
- Identify the dominant search intent
- Extract H1/H2 headings with frequency analysis
- Spot 6 content gaps your competitors are missing
- Generate a full SEO article outline (H1/H2/H3 + word counts + 30 semantic keywords)
- Write 8 FAQ schema Q&A pairs ready for structured data

**Languages:** Works in Traditional Chinese (HK/TW) and English — output language matches your keyword language.

**How to trigger:** Just say something like:
- `幫我用 First Page Ranker 分析「香港數碼營銷顧問」`
- `Run First Page Ranker for the keyword: Hong Kong social media expert`

📁 [`first-page-ranker/`](./first-page-ranker/)

---

### ✍️ Story First Writer

**What it does:** Writes FB posts, IG captions, blog articles, and eDM copy using the "Story First" formula — personal anecdote first, lesson second, pitch last. Uses the AIA structure (Anecdote → Insight → Application) with curiosity-gap hooks and short punchy paragraphs.

**Languages:** Defaults to Traditional Chinese (HK standard, no Cantonese) unless English is requested.

**How to trigger:**
- `幫我寫一篇關於...的 FB 貼文`
- `Write a Story First post about...`

📁 [`story-first-writer/`](./story-first-writer/)

---

## About

Built by [Jansen Lu](https://github.com/hkjansen) — digital & social media marketing consultant based in Hong Kong.

> These skills are designed for the Claude desktop app (Cowork mode). Requires Claude in Chrome for full SEO crawling functionality in First Page Ranker.

### 🎬 Jan VidIQ Researcher

**What it does:** Pulls measured VidIQ data for the @JansenLu YouTube channel, including search demand, title scores, competitor checks and dated HTML research briefs with visual evidence.

**How to trigger:** Say things like:
- `Research this YouTube idea with VidIQ`
- `How much do people search [topic]?`
- `Score these titles`

📁 [`jan-vidiq-researcher/`](./jan-vidiq-researcher/)


---

### 🌴 JansReel

**What it does:** Edits 9:16 travel reels and vertical shorts in a casual summer series style: relaxed pacing, a big type-only location opener, sunny sticker-style captions placed inside the IG Reels safe zone, and an ending info card with rating, transport and tips rows. No logo or local creative files needed — every graphic is drawn in the video.

**Languages:** Captions in Traditional Chinese (standard written Chinese, no Cantonese).

**How to trigger:**
- `/jansreel`
- `Make a summer travel reel from these clips`

📁 [`jansreel/`](./jansreel/)
