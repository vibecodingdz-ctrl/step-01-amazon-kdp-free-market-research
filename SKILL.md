---
name: step-01-amazon-kdp-free-market-research
description: Conduct free, evidence-based Amazon KDP niche, demand, competition, and reader-pain-point research using public Amazon, Reddit, YouTube, Google, Trends, and Etsy pages. Use when screening KDP keywords or validating book-market opportunities without paid tools.
---

# Amazon KDP Free Market Research

Act as a market-research agent. Collect verified, zero-cost evidence about Amazon KDP demand, competition, and reader needs using a strict two-stage process: complete the Cheap Screen first, then run the Deep Screen only for surviving keywords.

## Environment and output

- For browser or workspace agents, browse public URLs sequentially, one page at a time. Save each research run as a new dated file under `03_Market_Research/` or `06_Keyword_Research/`; never overwrite an existing research file.
- For search-only agents, return structured tables with verified URLs and capture dates.
- Record the Amazon department used. Default to Amazon US Books/Paperback with `i=stripbooks`; use Kindle (`i=digital-text`) or Audible only when explicitly requested.
- Use only free public pages. Do not log in, purchase anything, or use paid tools unless the user gives explicit written approval.

## Anti-block protocol

1. Open only one Amazon page at a time. Wait 4–6 seconds between page loads.
2. A valid search page must display book listings. A blank page, CAPTCHA, or robot check is a block, never zero results or low competition.
3. If blocked, do not solve or bypass the CAPTCHA. Wait 10 seconds and retry, then 20 seconds and retry, then 30 seconds and retry. After three failed attempts, stop and log `BLOCKED <url> <YYYY-MM-DD>`.
4. A product URL returning Amazon's Page Not Found (404) is an invalid ASIN, not a block.

## Public research routes

| Source | Route or method | Extract |
|---|---|---|
| Amazon Books Search | `https://www.amazon.com/s?k=<term>&i=stripbooks` | Total result count and page-1 organic listings |
| Amazon Best Sellers | `https://www.amazon.com/gp/bestsellers/books/<node>` | Roughly top 30 books, rank badges, titles, ASINs, and subcategory links |
| Amazon New Releases | `https://www.amazon.com/gp/new-releases/books/<node>` | Newly launched books gaining traction |
| Amazon Product Page | `https://www.amazon.com/dp/<ASIN>` | Overall BSR, subcategory BSRs, exact global ratings count, and Look Inside availability |
| Amazon and Google autocomplete | Enter seed phrase plus `a`–`z` in the relevant search bar | Buyer keyword variations |
| Reddit | Search `<topic> site:reddit.com` | Reader pain points, complaints, and exact phrasing; record URL and date |
| YouTube | Search `<topic>` | Video titles and view counts; label counts `[ASSUMPTION]` because they show creator interest, not book-buyer demand |
| Google Trends and Etsy | Public pages when reachable | Trend direction and product angles; if unreachable, log `could not open <url> <date>` |

## Stage A: Cheap Screen

For each candidate keyword:

1. Open `https://www.amazon.com/s?k=<keyword>&i=stripbooks`.
2. Read the result counter at the top of page 1, such as `1-16 of 64 results`. Target strictly fewer than 1,000 total results. If the counter is missing, record `not captured`; never guess.
3. Scan page-1 organic book listings. Exclude every `Sponsored` card.
4. Count off-topic organic listings and listings with fewer than 200 reviews. Record:

   `page1_organic=<N> | weak(<200_rev_or_off_topic)=<M> | total_results=<T> [VERIFIED: <url>, <date>]`

5. Stop if the keyword fails the user's screening rule, such as total results `>= 1000` or too many strong competitors. Do not open product pages for a disqualified keyword.

## Stage B: Deep Screen

Run this only for keywords that pass Stage A. Open each page-1 organic product URL and read the Product details section. Extract:

- the exact `<N> global ratings` count;
- the overall Best Sellers Rank, formatted `#X in Books`;
- every visible subcategory rank, formatted `#Y in <Sub-category>`;
- whether Look Inside is available, when relevant.

Before trusting a batch, manually spot-check one ranked book and one book with no rank shown.

## Measurement and data integrity

- Never count an unmeasured listing as weak. If 6 of 16 organic listings were read, the other 10 are `UNKNOWN`, not weak.
- For partial coverage, log: `INCOMPLETE: <read> of <total> page-1 results measured; <weak> weak so far`. Never issue PASS or FAIL for an incomplete screen.
- Distinguish `no rank shown` from `UNKNOWN`. Use `no rank shown` only when Product details fully loaded and Amazon displayed no BSR line. Use `UNKNOWN` when the page did not fully load or was not read; reload and verify.
- Every Deep Screen must report exact coverage: `<n> ranked / <n> no-rank shown / <n> unread (UNKNOWN)`.
- If earlier research used unmeasured assumptions, identify which prior verdicts may be contaminated.
- Include capture dates, source URLs, department, observed values, and any block or missing-data notes in the final report.

## Useful category node IDs

Index root: `https://www.amazon.com/gp/bestsellers/books/`. Low-content trackers and planners may rank under Office Products as well as Books, so check both where applicable.

| Node ID | Category path |
|---|---|
| `4402` | Humor & Entertainment > Puzzles & Games |
| `4416` | Puzzles & Games > Crosswords |
| `4436` | Puzzles & Games > Logic & Brain Teasers |
| `4742` | Self-Help > Journal Writing |
| `3568219011` | Self-Help > Anxieties & Phobias |
| `69980` | Religion & Spirituality > Devotionals |
| `12809` | Religion & Spirituality > Worship & Devotion |
| `11357541011` | Crafts, Hobbies & Home > Coloring Books for Grown-Ups |
| `3373` | Children's Books > Activity Books |
| `7009080011` | Children's Books > Early Learning |
| `266117` | Business & Money > Accounting |
| `2717` | Business & Money > Personal Finance |
| `4656` | Health, Fitness & Dieting > Diseases & Physical Ailments |
## Workflow position

**Step 01** — this card is part of the numbered KDP workflow. Use the previous step outputs before starting it and pass its required outputs to the next step.

