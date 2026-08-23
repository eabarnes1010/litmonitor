# LitMonitor Earth

Fetches recent papers from earth and climate journals and arXiv, scores each one 1–10 against *your* research profile using Claude, and puts the good ones at the top.

Everything runs in your browser. Your API key and profile are stored locally and go nowhere except to the Anthropic API.

🔗 **[Open LitMonitor Earth](https://eabarnes1010.github.io/litmonitor/)**

Every control explains itself on hover, and the app has a **Documentation** page behind the book icon. This page is the short version.

---

## Start here

1. **API key.** Get one at [console.anthropic.com](https://console.anthropic.com), then paste it into **LLM API Key** in the sidebar. Everyone needs their own — costs bill to the key's account.
2. **Profile.** Click the person icon in the left rail and fill in at least *Prioritize* or *Transferable Methods*. Specific beats broad: "AI emulators for climate model parameterizations" works, "machine learning" does not.
3. **Journals.** Open **Select Journals** and pick your sources. All are on by default.
4. **Fetch.** Click **↓ Fetch & Score**. Cards appear sorted by score.

Expect roughly **$0.10–0.12 per 100 papers**.

---

## The interface

The **rail** on the far left: hide the sidebar, **Export**, **Brief**, then at the foot **Research profile**, **Documentation**, and light/dark theme.

The **sidebar** holds everything that changes the next run — fetch settings, journals, custom sources, your API key — with **Spend** and **Clear cache** at the bottom. Its top section never scrolls, so **Fetch & Score** and the status line are always in reach.

The **profile panel** opens over the feed from the rail, since the profile is set once and then left alone.

---

## Your profile

| Field | Effect |
| --- | --- |
| **Research Context** | One sentence on who you are. Background for every score. 160 characters. |
| **Prioritize** | What you work on. Matches score **8–10**. |
| **Transferable Methods** | Methods from other fields you would want in yours. Score **8–10** even with no earth-science application yet. Also feeds the arXiv.cs filter. |
| **Downgrade** | What you do not want. Scores **1–4**. |
| **Priority Authors** | One name per line. Matched in your browser on surname + first initial; those papers skip the prefilter, score at least **8**, and are tagged on the card. |

Changing any of these re-scores the affected papers on the next run.

The panel also lists your **Learned examples**, but that list is not typed — see [Reading the feed](#reading-the-feed).

---

## Fetch settings

| Setting | Default | Notes |
| --- | --- | --- |
| **Fetch days back** | 2 | **Auto** uses the gap since your last run. arXiv only ever carries the current announcement cycle, so a wider window helps journals, not preprints. |
| **Haiku prefilter** | On | A cheap Haiku pass drops the clear misses before Sonnet scores anything. The main cost saving. |
| **Drop if Haiku scores ≤** | 3 | Higher saves more and risks losing borderline papers. |
| **Abstract chars** | ∞ | How much of each abstract is sent for scoring. Lower it if a run costs more than you expect. |

**■ Stop** appears during a run and halts it after the batch in flight; papers already scored are kept.

---

## Journals and the arXiv.cs filter

Sources are tagged **api** (OpenAlex, reliable) or **rss** (arXiv, via a public proxy).

**cs.LG**, **cs.AI** and **stat.ML** carry hundreds of papers a day. The **arXiv.cs filter** keeps only those whose title or abstract mentions one of your terms *or* a method from your Transferable Methods list, and it runs in the browser, so rejected papers cost nothing. It **ships empty** — click *load suggested terms* to fill it with an earth-science list you can edit. No other feed is filtered this way.

**Custom journals** take one line each, `Name | S-ID or ISSN`, from [openalex.org](https://openalex.org). Use the `S…` source ID, not a `W…` work ID.

---

## Reading the feed

Above the cards: **Fetched**, **Shown**, **New**, **Score≥8**.

On each card, the left rail carries the score badge, journal and date. Colour there means the score band and nothing else — green 8–10, amber 5–7, red 1–4. A **ring around the badge** means the paper is new since your last run.

The **abstract** sits under the authors, clamped to four lines and starting at the paper's actual contribution rather than its opening background. **Show more**, or a click anywhere on the card, opens the rest. **Score reasoning** below it is Claude's one-sentence justification — the best tool you have for tuning the profile.

| Action | Effect |
| --- | --- |
| **Copy citation** | Formatted citation; preprints are labelled and carry their URL. |
| **☆ Star** | Keeps the paper in the Starred view forever, whatever its score. Survives **Clear cache**. |
| **+ More like this** | Records the paper as a *learned example* — see below. |
| **Dismiss** | Hides it and keeps it out of future runs. |

The title is the link to the paper.

**Learned examples** are the papers you marked with **+ More like this**. They go to Claude as calibration for what an 8–10 looks like in your field, which is often faster than describing it in *Prioritize*. The list sits at the foot of the profile panel: the newest 20 are sent by default (slider, 0–30), up to 100 are stored, and what is listed there is exactly what is sent. Adding or removing one re-scores on the next run.

---

## Filter, search, export

**All** / **New only** / **★ Starred**, a **Score ≥ N** floor (default 6), and a search box that matches title and author.

The starred view ignores both the score floor and the dismissed list, on purpose — an explicit star outranks them.

**Export** opens a styled report of whatever is currently shown, downloadable as one self-contained file. **Brief** asks Sonnet for two or three bullets per paper — what is interesting about it, and why it matters to you — for up to 50 papers, in a single call.

---

## Cost

The **Spend** line in the sidebar footer shows this run, this calendar month, how many papers that covers, and when the rates were last checked. The figures are measured from the token counts each response reports, not estimated, and the rates are constants in the app rather than a setting.

To spend less, in order of effect: put terms in the arXiv.cs filter, leave the Haiku prefilter on, and re-run over overlapping windows — cached papers are free.

---

## Odds and ends

- **Clear cache** resets seen papers, dismissals and cached scores. Starred papers are kept.
- **New only** works across sessions; the history persists in your browser.
- arXiv is reached through free public proxies, so an occasional run comes back with no preprints. Try again later. arXiv also publishes nothing at weekends.
- Papers appearing in more than one source are merged, preprint with published version, keeping whichever abstract actually exists.

---

## License

[CC BY 4.0](LICENSE). Built by Elizabeth A. Barnes with Claude (Anthropic) · 2026.
