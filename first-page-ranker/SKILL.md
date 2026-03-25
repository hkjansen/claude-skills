---
name: first-page-ranker
description: >
  Full Google SERP SEO analysis for any keyword. Use this skill whenever the user wants to:
  rank on Google, analyse competitor pages, find content gaps, get an SEO article outline,
  research what's already ranking for a keyword, build an FAQ schema, or understand search
  intent for any topic. Trigger on phrases like "first page ranker", "SEO analysis for [keyword]",
  "what's ranking for [keyword]", "analyse Google results for [keyword]", "help me rank for
  [keyword]", "content strategy for [keyword]", "competitor content analysis", or any time the
  user provides a search query and wants to know what the top results cover and how to outrank them.
  Works with any language — Chinese, English, or mixed — and any market.
---

# First Page Ranker

You are running a full Google SERP SEO analysis. Your job is to reverse-engineer the top 10 ranking pages for a user-supplied keyword, then turn those findings into a data-backed content strategy.

## Step 0 — Get the keyword

If the user has not provided a keyword in their message, ask exactly this before doing anything else:

> **請輸入您想分析的關鍵字：**（中英文皆可）
> *What keyword would you like to analyse? (Chinese or English both fine)*

Wait for the keyword. Do not proceed until you have it.

## Step 1 — Search Google for 10 URLs

Use **WebSearch** with the exact keyword the user supplied (do not translate or modify it).

Collect the first 10 organic result URLs from the search results. Ignore ads, featured snippets, and "People also ask" boxes — only organic blue-link results.

Present a clean numbered list of the 10 URLs to the user before continuing.

> If fewer than 10 organic URLs are returned, use what you have and note the shortfall.

## Step 2 — Crawl each URL

For each URL, try to fetch the page content using the following priority order:

1. **WebFetch** — try first (fast). If it returns an `EGRESS_BLOCKED` error or times out, fall back to option 2.
2. **Claude in Chrome** — navigate with `mcp__Claude_in_Chrome__navigate` then extract content with `mcp__Claude_in_Chrome__get_page_text`. If the page text is empty or too large, use `mcp__Claude_in_Chrome__read_page` with `depth: 3` to extract the heading structure.

For **each page** you successfully crawl, extract and record:
- **Page title** (the `<title>` tag or the tab title)
- **H1 headings** (there is usually one — note if there are multiple or none)
- **H2 headings** (all of them, in order)
- **Main content themes** (2–3 sentence summary of what the page is actually about)

If a page is completely inaccessible (blocked, error, CAPTCHA, redirect loop), mark it as `[inaccessible]` and move on — do not retry more than once.

Use a **single Chrome browser tab** across all URL visits (reuse the same tabId). Get the tab context once at the start with `mcp__Claude_in_Chrome__tabs_context_mcp` (set `createIfEmpty: true`) and reuse the tabId throughout.

## Step 3 — Compile the full analysis

Once all (or most) pages have been crawled, produce the following five deliverables **in order**. Present them all in one coherent response.

---

### Deliverable 1 — Clean URL List

A numbered list of all 10 URLs, noting any that were `[inaccessible]`.

---

### Deliverable 2 — Dominant Search Intent

Write **exactly one sentence** that describes what the person searching this keyword is trying to accomplish. Be specific about:
- Are they looking to **hire** someone / buy something? (transactional)
- Are they looking for **information** or data? (informational)
- Are they trying to **find** a specific site or brand? (navigational)
- Are they trying to **compare options** before deciding? (commercial investigation)

Example: *"Searchers are businesses in Hong Kong looking to hire a social media agency to manage their Facebook and Instagram accounts and run paid ads."*

---

### Deliverable 3 — H1 & H2 Heading Frequency Table

Two tables:

**H1 Headings** — list every unique H1 found across all pages, with the source URL.

**H2 Headings by Theme** — group all H2s into thematic clusters. For each cluster, provide:
- The theme name
- Example headings that fall into this cluster
- Frequency count (how many pages use this theme)

Sort by frequency, highest first.

---

### Deliverable 4 — Six Content Gaps

Identify **exactly six** subtopics or angles that are:
- Relevant to the keyword's audience, AND
- Missing or barely covered (≤1 page touches on it) across the 10 results

For each gap:
1. Give it a clear name
2. Explain in 2–3 sentences why it matters to the audience and why competitors are missing it
3. Rate its opportunity level: 🔥 High / ⚡ Medium / 💡 Niche

---

### Deliverable 5 — SEO Article Outline

Produce a full article outline optimised to rank for the user's keyword.

**Structure:**
- **Suggested H1** — include the keyword naturally; make it compelling
- **Introduction** (~200 words) — what it covers, why it matters
- **H2 sections** — 6 to 8 sections; each H2 must:
  - Include 2–4 H3 sub-sections
  - Have an estimated word count
- **FAQ section** — based on Deliverable 6 below
- **Conclusion + CTA** (~150 words)
- **Total estimated word count**

After the outline, list **30 semantically related keywords** to weave into the article. Group them into: Core Variants / Platform/Tool-Specific / Intent-Specific / Audience-Specific.

---

### Deliverable 6 — 8 FAQ Schema Q&A Pairs

Write **exactly 8** question-and-answer pairs suitable for `FAQPage` JSON-LD structured data. Each answer should be 3–6 sentences, factual, and directly useful to someone who searched the keyword. Cover a mix of:
- Pricing / cost questions
- How-to / process questions
- Comparison questions (e.g., option A vs option B)
- Common mistakes or "watch out for" questions

---

## Step 4 — Save the report

Save the complete analysis as a Markdown file at:
```
/sessions/[session-id]/mnt/Cowork/[keyword]_SEO_Analysis.md
```

Replace `[session-id]` with the actual session directory and `[keyword]` with a sanitised version of the keyword (replace spaces and special characters with underscores, keep Chinese characters as-is).

After saving, provide a `computer://` download link so the user can open the file directly.

---

## Quality standards

- **Language matching**: If the keyword is in Traditional Chinese, write the analysis in Traditional Chinese (using Hong Kong standard, not Cantonese slang). If the keyword is in English, write in English. If mixed, follow the keyword's dominant language.
- **Specificity over generality**: Every insight should be grounded in what you actually found on the crawled pages — not generic SEO advice.
- **Honest about gaps**: If fewer than 6 pages were crawled successfully, note it clearly and flag that the analysis is partial.
- **No padding**: The FAQ answers, content gaps, and outline sections should contain real substance. Avoid filler phrases like "this is important because it helps your business grow."

---

## Error handling

| Situation | What to do |
|---|---|
| All WebFetch calls blocked | Use Chrome for all URLs |
| Chrome tab not available | Use WebSearch snippets as a fallback and note the limitation |
| Fewer than 6 URLs accessible | Proceed, note limitation in Deliverable 1, complete analysis with available data |
| Keyword is ambiguous (e.g., one word) | Ask one clarifying question: "Are you targeting [market A] or [market B]?" |
| Very long pages causing Chrome timeout | Use `read_page` with `depth: 2` to get headings only |
