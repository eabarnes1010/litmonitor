# LitMonitor Earth

Fetches recent papers from earth and climate journals and arXiv, scores each one 1–10 against *your* research profile using Claude, and puts the good ones at the top.

Everything runs in your browser. Your API key and profile are stored locally and go nowhere except to the Anthropic API.

🔗 **[Open LitMonitor Earth](https://eabarnes1010.github.io/litmonitor/)**

Every control explains itself on hover, and there is a full **Documentation** page behind the book icon in the left rail. This page is the short version.

---

## Start here

On a first visit the app opens the API key section and the profile panel, and the steps below are clickable.

1. **API key.** Get one at [console.anthropic.com](https://console.anthropic.com) and paste it into **LLM API key** in the sidebar. Costs bill to your own key.
2. **Profile.** Open **Research profile** from the left rail and fill in at least *Prioritize* or *Transferable methods*. Specific beats broad: "AI emulators for climate model parameterizations" works, "machine learning" does not.
3. **Journals.** Open **Select journals** and pick your sources. All are on by default.
4. **Fetch.** Click **↓ Fetch & Score**.

Expect roughly **$0.10–0.12 per 100 papers**.

---

## The interface

**Left rail**, top to bottom: **Brief** and **Export**; at the foot, **Research profile**, the theme control and **Documentation**. Hover any of them for its name. The sidebar hides from a button on its own edge, not from the rail.

**Sidebar**: everything that changes the next run — fetch settings, journals, custom sources, API key — with **Cost estimate** and **Clear cache** at the bottom. Its top section does not scroll, so **Fetch & Score** and the status line stay in reach.

**Profile panel**: opens from the rail and narrows the feed. `Esc` closes it.

---

## Your profile

| Field | Effect |
| --- | --- |
| **Research context** | One sentence on who you are. Background for every score. 160 characters. |
| **Prioritize** | What you work on. Matches score **8–10**. |
| **Transferable methods** | Methods from other fields you would want in yours. Score **8–10** even with no earth-science application yet. Also feeds the arXiv.cs filter. |
| **Downgrade** | What you do not want. Scores **1–4**. |
| **Priority authors** | One name per line, matched on surname + first initial. Those papers skip the prefilter, score at least **8**, and are tagged on the card. |

Editing any field re-scores every paper on the next run — the cache is keyed to the profile as a whole.

---

## Fetch settings

| Setting | Default | Notes |
| --- | --- | --- |
| **Fetch days back** | 2 | A slider over 1–30 days with **Auto** at its left stop, or type any window up to 99 into the box. **Auto** uses the gap since your last successful run. The window applies to arXiv as well as to journals, so days missed by a failed run come back on the next one. |
| **Haiku prefilter** | On | A cheap Haiku pass drops the clear misses before Sonnet scores anything. The main cost saving. |
| **Drop if Haiku scores ≤** | 5 | Higher saves more and risks losing borderline papers. |
| **Abstract chars** | All | How much of each abstract is sent for scoring: 500, 1000, 1500, 2000, 2500 or **All**. Lower it if a run costs more than you expect. |

**■ Stop** appears during a run and halts it after the batch in flight. Papers already scored are kept.

---

## Journals and the arXiv.cs filter

Sources are tagged **api** (OpenAlex, read directly) or **relay** (arXiv, which a browser cannot read directly).

**cs.LG**, **cs.AI** and **stat.ML** carry hundreds of papers a day. The **arXiv.cs filter** keeps only those whose title or abstract mentions one of your terms *or* a method from your Transferable methods list. It runs in your browser, so rejected papers cost nothing. It ships empty — click **load suggested terms** for an earth-science list you can edit. No other feed is filtered.

**Custom journals** take one line each, `Name | S-ID or ISSN`, from [openalex.org](https://openalex.org). Use the `S…` source ID, not a `W…` work ID.

---

## Reading the feed

Above the cards: **Fetched**, **Shown**, **New**, **Score≥8**. Hover each for what it counts.

On each card the left column carries the score badge, journal and date:

| Badge | Score |
| --- | --- |
| Filled green | **8–10** |
| Amber outline | **6–7** |
| Grey outline | **1–5** |

A **ring around the badge** means the paper is new since your last run.

The **abstract** sits under the authors, four lines deep. It starts at the paper's finding rather than its opening background; a leading `…` marks where it skipped. **Show more**, or a click anywhere on the card, opens the whole thing. **Score reasoning** below it is Claude's one-sentence justification, and the best guide to tuning your profile.

| Action | Effect |
| --- | --- |
| **Copy citation** | Formatted citation; preprints are labelled and carry their URL. |
| **☆ Star** | Keeps the paper in the Starred view whatever its score. Survives **Clear cache**. |
| **+ More like this** | Records it as a *learned example*. |
| **Dismiss** | Hides it and keeps it out of future runs. |

The title is the link to the paper.

**Learned examples** go to Claude as calibration for what an 8–10 looks like in your field, which is often quicker than describing it in *Prioritize*. The list is at the foot of the profile panel: the newest 20 are sent by default (slider, 0–30), up to 100 are stored, and what the list shows is exactly what is sent. Adding or removing one re-scores on the next run.

---

## Filter, search, export

**All** / **New only** / **Score ≥ N** (default 6) / **★ Starred**, and a search box matching title and author.

The starred view ignores the score floor and the dismissed list; the floor is greyed out there.

**Export** opens a report of whatever is shown, downloadable as one self-contained file. **Brief** asks Sonnet for two or three bullets per paper — what is interesting, and why it matters to you — for up to 50 papers in a single call. Both act on the papers currently shown; hover them for the count.

---

## Cost

The **Cost estimate** line at the foot of the sidebar shows this run, this calendar month, and how many papers that covers. Token counts are measured — every response reports them — and multiplied by published list rates, so the estimate is in the price, not the usage.

To spend less, in order of effect: put terms in the arXiv.cs filter, leave the Haiku prefilter on, and re-run over overlapping windows — cached papers are free.

---

## Odds and ends

- **Clear cache** resets seen papers, dismissals, cached scores, and the record of which arXiv days have been fetched. Starred papers are kept.
- **New only** works across sessions; the history persists in your browser.
- arXiv is reached through free public relays. When they are down the app falls back to one that returns **only the ten newest** papers per feed and says so above the cards; those days are requested again on your next run.
- Messages above the cards can be dismissed with the **×** at their right. A run that still has something to report says so again.
- Papers found in more than one source are merged, preprint with published version, keeping whichever abstract exists.
- The app is one HTML file, so you can save a copy and keep it.

---

## License

[CC BY 4.0](LICENSE). Built by Elizabeth A. Barnes · 2026.
