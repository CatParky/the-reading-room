# The Refuge — Deploy & Retire Guide

You are merging two repos into one. After this, everything lives in
**the-reading-room**, and **universal-story-export** is retired.

Nothing about your existing sealed stories changes — the encryption is
identical across all pages, so old files keep working.

Take it in three parts: put the new files in, retire the old repo, then
test. Fifteen minutes, unhurried.

---

## Before you start — one safety net

Your live library URL is `catparky.github.io/the-reading-room/`. It stays
exactly the same after this. The other site
(`catparky.github.io/universal-story-export/`) is the one going away.

If you want a backstop, in GitHub Desktop with **the-reading-room** as the
current repository: **Branch → New branch**, call it `before-merge`, publish
it, then switch back to `main`. That gives you a labelled snapshot to return
to if anything feels wrong. Optional, but cheap peace of mind.

---

## Part 1 — Put the new files into the-reading-room

### Step 1: open the repo folder
GitHub Desktop → make sure **Current repository** is **the-reading-room** →
**Repository → Show in Explorer**.

### Step 2: copy the new files in
From the `merged` folder I gave you, copy these into the repo folder,
overwriting when asked:

```
landing.html          ← NEW (the Refuge home page)
pipeline.html         ← NEW (Extract · Check · Archive · Edit sealed)
mhtml-converter.html  ← NEW (legacy, now with a nav bar)
claude_export.py      ← NEW (the Python extractor script)
index.html            ← REPLACE (library — now with the Refuge nav bar)
tools/prepare.html    ← REPLACE (now matches everything else)
.github/workflows/build.yml   ← already there; leave it
```

`.github` may be hidden in Explorer (View → Show → Hidden items).

### Step 3: rename the old extractor
The old `universal-story-export` repo had its own `index.html` that was the
**extractor** tool. In the merged repo, `index.html` is the **library**, so
the extractor needs a new name. If you want to keep the extractor:

1. Find the old extractor `index.html` (it's in your universal-story-export
   repo folder, NOT the reading-room one).
2. Copy it into the-reading-room folder and rename it to **`extractor.html`**.

`landing.html` already links to `extractor.html`, so once it's there the link
works. If you don't use the extractor, skip this — just delete the
"extractor" link from `landing.html` later, or leave it (a dead link on a
private legacy line is harmless).

### Step 4: commit and push
Back in GitHub Desktop, the Changes panel lists the new/changed files.
Summary: `Merge the Refuge into one repo`. **Commit to main → Push origin.**

Give it a minute. Visit `catparky.github.io/the-reading-room/landing.html` —
you should see the Refuge home with the shared nav and a Theme button.

---

## Part 2 — Retire universal-story-export

The old repo's site is now duplicated inside the-reading-room. Turn it off so
there's only one Refuge.

### The gentle option (recommended): turn off its Pages site
1. On github.com, go to the **universal-story-export** repo.
2. **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **None**. Save.

The repo and its files still exist as a backup, but the public site stops
serving. Nothing links to it any more, so it simply goes quiet.

### The tidy-later option: archive the repo
Once you're happy the merged site does everything, you can archive the old
repo (Settings → scroll to bottom → **Archive this repository**). This makes
it read-only and clearly "retired" without deleting anything. Do this only
after a week or two of the merged site working, so you're certain nothing was
left behind.

**Don't delete it outright yet.** Keep it as cold storage until the merged
site has proven itself.

---

## Part 3 — Test the merged site

Work through these once. Each proves one link in the chain.

1. **Navigation.** From `landing.html`, click Home, Pipeline, Edit sealed,
   Library in turn. Every page should carry the same top nav in the same
   place, and land you where the label says.

2. **Theme carries across pages.** On any page, click **Theme** until it's
   dark. Navigate to another page — it should already be dark. This confirms
   the shared setting works.

3. **Seal a chapter through the pipeline.** Pipeline → drop or paste a chat →
   Check → Archive. Fill in **Story Name**, a **Part Number**, a **Chapter
   Title**, pick **Tags** from the autocomplete, add a summary, enter your
   passphrase. Click **Seal & download**. Also click **Text copy** — you
   should get a plain `.txt` backup too.

4. **Shelve it.** Put the `story-XXXX.json` into `stories/`, commit, push.
   Watch the repo's **Actions** tab for the green "Rebuild manifest" run.

5. **Read it.** Open the library, enter your passphrase. The new chapter
   should appear, and if it's part of a multi-part story, chapters should sit
   in Part order under one story.

6. **Edit sealed + offline backup.** Pipeline → Edit sealed → drop the sealed
   file, enter passphrase, Unseal. Confirm Story/Part/Chapter/Tags all load.
   Click **Markdown copy** — you get a plain backup. Make a tiny edit,
   Re-seal, and replace the file in `stories/`.

If any step misbehaves, note exactly which one and what you saw.

---

## The everyday routine, after merging

**Add a story:** Pipeline → Extract → Check → Archive → seal → drop the JSON
in `stories/` → commit & push. (Or `tools/prepare.html` if you already have
clean text and don't need Extract/Check.)

**Back up plain copies:** any Archive or Edit-sealed screen has Markdown/Text
copy buttons — grab one whenever you want an unencrypted offline copy for
your own records. Your Drive originals remain the ultimate safety net.

**Edit a story:** Pipeline → Edit sealed → unseal → change → re-seal →
replace in `stories/` → push.

**Clean up tags:** the "Manage your tag list" panel in Archive renames or
deletes entries in your autocomplete library.

---

## What lives where now (final structure)

```
the-reading-room/
├── landing.html          Home — the Refuge front door
├── pipeline.html         Extract · Check · Archive · #edit
├── index.html            the library (your live URL points here)
├── extractor.html        legacy Python-export helper page (if kept)
├── mhtml-converter.html  legacy converter (superseded by Pipeline)
├── claude_export.py      Python extractor script
├── tools/
│   └── prepare.html      quick seal-from-clean-text path
├── stories/              your sealed library
├── manifest.json         auto-rebuilt by the robot
└── .github/workflows/build.yml
```

## A note on the two overlapping tools

`tools/prepare.html` and the pipeline's Archive stage now do the same job.
Both are kept: Prepare is the fast lane when you already have tidy text;
Archive is the end of the full Extract-Check-Archive journey. If you find you
only ever use one, delete the other later — they don't conflict.
