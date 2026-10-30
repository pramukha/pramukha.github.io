# pramukha.github.io

Personal portfolio of **Pramuk Wijerathne** — AI Architect · Solution Architect · Forward Deployed Engineer.

- Live: https://pramukha.github.io
- Repo: https://github.com/pramukha/pramukha.github.io
- Deploy: GitHub Pages → *Deploy from a branch* → `main` / root

The whole site is **one self-contained `index.html`**: inline CSS and JS, no build step, no frameworks, no npm.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site. The hero photo is embedded in it as a base64 data URI, and the favicon is an inline SVG. |
| `og-image.jpg` | 1200×630 social preview image (`og:image` / `twitter:image`). |
| `profile.jpg` | 680×680 source photo. The site doesn't load it; it's kept as the original for re-embedding. |
| `resume.pdf` | Linked from the hero "Download Resume" button. |
| `assets/about-film.mp4` | 20 s silent loop shown in About in place of the highlights grid (864×1080, H.264, no audio). Made in code; the source and render steps live in `D:\projects\motion-video\PCUB-health-safety\films\github-io`. |
| `assets/about-film-poster.jpg` | The film's first frame, used as its poster and fallback image. |

## First-time setup: connect this folder to GitHub

These files were copied from the repo without git history. To turn this folder into a working copy of the repo, run this once from inside `D:\projects\github-io`:

```bash
git init -b main
git remote add origin https://github.com/pramukha/pramukha.github.io.git
git fetch origin
git reset --mixed origin/main   # adopt the repo's history; your files stay as they are
git status                      # should report a clean tree
```

After that, publish with `git add . && git commit -m "..." && git push`. Pages redeploys in about a minute. You can also keep uploading `index.html` through GitHub's web UI.

## Preview locally

Open `index.html` directly in a browser, or serve the folder:

```bash
python -m http.server 8000   # then open http://localhost:8000
```

## Where to edit things

### Content: edit the data, not the markup
Most sections are rendered by JS from constants near the top of the single `<script>` block (search for `/* ============== DATA`):

| Const | Renders |
|---|---|
| `EXP` | Experience timeline (`role, co, loc, per, now, sum, pts[], tech[]`) |
| `PROJ` | Featured projects (`t, org, dt, cat, d, impact, tech[]`, optional `link:{u,l}` for a read-more link). Filter buttons come from the unique `cat` values. |
| `SKILLS` | Skill groups (`c` category, `i` SVG path markup for the icon, `s[]` skills) |
| `CERTS` | Certifications & awards (`award:true` gives the amber treatment) |
| `ACH` | The 2×2 highlights grid in About (`n` + `suf` gives an animated counter; `txt` shows a static value instead) |
| `EDU` | Education timeline |
| `POSTS` | Writing section: the Medium build series in series order (`t` title, `s` subtitle, `d` date, `u` URL) |
| `MARQ` | Tech marquee under the hero |
| `ROLES` | Rotating role line in the hero (just below the RENDER block) |

The hero, About paragraphs, affiliations and contact details are plain HTML in `<main>`.

### Design tokens (top of `<style>`)
Light values live on `:root` and dark values on `[data-theme="dark"]`. Dark is the default, and the toggle saves the choice to `localStorage`.

- Surfaces: `--bg`, `--bg-2` (feature band and footer), `--s1` (cards), `--s2` (pills and insets), `--s3`
- Text: `--fg`, `--fg-2`, `--muted-fg`
- Lines: `--border`, `--border-2`
- Accent: `--primary` (indigo, the single working accent), `--primary-soft`, `--accent` (cyan, used only in the hero gradient phrase and the progress bar), `--ok` (green, for "current" and "available")
- Type scale: `--fs-1` (h1) through `--fs-7` (labels)
- Other: `--radius`, `--radius-sm`, `--shadow`, `--shadow-lg`, `--ring`, `--ease`, `--maxw`

Fonts: **Sora** for headings, **Inter** for body text, **JetBrains Mono** for dates, labels and metadata.

### Motion hooks
| Hook | Effect |
|---|---|
| `.rv` + `style="--d:120ms"` | Scroll reveal with stagger delay |
| `.word` + `--wd` | Hero word-by-word reveal (applied automatically to `#h1` and `#sub`) |
| `.spot` | Cursor spotlight (styled on `.pcard` and `.hcard` only) |
| `.tilt` | Subtle 3D tilt (project cards) |
| `[data-mag]` | Magnetic button (Download Resume only) |
| `.count` + `data-to` | Count-up number |
| `.tl-fill`, `.job.lit` | Timeline line draws and dots light up on scroll |
| `.mesh`, `.grain` | Ambient blobs and film grain |
| `.marq` | Marquee (pauses on hover) |
| `#prog` | Scroll progress bar |
| `.film`, `#film` | About film. The video source loads only when About nears the viewport and pauses off-screen. With reduced motion the video never loads and the highlights grid shows instead; otherwise the grid stays in the page, visually hidden, for screen readers and search. |

The `@media (prefers-reduced-motion: reduce)` block at the end of the CSS turns all of this off. Keep it.

## Constraints to keep
1. One file: all CSS and JS stay inline.
2. No new external requests. The only ones today are Google Fonts, `resume.pdf` and the About film in `assets/` (same origin, lazy-loaded).
3. Keep the photo embedded (base64) and the favicon inline.
4. Accessibility: keep the skip link, ARIA labels, visible focus rings, semantic sections and the reduced-motion block.
5. SEO: keep the OpenGraph and Twitter meta, the JSON-LD `Person` schema and the canonical URL. `og:image` must stay an absolute URL.
6. Facts (roles, dates, companies, certifications) come from the CV. Wording can be polished, but facts change only on purpose.

### Replacing the hero photo
Encode the new image and swap it into the `<img src="data:image/jpeg;base64,...">` inside `.photo`. Keep it around 680px and a moderate JPEG quality so `index.html` stays near 150 KB.

```bash
# macOS / Linux / Git Bash
base64 -w0 profile.jpg > photo.b64
```
```powershell
# PowerShell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("profile.jpg")) | Set-Content photo.b64
```

## Changelog
- **Oct 2026: Medium.** Medium icon in the hero socials. New 04 Writing section listing the CAS-RAG build series (later sections renumbered 05 to 08, Writing added to the navs). The CAS-Agentic-RAG card now states measured results (F1 0.42 to 0.63, then +5% from classifier priors), links to the series and lists DeBERTa-v3, fastapi-mcp and text-embedding-3-large. Three new Inov8 bullets from the posts. "150K+ datasets" now reads "150K+ incident cases" in the bullets, the About highlight and the film.
- **Oct 2026: About film.** A 20 s silent loop walks the career line (2004 Sri Lanka, 2014 Singapore, 2025 Wellington, GovHack 2025) and ends on the name card. It replaces the highlights grid on screen; the grid remains for reduced motion and assistive tech.
- **Sep 2026: design refresh.** Name-first hero with a rotating role line (the old typewriter line was empty). Fixed the stat-counter sizing bug. Folded Achievements into the About highlights grid. Numbered, left-aligned section headers. Three-tier surfaces in place of glass. Indigo-led palette. Fluid type scale. Experience dates moved into their own column. Removed decorative idle motion.
