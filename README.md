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
3. **Journals.** Open **Select journals** and pick your sources. All are on by default on a first run; a journal added in a later release arrives switched off, so check the list after an update.
4. **Fetch.** Click **↓ Fetch & Score**.

Expect roughly **$0.10–0.12 per 100 papers**.

---

## The interface

**Left rail**, top to bottom: **Fetch & Score**, **Brief** and **Export**; at the foot, **Research profile**, the theme control, **Documentation** and **Back up & restore**. The rail's **Fetch & Score** spins and turns red while a run is going, and a click on it stops the run — so starting, stopping and watching a run all survive the sidebar being closed. Hover any of them for its name. The sidebar hides from a button on its own edge, not from the rail.

**Sidebar**: everything that changes the next run — fetch settings, journals, custom sources, API key — with **Cost estimate** and **Clear cache** at the bottom. Its top section does not scroll, so **Fetch & Score** and the status line stay in reach.

**Profile panel**: opens from the rail and narrows the feed. `Esc` closes it.

**Back up & restore panel**: opens from the archive-box button at the foot of the rail.

---

## Back up your settings

Everything you type lives in your browser and nowhere else. A browser set to clear cookies and site data when it closes will erase your research profile, journal selection and settings without asking, and there is no server copy to fall back on. This has already happened to a user.

**Back up settings to a file** writes one small JSON file holding your research profile, journal selection, custom journals, fetch settings, starred papers and learned examples. Your **API key is deliberately not in the file**, so it is safe to email, and the score caches are left out because they rebuild themselves.

**Restore from a file** reads that file back, replaces what is in the browser now, and reloads. A setting the file does not carry is cleared rather than left behind, so a restore returns you to exactly the state you backed up. Your API key and your cached scores are untouched, so re-enter the key after restoring.

Take a backup once your profile is set, and again whenever you change it.

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
| **Fetch days back** | 2 | A slider over 1–30 days with **Auto** at its left stop, or type any window up to 99 into the box. **Auto** asks each source for everything since *that source* was last fetched in full, so a feed that has been failing for a fortnight is caught up on the next run that works while the sources that were fine keep the recent window. Coverage is recorded per source and only when the source answered without error, without a partial, and without being trimmed by the 1,000-paper cap. Journals are asked what OpenAlex has *added* since that mark rather than what was published, because OpenAlex indexes a paper days to weeks after its publication date; arXiv is asked by submission date with a four-day overlap. Catching up costs no extra tokens — papers already scored under the current profile are cache hits. A typed number stays a literal window. |
| **Haiku prefilter** | On | A cheap Haiku pass drops the clear misses before Sonnet sees them. The main cost saving. |
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

The **abstract** sits under the authors, four lines deep. It starts at the paper's finding rather than its opening background; a leading `…` marks where it skipped. **Show more**, or a click anywhere on the card, opens the whole thing; it is not shown when the preview is already the whole abstract. **Score reasoning** below it is Claude's one-sentence justification, and the best guide to tuning your profile.

| Action | Effect |
| --- | --- |
| **Copy citation** | Formatted citation; preprints are labelled and carry their URL. |
| **☆ Star** | Keeps the paper in the Starred view whatever its score. Survives **Clear cache**. |
| **+ More like this** | Records it as a *learned example*. |
| **Dismiss** | Hides it and keeps it out of future runs. |

Clicking anywhere on a card opens the paper — the score, the journal, the date, the title, the authors, the score reasoning, and the space between them. The abstract and the row of buttons are the two exceptions, because a click there already means something else.

**Learned examples** go to Claude as calibration for what an 8–10 looks like in your field, which is often quicker than describing it in *Prioritize*. The list is at the foot of the profile panel: the newest 20 are sent by default (slider, 0–30), up to 100 are stored, and what the list shows is exactly what is sent. Adding or removing one re-scores on the next run.

---

## Filter, search, export

**All** / **New only** / **Score ≥ N** (default 6) / **★ Starred**, and a search box matching title and author.

The starred view ignores the score floor and the dismissed list; the floor is greyed out there. A star keeps the whole paper, abstract included, so it still reads properly months later — papers starred before v3.6 kept a shortened abstract, and re-starring one rewrites it.

**Export** opens a report of whatever is shown, downloadable as one self-contained file. **Brief** asks Sonnet to read everything shown in one call — up to 75 papers, the highest-scoring if there are more — and write one 700–900 word synthesis of the set: three to six themes drawn from the papers themselves, each bullet citing its papers by number, closing with **Emerging Signals** on what cuts across them, contradicts, or is missing. A numbered reference list of every shown paper sits below it, and a citation jumps to its entry. Both act on the papers currently shown; hover them for the count.

---

## Cost

The **Cost estimate** line at the foot of the sidebar shows this run, this calendar month, and how many papers that covers. Token counts are measured — every response reports them — and multiplied by published list rates, so the estimate is in the price, not the usage.

If more than 300 papers could still reach Sonnet, a dialog asks before spending anything, with the estimated cost and run time. The number it quotes is what Sonnet may actually be asked to score, which is neither the number fetched nor the number missing from the score cache. There are two caches: Sonnet's, and the Haiku prefilter's own. A paper the prefilter rejected on an earlier run is not in the score cache — Sonnet never scored it — but it is in the prefilter's, so it is rejected again for no API call at all, and the dialog counts those separately instead of charging you for them. Its figure is a ceiling, since papers meeting the prefilter for the first time will mostly not get past it.

To spend less, in order of effect: put terms in the arXiv.cs filter, leave the Haiku prefilter on, and re-run over overlapping windows — a paper already scored, or already rejected by the prefilter, costs nothing the second time.

---

## Odds and ends

- **Messages** come in two colours. Red is a failure — storage full, a run that died, a backup file that could not be read — and always needs you. Amber is a notice: nothing is broken, and it is either an empty state or a report on the run. The three notices a run writes about itself (a source that did not answer, a source that answered for part of its window, the paper cap) carry a **Don't show again** button that silences that kind for good, whichever source it is about next time. That is safe because a source that failed or came back partial keeps its old coverage mark, so its window is asked for again on the next run whether or not the run says so.
- **Clear cache** resets seen papers, dismissals, cached scores, silenced notices, and the record of which arXiv days have been fetched. Starred papers are kept.
- **New only** works across sessions; the history persists in your browser.
- arXiv is reached through free public relays. When they are down the app falls back to one that returns **only the ten newest** papers per feed and says so above the cards; those days are requested again on your next run.
- Messages above the cards can be dismissed with the **×** at their right. A run that still has something to report says so again.
- Papers found in more than one source are merged, preprint with published version, keeping whichever abstract exists.
- The app is one HTML file, so you can save a copy and keep it.

---

## License

[CC BY 4.0](LICENSE). Built by Elizabeth A. Barnes · 2026.
