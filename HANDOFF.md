# handoff: finish joai and push it live

Repo: `mrshippers/portfolio`, branch `main`, HEAD `5d812d0`. One hand-built `index.html`, no build step. The copy and structure are done; what's left is assets, two numbers, the deploy, and the rename. Read `index.html` once before touching it.

## 0. prime directive

If any step below fights the page being good, break the step and say which.

## 1. hard constraints

- No framework, no bundler, no new dependencies. `index.html` stays one file.
- No pills or chips. No em dashes anywhere in copy. No rule-of-three padding.
- Fonts stay Hanken Grotesk and JetBrains Mono only.
- Do not restore the StrydeOS website screenshots, the "demo clinic" subtext, the font credit in the footer, or the line under the tabs. They were removed on purpose.
- Images go in `assets/` at 1600×1000 JPEG (quality ~85), 16:10, `object-position: top` is assumed. Phone shots stay 462×1000.
- Nothing from `SHP001_AGElectrical_Deposit.pdf` goes on the page (bank details). The invoice is not mentioned anywhere.

## 2. craft slots waiting on assets

Each `article.piece.pending` has `img[data-src="assets/<file>"]`. It renders only when `<file>` is listed in `assets/manifest.json` → `ready`. Missing files produce no 404s; the piece is removed at runtime.

| file | piece | where to get it |
|---|---|---|
| `bypass.jpg` | Bypass, parametric turbofan | `bypass.html` from the Sep 7 session, exploded view at 1440×900. `bypass.mp4` optional, add a `<video>` the same way `africa.mp4` is wired. |
| `renovation.jpg` | Spires renovation | Jamal's render. Check the caption, it was written without seeing it. |
| `flashing-lights.jpg` | Flashing lights | Jamal's render. Caption written blind; fix the Built-with line to match reality. |
| `padel.jpg` | Padel | Jamal's render. Caption claims regulation dimensions; confirm or cut that line. |
| `tm-system.jpg` | TradeMind, designed first | The 10-screen kit / brand book sheet from `~/Downloads/AI/Shippers/TradeMind/`. |
| `icons.jpg` | Marks for the fleet | The four home-screen icons (Oracle, Sillage, Slate, Shippers) on one 16:10 dark sheet. |
| `splat.jpg` | Room scan viewer | Screenshot of `splat-viewer.html` with a scan loaded. |
| `stryde-weekly.jpg` | StrydeOS, from the inside (frame 1) | Screenshot of the weekly summary email StrydeOS sends Jamal. Redact patient names if any appear. |
| `stryde-gate.jpg` | StrydeOS, from the inside (frame 2) | Ava's go-live gate report, Run 11, 8 Oct 2026. |

Add to manifest, push, done. A piece with two frames shows when either file is listed.

## 3. two numbers to confirm with Jamal

In the hero legend and `COUNTS` in the script (both must match, the orb is drawn from `COUNTS`):

- `579` patients scored by Pulse. Possibly stale. Ask for the current figure.
- `500` Evidence training examples. He flagged Evidence and did not say what to change. Ask.

The other three (898, 840, 521) are from October and fine.

## 4. deploy

The last two pushes to `main` did not reach joadu.pages.dev. The Pages project is either not git-linked or linked elsewhere. Check Cloudflare → Workers & Pages → `joadu` → Settings → Builds. If it is not linked to `mrshippers/portfolio` `main`, either link it (build command empty, output directory `/`) or deploy direct:

```
npx wrangler pages deploy . --project-name=joadu
```

Then `curl -s https://joadu.pages.dev | grep '<title>'` must print `<title>joai</title>`.

## 5. rename to joai.pages.dev

Cloudflare → the Pages project → Settings → rename project to `joai`. The page already points `og:image` at `https://joai.pages.dev/assets/og.jpg` and the week-resolved chrome text already says `joai.pages.dev/week-resolved`. After the rename, redeploy once so the old `joadu` subdomain is no longer canonical. Also regenerate `assets/og.jpg` from the new hero (1200×630) because the current one still carries the old headline.

## 6. verify before "done"

Serve locally (`python3 -m http.server 8765`) and run a Playwright pass at 1440×900 and 390×844:

- console errors: 0
- 4xx responses: 0
- axe wcag2a/aa violations: 0
- every `.piece` listed in the manifest actually renders, and nothing pending is left in the DOM
- screenshots reviewed by eye at both widths, work tab and craft tab

Three failed passes on the same item: stop and hand back with the numbers.

## 7. commit

Short message, what changed, no essay. Push to `main`.
