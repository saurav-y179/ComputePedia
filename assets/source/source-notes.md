# Source notes — ComputePedia image assets (HiEQ Layout die-shot gallery)

Compiled: 2026-10-05. Everything below was verified from the live site or its public data files.
Nothing in this file is guessed: any item that could not be verified is marked as such.

---

## 1. Original website

| Field | Value |
| --- | --- |
| Site URL | https://hieq-home404.pages.dev/ |
| Gallery view inspected | https://hieq-home404.pages.dev/#/layout (hash route, "Layout" section) |
| Site title / brand | HiEQ·HOME / "HiEq" |
| Hosting | Cloudflare Pages (stated in the site footer) |
| Site description (meta) | "一个小小的个人网页" ("a small personal webpage") |
| Author profile linked in header/footer | https://github.com/HiEq |

The site is a single-page app: the gallery is not written into the HTML, it is loaded at
runtime by inline JavaScript.

---

## 2. Where the gallery data was found

| File | URL | Size | Notes |
| --- | --- | --- | --- |
| Primary manifest | https://hieq-home404.pages.dev/gallery.json | 117,337 bytes | 232 items — the "Dieshot" collection shown on `#/layout` |
| Secondary manifest | https://hieq-home404.pages.dev/misc.json | 13,380 bytes | 25 items — a "misc" collection (screenshots/other images, not CPUs) |
| Auto-generated catalog | https://hieq-home404.pages.dev/CATALOG.md | 30,781 bytes | Vendor index generated from `gallery.json` on 2026-09-23 (26 vendors / 219 items at that time) |

`gallery.json` structure (preserved verbatim in `assets/data/gallery.json`):

```jsonc
{
  "version": 1,
  "site":   { "title", "sub", "brand", "avatar", "bg" },
  "config": { "adminHash": "...", "github": { "token", "owner", "branch", "dir", "manifest" } },
  "items": [
    { "id", "coll", "title", "desc", "tags": [], "name", "size", "addedAt",
      "w", "h", "src", "file", "thumb" }
  ]
}
```

Per-item metadata available: `title`, `desc` (used by the site as the process-node/technology
line), `tags` (used as the vendor/category chips), `w`/`h` (dimensions), `size` (bytes of the
served file), `addedAt`, `src`/`file` (image path), `thumb` (thumbnail path), `id`.

How the page loads it (verified in page source): `fetch('gallery.json?t=' + Date.now())` and
`fetch('misc.json?t=' + Date.now())`, i.e. relative to the site root.

---

## 3. Where the image files were found

| Collection | URL pattern | Example |
| --- | --- | --- |
| Dieshot (main) | `https://hieq-home404.pages.dev/images/dieshot/<vendor-slug>/<file>` | `.../images/dieshot/intel/img_0752-t5al.jpg` |
| Thumbnails | same path + `.thumb.jpg` | `.../images/dieshot/intel/img_0752-t5al.thumb.jpg` |
| Misc | `https://hieq-home404.pages.dev/images/misc/<YYYY-MM>/<file>` | not downloaded |

All 42 downloaded URLs were taken from the `src` field of `gallery.json` and verified to exist
in that manifest before downloading. No URL was constructed or guessed.

---

## 4. Repository / source URL

From `gallery.json` → `config.github`:

```json
{ "token": "", "owner": "HiEq/Dieshot", "branch": "main",
  "dir": "images/dieshot", "manifest": "gallery.json" }
```

Verification results (GitHub API, 2026-10-05):

- `https://api.github.com/repos/HiEq/Dieshot` → **404** (repository is private or deleted; it
  is not publicly readable, so the true full-resolution originals and any repo LICENSE could
  not be inspected).
- `https://github.com/HiEq` → 3 public repositories: `HiEq/HiEq`, `HiEq/Python-ASC2-Player`,
  `HiEq/tubatoolsPlugin`. **None** contains this gallery, and **none** reports a license
  (`license` field is empty for all three).
- The page also contains admin endpoints (`/api/login`, `/api/gh`) used for an upload panel;
  they were not used and not probed.
- No other repository URL, no `LICENSE`, and no project homepage for the site source were
  found anywhere in the page or its data files.

Conclusion: **the only publicly reachable source of these images is the Pages site itself.**

---

## 5. Licensing / usage / attribution — what was actually found

Searches performed (English + Chinese terms): `license`, `copyright`, `CC`, `CC0`, `CC BY`,
`attribution`, `credit`, `terms`, `许可`, `版权`, `授权`, `图源`, `来源`, `署名`, across:

- the complete page HTML including all inline CSS and JavaScript,
- `gallery.json`, `misc.json`, `CATALOG.md`,
- the page footer (brand, hosting, catalog link, two friend links, dynamic year).

**Result: zero matches. There is no explicit license, copyright notice, terms of use, or
attribution requirement anywhere on the site.**

Implications for reuse:

1. **No permission to reuse is granted by the site.** Absent a license, default copyright
   applies; treat these images as all-rights-reserved by their respective rights holders.
2. **No attribution requirement is stated** — but equally, no credit is given for the
   originals, so the chain of title is unknown.
3. **Provenance hints (not verified):** some original filenames in `gallery.json` look like
   third-party photo IDs or camera filenames, e.g.
   `48319140097_4a5e932125_o-guz2.jpg`, `52402446455_4ef38a5fff_o-1fus.jpg` (Flickr-style),
   `IMG_0752`, `IMG_0804`, `IMG_20261001_230751`. The site does not document where any image
   came from, so some or all of these photographs may belong to third parties rather than to
   the site owner.
4. **Trademarks:** product and company names (Intel, AMD, Apple, Qualcomm, MediaTek, Samsung,
   NVIDIA, Spacemit, …) are trademarks of their owners; the images are photographs/micrographs
   of physical dies.
5. **Recommendation for ComputePedia:** if the site will be published or used commercially,
   obtain permission from the site owner (GitHub: `HiEq`) and/or the original photographers
   first. For private study/prototyping, keep this file with the assets.

`config.adminHash` inside `gallery.json` is a password hash that the site itself publishes in
the clear. It was kept only so the copied manifest stays byte-identical to the original
(verified: SHA-256 of `assets/data/gallery.json` matches a fresh download from the site).

---

## 6. Image quality / resolution caveat (important)

From the header of `CATALOG.md` (translated from Chinese):

> Web copies total 342.4 MB (JPEG q45 / long edge 4096 px), plus 219 thumbnails (720 px).
> Aspect ratio is identical to the original; the originals (about 9.4 GB) are kept locally and
> are **not** published.

Verified against the 42 downloaded files:

- 38 files have a long edge of exactly 4096 px → downscaled/re-encoded by the site.
- 1 file is smaller than the cap: `amd-steamdeck.jpg` (2854×2601).
- 3 files exceed the cap and are closer to the author's originals:
  `qualcomm-snapdragon-8-elite-gen6.jpg` (10557×10116), `mediatek-helio-g95.jpg` (9033×9380),
  `mediatek-dimensity-9600-pro.jpg` (10000×8810).
- The `w`/`h` values in `gallery.json` describe the **originals**, not the served files; both
  values are recorded per image in `assets/data/image-index.json`
  (`dimensions` = downloaded file, `originalDimensions` = source manifest).

So the highest resolution obtainable publicly is what the site serves (max ~4096 px for most
images). The true high-resolution originals (~9.4 GB) are **not retrievable** — they exist only
on the author's machine / in the non-public `HiEq/Dieshot` repository.

---

## 7. What was downloaded, and what was deliberately skipped

Downloaded into this project:

| Path | Content |
| --- | --- |
| `assets/images/` | 42 images, 101.1 MB total, all decoded and verified |
| `assets/data/gallery.json` | verbatim copy of the site's manifest (232 items) |
| `assets/data/misc.json` | verbatim copy of the site's second manifest (25 items) |
| `assets/data/image-index.json` | generated index of the 42 downloaded files |
| `assets/source/source-notes.md` | this file |

Deliberately **not** downloaded (per instructions — not needed to locate/reproduce the gallery):

- the site's HTML, CSS, JavaScript bundles, framework/analytics scripts;
- fonts (`fonts/QualcommNext-*.ttf`), Live2D models, icons, `images/home-bg.jpg`, `1.jpg`;
- thumbnails (`.thumb.jpg`) — only full-size files were kept;
- the other 190 images in `gallery.json`;
- all 25 images referenced by `misc.json` (screenshots, not CPUs — none match the requested
  architecture set). `misc.json` itself was kept only as documentation of what exists.

---

## 8. Selection used for ComputePedia (42 images)

| Category | Count | Detail |
| --- | --- | --- |
| x86 / x86-64 | 18 | 9 Intel (Penryn 45nm → Lunar Lake 3nm), 9 AMD (Kabini 28nm → Zen 5c 3nm) |
| ARM | 17 | 5 Apple (A10 16nm → M4 3nm), 5 Qualcomm (820 14nm → 8 Elite Gen6 2nm), 4 MediaTek (G95 12nm → Dimensity 9600 Pro 2nm), 3 Samsung (Exynos 990 7nm → Exynos 2600 2nm) |
| RISC-V | 1 | Spacemit K1 (22nm) — **the site contains only one RISC-V die shot** |
| NVIDIA GPU/accelerator | 6 | GK110 28nm → GP100 16nm → GV100 12nm → GA100 7nm → GH100 4nm → GB10 3nm |

Picks were spread across generations and process nodes as requested. Selection was constrained
to items that actually exist in `gallery.json`; nothing was substituted from outside the site.
