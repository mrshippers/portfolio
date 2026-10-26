# handoff: what is left on joai

Repo: `mrshippers/portfolio`, branch `main`. One hand-built `index.html`, no build step. Live at https://joai.pages.dev. Read `index.html` once before touching it.

## 0. prime directive

If any step below fights the page being good, break the step and say which.

## 1. hard constraints

- No framework, no bundler, no new dependencies. `index.html` stays one file.
- No pills or chips. No em dashes anywhere in copy. No rule-of-three padding.
- Fonts stay Hanken Grotesk and JetBrains Mono only.
- Do not restore the StrydeOS website screenshots, the "demo clinic" subtext, the font credit in the footer, or the line under the tabs. They were removed on purpose.
- Images go in `assets/` at 1600×1000 JPEG (quality ~85), 16:10, `object-position: top` is assumed. Phone shots stay 462×1000.
- Nothing from `SHP001_AGElectrical_Deposit.pdf` goes on the page (bank details). The invoice is not mentioned anywhere.
- Captures come from the CDP Chrome, which runs the Acrobat extension: check every capture's bottom-right corner for its button (it was baked into the old Shippers poster and video).

## 2. slots still waiting

A `article.piece.pending` renders only when its `img[data-src]` file is listed in `assets/manifest.json` → `ready`.

| file | piece | state |
|---|---|---|
| `stryde-weekly.jpg` | StrydeOS, from the inside (frame 1) | The weekly Brief was unpaused 22 Oct; its cron first fires Mon 26 Oct 07:00 UTC. Screenshot the first real one, redact patient names. The piece shows with the gate frame alone until then. |
| `splat.jpg` | Room scan viewer | `splat-viewer.html` was not found on disk, in GitHub, or among the artifacts. Needs Jamal to point at it. |

Removed on purpose, ask Jamal before restoring: the Flashing lights slot (a private piece; its own spec says unlisted, noindex, no OG image) and Oracle from the icon sheet (no Oracle icon exists anywhere; the sheet shows Sillage, Slate and Shippers).

## 3. deploy

Pages projects are not git-linked, and a `pages.dev` subdomain cannot be renamed, so `joai` is its own project and `joadu` serves only a 301 to it. Deploy the tracked files, never the working tree (this file and `.DS_Store` stay out):

```
rm -rf /tmp/joai && mkdir /tmp/joai && git archive HEAD | tar -x -C /tmp/joai && rm /tmp/joai/HANDOFF.md
npx wrangler pages deploy /tmp/joai --project-name=joai --branch=main
curl -s https://joai.pages.dev | grep '<title>'   # <title>joai</title>
```

## 4. verify before "done"

Serve locally (`python3 -m http.server 8765`) and check at 1440×900 and 390×844: 0 console errors, 0 4xx, 0 axe wcag2a/aa violations, every manifest file rendered and nothing pending left in the DOM, screenshots of both tabs reviewed by eye. Playwright `connectOverCDP` hangs on this Chrome when it has many tabs; drive one page target over raw CDP instead (`PUT /json/new`, `Page.bringToFront`, close the tab after). Ignore exceptions from `chrome-extension://` contexts, and wait for `.rv` reveals to finish before running axe.
