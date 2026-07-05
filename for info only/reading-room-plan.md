# The Reading Room — Definitive Build Plan

Your private online library for completed RP chats. Kindle-style reading, tags,
easy drop-in of new stories, no git knowledge required beyond three buttons in
GitHub Desktop.

---

## The shape of the thing

**Where stories live:** your GitHub repo (`CatParky/the-reading-room`), in a
`/stories` folder. One file per story. Google Drive (`AI/The Refuge`) stays as
your *working* space — raw exports and drafts live there; only finished,
tidied stories get promoted into the repo.

**How the site works:** a single `index.html` app (already built in draft form)
served by GitHub Pages. It reads a `manifest.json` listing all stories, shows
them as a browsable shelf with tags/search, and renders each one in a proper
reading view — serif typography, sepia/dark themes, comfortable line length,
mobile-friendly. No white-background markdown eye strain.

**How new stories get in:** you drop a file into the `/stories` folder and
push. A small GitHub Action (a robot script that runs automatically on GitHub's
servers) rebuilds `manifest.json` for you every time you push. You never edit
the manifest by hand. "Drop in a new chat" is genuinely just: save file,
commit, push.

**Privacy:** a passphrase gate. Story text in the repo is encrypted; the site
asks for your passphrase once (remembered on your device) and decrypts in the
browser. Without the phrase, both the site and the raw repo files are
unreadable. This is the piece that makes a *public, free* repo safe for
private adult creative work.

---

## Folder layout (what the repo will look like)

```
the-reading-room/
├── index.html          ← the app (I build this)
├── manifest.json       ← auto-generated, never touch
├── stories/
│   ├── thaw.md
│   ├── masquerade.md
│   └── ...
├── tools/
│   └── prepare.html    ← local page that encrypts + formats a story for you
└── .github/workflows/
    └── build.yml       ← the robot that rebuilds the manifest
```

## Story file format

Each story is a markdown file with a small header block (frontmatter) on top:

```
---
title: Thaw
character: Gale
scenario: Thaw
tags: [slow burn, amnesia, cabin]
status: complete
date: 2026-01-12
summary: Two strangers find the first uneasy rhythm of a shared roof.
---

The story text, in plain markdown, exactly as you edited it.
```

The header drives the shelf: character, scenario, and tags all become
clickable filters, and full-text search covers everything.

## Your workflow for adding a chat (the whole thing)

1. **Export** the chat from Claude/CrushOn as usual; drop the raw file in
   Drive → `AI/The Refuge` as you already do.
2. **Edit** it there at leisure — tidy the text, cut what you don't want.
3. **Prepare**: open `tools/prepare.html` (a local page, works offline),
   paste the edited text, fill in title/tags in a little form, enter your
   passphrase. It hands you back a ready-made encrypted story file.
4. **Drop in**: save that file into the `stories/` folder inside your
   GitHub Desktop repo folder (Repository → Show in Explorer).
5. **Push**: in GitHub Desktop — tick the file, type a summary like
   "Add Thaw", click *Commit to main*, click *Push origin*.
6. Wait a minute. It's live in your library.

Steps 4–5 are the only "git" you ever do, and they're two buttons.

## What gets built, in order

**Phase 1 — the shelf and reader (mostly done).** The `index.html` you
already have, upgraded to: read `manifest.json`, passphrase gate + decryption,
reading themes (sepia / dark / dusk — no harsh white), font size control,
remember-my-place per story.

**Phase 2 — the pipeline.** The `prepare.html` tool (paste → tag → encrypt →
download) and the GitHub Action that auto-builds the manifest. After this,
the drop-in workflow above works end to end.

**Phase 3 — optional, later.** Google Drive integration so the app can pull
drafts from The Refuge directly. Deferred deliberately: it needs OAuth setup,
adds complexity, and the manual Drive→prepare→drop flow is honestly quick.
Also optional: in-browser editing that commits back via the GitHub API.

## What I need from you

- Confirm you're happy with the passphrase approach (vs. paying for a
  private repo, or plain obscurity).
- Pick a passphrase you'll remember — it is *not recoverable*. If you lose
  it, the encrypted copies in the repo are unreadable; your Drive originals
  remain your safety net, which is another good reason to keep them.
- One sample story (any completed chat, already edited) to test the full
  pipeline with.

## Things this plan deliberately avoids

- Hand-editing `manifest.json` (robot does it)
- Terminal commands (GitHub Desktop only)
- Servers, databases, hosting costs (all static, all free)
- Renaming or restructuring your Drive folder (it stays exactly as is)
