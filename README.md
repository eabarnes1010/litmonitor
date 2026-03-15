# LitMonitor Earth

A personal literature monitoring tool for earth and climate scientists. It fetches recent papers from a curated set of journals and arXiv feeds, then uses Claude (Anthropic's AI) to score each paper 1–10 for relevance to your specific research profile. High-scoring papers surface at the top; low-scoring ones stay out of the way.

**Everything runs in your browser.** No server, no data stored externally — your Claude API key and profile are saved only in your browser's local storage.

🔗 **[Open LitMonitor Earth](https://eabarnes1010.github.io/litmonitor/)**

---

## Contents

1. [Getting Started](#2-getting-started)
2. [Fetch Settings](#3-fetch-settings)
3. [Research Profile](#4-research-profile)
4. [How Scoring Works](#5-how-scoring-works)
5. [Journals](#6-journals)
6. [Fetch & Score Pipeline](#7-fetch--score)
7. [Reading Results](#8-reading-results)
8. [Filter & Search](#9-filter--search)
9. [Export](#10-export)
10. [Research Brief](#11--research-brief)
11. [Tips & Best Practices](#12-tips--best-practices)

---

## 2. Getting Started

### Step 1 — Get an Anthropic API key

LitMonitor uses the Claude API to score papers. You need an Anthropic API key from **console.anthropic.com**. Paste it into the **Anthropic API Key** field at the top of the sidebar. The key is stored in your browser's local storage and is never sent anywhere except directly to the Anthropic API.

> **Each user needs their own API key.** Costs are billed to the account associated with the key used, so every team member should use their own.

### Step 2 — Fill in your Research Profile

Click **Profile ▾** in the sidebar to expand the profile section. Fill in at least the *Prioritize* or *Transferable Methods* fields. The more specific your profile, the more useful the scores will be. See [Section 4](#4-research-profile) for details on each field.

### Step 3 — Select journals

Scroll down to the **Journals** section and check the sources you want to monitor. All journals are checked by default. Deselecting high-volume arXiv feeds (cs.LG, stat.ML) significantly reduces the number of papers and API cost per run.

### Step 4 — Fetch & Score

Click the **↓ Fetch & Score** button. LitMonitor will pull recent papers from each selected source and then ask Claude to score them for you. Results appear as scored cards in the main feed, sorted highest score first.

---

## 3. Fetch Settings

### Fetch days back

How many calendar days back to look when fetching papers. A value of **2** fetches papers published in the last two days. Increase to catch up after a weekend or holiday. Note that arXiv RSS feeds return a fixed number of recent items regardless of date, so very large values mainly affect OpenAlex journal fetches.

### ⚡ Haiku Prefilter (button + threshold)

The **⚡ Haiku Prefilter** toggle in the header activates a two-pass scoring pipeline. It is **on by default** and is the primary cost-saving mechanism.

- **Pass 1 (Haiku):** All uncached papers are scored quickly and cheaply by Claude Haiku. Any paper scoring at or below the *Haiku drop if ≤* threshold is discarded without being sent to Sonnet.
- **Pass 2 (Sonnet):** Only the survivors are scored by Claude Sonnet, which provides the final scores you see in the feed.

The **Haiku drop if ≤** setting (default: 3) controls the cutoff. Raise it to filter more aggressively (saves more cost but risks dropping borderline papers); lower it to be more conservative.

### Abstract chars (scoring)

Controls how many characters of each paper's abstract are sent to Haiku and Sonnet during scoring. Options: **300**, **500** (default), **800**, **1000**, **1500**, or **∞ all**.

Most of the relevance signal is in the first few hundred characters of an abstract, so truncating saves input tokens without meaningfully affecting score quality. This setting only applies to scoring — the full abstract is always used for ✦ Research Brief generation and for the inline abstract toggle in the feed.

### ■ Stop button

While a Fetch & Score run is in progress, a red **■ Stop** button appears in the header. Clicking it sets an abort flag that halts the run after the current in-flight scoring batch completes. Papers scored up to that point are displayed normally. The status line will show *· stopped early* to indicate the run was not completed.

### Custom journals

A free-text box at the bottom of the journal list lets you add any OpenAlex-indexed journal not in the built-in list. Enter **one journal per line** using the format:

```
Display Name | OpenAlex-ID-or-ISSN
```

The identifier after the `|` can be either an OpenAlex source ID (e.g. `S4210178125`) or an ISSN (e.g. `2398-9629`). To find an ID, search for the journal at **openalex.org** and copy the `S…` number from the URL. Lines starting with `#` are treated as comments and ignored. Custom journals are always fetched when present — there is no individual checkbox for them. The journal count updates to show e.g. *18/24 +2 custom*.

Example:
```
Nature Sustainability | S4210178125
# My other journal
Journal of Geophysical Research: Oceans | 2169-9291
```

---

## 4. Research Profile

The profile tells Claude who you are and what to prioritize. It is assembled into a prompt sent with every scoring request. All fields are optional but the more you fill in, the more accurate the scores.

### Research Context (max 160 chars)

A one-sentence description of who you are and what you work on. This is used as background context for all scoring decisions. Keep it concise — it has a 160-character limit. Example: *"I am a climate scientist with extensive AI knowledge working at the intersection of machine learning and climate/earth science."*

### Prioritize `[8–10]`

Topics, application areas, or contributions you want to score high. One item per line. Claude uses semantic judgment — you do not need exact keyword matches. Papers that directly advance or apply something on this list will score 8–10.

Be specific about the combination of topic and approach that matters to you. Broad topics like "machine learning" or "climate" will match too many papers; something like "AI emulators for climate model parameterizations" is far more useful.

### Transferable Methods `[8–10]`

Methods or techniques from *outside* your field that you would find useful if they were applied to your domain. Papers advancing these techniques on spatial, temporal, or spatiotemporal data — even in a different domain — will score 8–10 if the method would transfer naturally to earth/climate science.

This is distinct from Prioritize: use Prioritize for what you directly work on; use Transferable Methods for techniques you are watching from adjacent fields (e.g., uncertainty quantification, representation learning, OOD detection).

### Downgrade `[1–4]`

Topics or paper types you are *not* interested in. Papers matching items on this list will score 1–4 unless they also strongly match a Prioritize or Transferable Methods item. Use this to suppress high-volume topics that appear in your journals but aren't relevant to your work.

Examples: *"Purely observational studies with no ML component"*, *"Standard numerical weather prediction without novel AI"*, *"Regional case studies without generalizable methods"*.

### Priority Authors `[8–10]`

Names of researchers whose papers you always want to see, regardless of topic. One name per line. Papers with any matching author — including co-authors — will score 8–10. Claude uses fuzzy matching on last name and first initial, so minor name variations are handled gracefully.

---

## 5. How Scoring Works

Papers are scored 1–10 by Claude Sonnet (or Haiku in the prefilter pass) using your assembled profile. Papers are sent in batches of 20. Each paper's title, authors, journal, and abstract are provided. Claude returns a score and a one-sentence reason for each paper.

**Score caching:** Once a paper has been scored by Sonnet, its score and reason are saved in your browser's local storage. On subsequent runs, any paper whose ID already appears in the cache is displayed immediately without making an API call — so re-running across overlapping date windows costs nothing for papers you have already scored. The cache is cleared when you click **Clear cache**.

### Score ranges

| Range | Meaning |
|-------|---------|
| 8–10 | Directly on-topic — strong match to Prioritize, Transferable Methods, or a Priority Author |
| 5–7 | Peripherally relevant — related but not a strong match; might inform your work |
| 1–4 | Not relevant — matches Downgrade topics or has no meaningful connection to your profile |

The default **Score ≥ 6** filter means you see scores 6–10: everything mid-range and above.

### Scoring models

Final scores use **Claude Sonnet** (claude-sonnet-4-5). The Haiku prefilter uses **Claude Haiku** (claude-haiku-4-5). Models are pinned intentionally to keep costs predictable.

> **Tip:** If papers you expect to score high are coming in at 5–7, your Prioritize list may be too vague. Add more specific entries that match the exact combination of topic and method you care about.

---

## 6. Journals

Each source in the journal list has a small badge showing how it is fetched:

- **api** — fetched directly from the OpenAlex API (open scholarly database). Reliable, no third-party proxy needed. Abstracts are included when available.
- **rss** — fetched via an arXiv RSS feed through a CORS proxy. arXiv feeds are category-exact (e.g. cs.LG returns every paper cross-listed under cs.LG). These feeds can be very large; deselecting them reduces both run time and API cost.

Use the **All** and **None** buttons to quickly select or deselect all sources. Your selection is saved automatically.

### Current journal list

AI for Earth Systems (AMS) · arXiv cs.LG · arXiv physics.ao-ph · arXiv stat.ML · BAMS · Earth's Future (AGU) · Environmental Data Science · Environmental Research: Climate · Environmental Research Letters · Geoscientific Model Development · GRL (AGU) · JAMES (AGU) · JGR-Atmospheres (AGU) · Machine Learning: Earth · Monthly Weather Review (AMS) · Nature · Nature Climate Change · Nature Communications · npj Climate and Atmospheric Science · Science (AAAS) · Science Advances (AAAS) · Scientific Reports (Nature)

### Adding custom journals

Any OpenAlex-indexed journal can be added using the **Custom journals** box at the bottom of the journal list. See the [Custom journals](#custom-journals) section under Fetch Settings for full details on the format.

---

## 7. Fetch & Score

Clicking **↓ Fetch & Score** runs the full pipeline:

1. **Fetch** — Papers are pulled from each selected journal or arXiv feed using the *Fetch days back* window. A progress bar shows each source as it loads. Sources that fail (e.g. proxy timeouts) are reported as warnings; papers from other sources still proceed. A hard cap of **1,000 papers** is applied after fetching to prevent runaway runs.

2. **Deduplicate** — Papers appearing in multiple sources are deduplicated by DOI or a title+date fingerprint.

3. **Abstract enrichment** — Papers without an abstract are looked up against the Semantic Scholar batch API using their DOI. Semantic Scholar has broader publisher agreements than OpenAlex, so many abstracts that OpenAlex omits (e.g. from Nature Group or AAAS journals) can be recovered here. Abstracts filled in this step are used for both scoring and brief generation.

4. **Score cache split** — Papers whose IDs appear in the local score cache are set aside and displayed immediately with their cached score and reason — no API call needed. Only uncached papers proceed to the next steps.

5. **Large-batch warning** — If more than 300 uncached papers remain, a confirmation dialog shows the estimated run time and gives you the option to cancel and narrow your selection.

6. **Score** — Uncached papers are sent to Claude in batches of 20. If Haiku Prefilter is active, a fast Haiku pass runs first and drops low-scoring papers; survivors are then scored by Sonnet. New Sonnet scores are saved to the local score cache for future runs.

7. **Results** — All papers (cached + newly scored) are merged and displayed sorted by score, highest first. At any point during steps 1–6 you can click the red **■ Stop** button in the header to halt the run early; papers scored up to that point are shown normally.

> **New vs. seen:** LitMonitor tracks every paper ID it has ever scored. Papers not seen in a previous run are tagged **new**. This tag persists across browser sessions until you clear the cache.

---

## 8. Reading Results

### Stats bar

Above the paper cards, a stats bar shows: **Fetched** (total papers scored this run), **Shown** (papers passing the current min-score and filter), **New** (papers not seen in any previous run), and **Score≥8** (high-relevance count).

### Score badge

Each card shows a color-coded score badge: green (8–10) = highly relevant, amber (5–7) = peripheral, red (1–4) = not relevant.

### Relevance reason

Below the title and authors, a one-sentence italic reason explains why Claude assigned the score it did. This is useful for calibrating your profile over time.

### Card actions

**Open →** opens the paper's DOI link in a new tab. **Abstract** expands the full abstract inline. **Dismiss** removes the card from the current view and records the paper as dismissed, so it won't reappear in future runs.

---

## 9. Filter & Search

**All** shows every paper at or above the min score. **New only** restricts the feed to papers not seen in any previous run.

The **Score ≥ N** dropdown sets the minimum score threshold. The default is **Score ≥ 6**. Raise it to see only highly relevant papers; lower it to browse more of the feed.

The search box filters by title and author name simultaneously. Results update as you type.

All filter changes are instant — no re-fetch required. The Export and Brief functions always operate on the currently visible (filtered) set of papers.

---

## 10. Export

The **↓ Export** button opens a new browser tab containing a styled HTML report of all currently visible papers (respecting min score, filter, and search). The exported page includes title, journal, authors, date, Claude's relevance reason, and a collapsible abstract for each paper. A *Download* button on the export page saves the report as a self-contained HTML file you can archive or share.

---

## 11. ✦ Research Brief

The **✦ Brief** button generates a personalized narrative brief for each visible paper — up to 20 at a time. For each paper with an abstract, Claude produces 2–3 bullet points:

- **Bullet 1** — What is interesting, innovative, or important about this specific paper: a concrete finding, method advance, or result that sets it apart from generic topic descriptions.
- **Bullet 2 (and optionally 3)** — Why it matters to *you specifically*, based on your Research Profile, and any notable caveats or connections to your work.

Papers without an abstract are included in the brief output but show *No abstract available — brief not generated.* in place of bullets. No API call is made for those papers.

The brief opens in a new tab and can be downloaded as a self-contained HTML file.

> **Cost note:** Brief makes a single API call to Sonnet with up to 20 papers included (only those with abstracts are sent). If your visible list exceeds 20 papers, a notice is shown and the remaining papers are omitted. Raise your min score to reduce the list.

---

## 12. Tips & Best Practices

### Controlling cost

The main cost driver is the number of papers scored. At Sonnet pricing, expect roughly **$0.10–0.12 per 100 papers**. A typical run across a full journal list (150–200 papers) costs $0.15–0.25. arXiv cs.LG and stat.ML can each add 100–300 papers per day on their own — deselecting them is the single biggest cost reduction. The ⚡ Haiku Prefilter helps further when you have many low-relevance papers to discard cheaply before Sonnet scoring. Score caching means re-running over an overlapping date window is nearly free — only papers you have not scored before incur an API call. Costs are billed directly to your Anthropic account.

### Tuning your profile

Run the tool a few times and look at papers scoring 6–7: if they feel more relevant than their score suggests, add a more specific Prioritize entry. If papers you don't care about keep appearing, add them to Downgrade. The relevance reason on each card is the best diagnostic — it tells you exactly which part of your profile Claude matched (or didn't).

### Using New only

After your first run, use the **New only** filter on subsequent days to focus on papers you haven't seen before. The seen-papers cache persists across sessions so this works even if you close and reopen the app.

### Clearing the cache

The **Clear cache** button in the sidebar footer resets the seen-papers history, the dismissed-papers list, *and* the score cache. All papers will be marked as new and re-scored from scratch on the next run. Use this if you want a completely fresh start, if you have significantly revised your Research Profile and want fresh Sonnet scores, or if the cache has grown stale.

### Light / Dark theme

The **☀ Light** / **🌙 Dark** button in the sidebar footer toggles the color scheme. The preference is saved in local storage and persists across sessions. Export and Brief pages mirror the theme at the moment they are generated.

---

## License

This work is licensed under [CC BY 4.0](LICENSE). Built by Elizabeth A. Barnes with Claude (Anthropic) · 2026.
