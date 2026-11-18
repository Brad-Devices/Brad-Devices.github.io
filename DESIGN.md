# SITE DESIGN — the Brad Devices site's own design guideline

> **State:** POLICY — adopted 2026-11-17. §9 is the adoption ledger; §11 is the
> debt this document refuses to hide.
> **Governs:** `site/` — the fourteen public doors, the deck, the assets they
> serve.
> **Does not govern:** `src/bradium/` (BradUI, the product's design system) or
> any BradOS surface. See §1.
> **Owns the rules, not the art.** The mark, the words and the big composition
> calls are The Architect's. This file records what has been decided so the next
> agent does not re-decide it by accident.

---

## 1. Three layers, one brand

"Separate BradUI from the website" is right, and it is not a fork of the brand.
It is three layers with one identity at the top:

```
                    ┌───────────────────────────────┐
   THE BRAND  ──────▶│ tokens: mark, gold ramp,      │  one identity
   (shared)          │ Orbit Black, Mist, status set │  nobody duplicates
                    └──────────────┬────────────────┘
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
   ┌──────────────────────┐                  ┌──────────────────────┐
   │ THE SITE             │                  │ THE PRODUCT          │
   │ site/DESIGN.md       │                  │ BRADIUM_FOUNDATIONS  │
   │ site/css/style.css   │                  │ src/bradium/         │
   │ layout, type, doors  │                  │ BradOS surfaces      │
   └──────────────────────┘                  └──────────────────────┘
```

| Layer | Owns | Lives in | Consumes |
|---|---|---|---|
| **Brand** | Gold ramp, canvases, ink, status set, mark, wordmark, voice | `src/bradium/tokens/primitives/`, `brand/`, `VOICE.md` | nothing |
| **Site** | Grid, type scale, spacing, components, page patterns, motion, theme parity | `site/DESIGN.md`, `site/css/style.css`, `tools/gen_site_tokens.py` | brand primitives + the measured `light-matter` theme |
| **Product** | BradOS surfaces: compositor, chrome, components | `src/bradium/`, `Brad Software/OS/BradUI/` | brand primitives through the full semantic layer |

**The rule that makes the separation real:** the site never imports
`src/bradium/builds/bradium.css` and never copies a product semantic token. It
consumes the **brand primitives** (`brand.color.*`) plus the measured
`light-matter` theme through `tools/gen_site_tokens.py`, and then defines its own
expressive layer — type, grid, components — by hand. The product keeps its own
semantic layer. They meet at the brand layer, and only there.

Why this was worth doing: before this file, the site carried a hand-typed
palette (`--orbit-black`, `--brad-gold`, …), a hand-typed light override block,
the deck carried a **third** copy inline, and `/now` carried a **fourth** —
Bootstrap's `#2ecc71` / `#f1c40f` / `#3498db` / `#95a5a6` / `#e74c3c`. Four
palettes, one brand, no generator. That is the whole disease.

---

## 2. Colour — normative

**No colour is hand-typed in the site's CSS.** Between the `BEGIN` / `END`
markers in `site/css/style.css` and `site/deck/index.html`, the values are
written by `tools/gen_site_tokens.py` from the brand tokens. Outside those
markers, a gold literal is a failure (`tools/check_site_css.py`).

### 2.1 The map

The site's dark theme resolves the **brand layer** — the front door wears Orbit
Black, not the product's Deep Space. The light theme resolves the **measured
Light Matter** theme through its semantic tokens.

| Site variable | Dark (brand layer) | Light (`light-matter`) |
|---|---|---|
| `--orbit-black` (canvas) | Orbit Black `#0E1117` | Platinum Mist `#D9DEE5` |
| `--orbit-black-2` (surface) | `#12161F` | Mist Surface `#E6EAF0` |
| `--orbit-black-3` (elevated) | `#171C26` | Mist Elevated `#EDF0F4` |
| `--orbit-border` (hairline) | `#232A36` | Mist Hairline `#C3CBD5` |
| `--orbit-border-hi` (strong) | `#33405A` | Mist Strong `#5A6C7E` |
| `--orbit-text` (ink) | `#E9ECF1` | Mist Ink `#131B28` |
| `--orbit-muted` (secondary) | `#A7AFBD` | Mist Muted Ink `#3A4552` |
| `--orbit-faint` (tertiary) | `#6A7385` | Mist Tertiary `#5E6875` |
| `--brad-gold` (accent) | gold.500 `#C9A227` | gold.300 `#6C5A1F` |
| `--brad-gold-light` | gold.700 `#E4D093` | gold.400 `#9A7E23` (fills only) |
| `--brad-gold-dim` | gold.400 `#9A7E23` | gold.200 `#3D351B` |
| `--brad-link-border` | orbit.faint `#6A7385` | border.strong `#5A6C7E` |

Everything else — `--gen1`, `--live`, `--vision`, `--ref`, `--danger`, the
`--status-*` set, the `--era-*` ramp — is in the same generated block. Read the
file; it is the source.

### 2.2 The gold ramp, and why the light theme goes *down* it

```
gold.100 #0E1117   gold.200 #3D351B   gold.300 #6C5A1F   gold.400 #9A7E23
gold.500 #C9A227   gold.600 #D6B95D   gold.700 #E4D093   gold.800 #F2E8C9
```

On Orbit Black the accent is gold.500 (`#C9A227`, **7.81:1**). On Platinum Mist
it is gold.300 (`#6C5A1F`, **4.98:1**), because gold.400 measures **2.89:1** on
that canvas — under the 3.0 floor for a non-text boundary, let alone text.
Emphasis moves *darker* on light and *lighter* on dark; it never moves toward
the canvas.

gold.400 is allowed on Mist **as a fill only** (ink-on-gold measures 5.09:1).
Using it as text on Mist is the defect the measurement exists to prevent.

### 2.3 Contrast floors, and what has to pass

| Role | Floor | Why |
|---|---|---|
| Body and meta text | 4.5:1 | WCAG AA, normal text |
| Large display text | 3.0:1 | WCAG AA, ≥24px |
| Boundaries, icons, 3px rules | 3.0:1 | WCAG AA non-text |
| Decorative tints, alpha washes | — | they carry no information |

Every pair is measured by `src/bradium/tools/validate_tokens.py` — 16 pairs per
theme, 0 errors at adoption. Adding a colour means adding its pairs there, not
asserting that it looks fine.

### 2.4 The console exception

The terminal is a **console**, not a themed surface. `.terminal` pins its own
surfaces (`#0E1218`, `#1E2530`, `#9AA7BC`, `#5F6C81`) in both themes, and takes
its gold from the dark brand layer so the banner never inherits the light
theme's deep ink. Its frame (`#0B0E13`) is darker than either canvas on purpose:
it is a screen inside the page, not a panel of it.

### 2.5 Status and era colour

The status set resolves from the brand's status primitives — never from a
framework's defaults:

| State | Dark | Light |
|---|---|---|
| shipped | `#3DDC97` | `#0F5C35` |
| in progress | `#FFC857` | `#7A4E00` |
| designed | `#7EB3FF` | `#1749C6` |
| queued | `#A7AFBD` | `#3A4552` |
| stalled | `#FF7B72` | `#B42318` |

The journal timeline's five eras walk **up the gold ramp** in both themes
(dark: gold.300 → gold.700; light: gold.200 → gold.600). The earliest era is the
deepest gold, the shipped era the brightest. Chronology as a gradient of
maturity, no second hue family, no colour picker.

---

## 3. Type — normative

Two families, two jobs, no third.

| Voice | Family | Carries |
|---|---|---|
| Machine | `--font-mono` (ui-monospace stack) | Kickers, tags, receipts, paths, code, counts, terminal |
| Editorial | `--font-sans` (system stack) | Headlines, prose, navigation, buttons |

The mono voice is the brand's honesty tell: if a thing is a **receipt** — a
path, a count, a state, a command — it is set in mono, uppercase for labels,
with `.16em` tracking. Prose is never mono. Receipts are never sans.

**The scale (normative from this document).** Sizes resolve from one ladder:

| Step | Value | Used for |
|---|---|---|
| `--text-2xs` | 11px | badge and chip labels |
| `--text-xs` | 12px | kickers, mono meta, table headers |
| `--text-sm` | 13px | card meta, timeline rows, captions |
| `--text-md` | 15px | buttons, nav, UI copy |
| `--text-base` | 1.06rem (17px) | ledes, long-form body |
| `--text-lg` | 1.25rem (20px) | sub-heads, pull quotes |
| `--text-xl` | `clamp(1.8rem, 4vw, 2.7rem)` | section `h2` |
| `--text-hero` | `clamp(2.6rem, 7.2vw, 5.2rem)` | the home `h1` |

Headlines track tight (`-.02em` to `-.035em`); body does not. Prose measure is
**62–74ch**, and ledes cap at **720px**. A line longer than 78ch is a layout
bug, not a taste.

**Honest status:** the stylesheet still carries **26 distinct font sizes** —
`.68rem` through `1.6rem` — many within 1–2px of each other. The ladder above is
Normative; its adoption is §9.1. Until then, the rule is: any rule you touch
moves to the nearest ladder step, and a deliberate deviation keeps a comment
saying why.

---

## 4. Space and grid — normative

**The step is 8.** Spacing values are multiples of 4 for tight pairs (4, 8, 12),
multiples of 8 upward (16, 24, 32, 48, 64, 96). A `37px` padding is a typo with
ambition.

| Token | Value | Role |
|---|---|---|
| container | 1160px | `.container` max width |
| gutter | 24px | horizontal page padding |
| section rhythm | 96px desktop / 64px ≤720px | `.section` |
| card padding | 24–32px | `.card`, `.stat`, `.receipt` |
| radii | 6px / 8px / 10px | inner / control / panel |

Breakpoints are **560 / 720 / 1000** and nothing else. A fourth breakpoint is a
symptom: the component is trying to do two jobs.

Grid: 12 columns conceptually, expressed as `.grid-2 / .grid-3 / .grid-4` — one
set of gaps, one collapse rule per breakpoint. Cards do not invent their own
column behaviour.

---

## 5. Components and page patterns

The components exist to make evidence visible. That is the design language —
*receipts are the layout*. A component that only decorates is not in this list.

| Pattern | Rules |
|---|---|
| **Door header** (`.page-hero`) | kicker (mono, gold) → `h1` (hero step) → lede (meas 62–74ch). One rhythm, every door. §11.6 records the doors that improvise. |
| **Card** (`.card`, `.stat`, `.receipt`) | surface `--orbit-black-2`, 1px `--orbit-border`, radius 10px, padding 24–32. A card that needs a shadow needs a hierarchy reconsidered. |
| **Badge / tag** (`.badge`) | mono, 11px, uppercase, one of the status/gen vocabularies. A badge never invents a state — the vocabulary is `SHIPPED / DESIGNED / VISION / RECORD / PROPOSAL / PLAN / POLICY / GEN n`. |
| **Terminal** (`.terminal`) | the console exception (§2.4); the ASCII mark is generated and gate-checked (`tools/gen_ascii.py`, `tools/check_ascii.py`). Never redraw the mark by hand. |
| **Doc index** (`.doc-index-*`) | server-rendered from `generated by tools/gen_doc_index.py`; each row quotes the document's **own** state line; filter JS is enhancement only. A row with no declared state says so. |
| **Stat wall** (`.stat`) | every number comes from `site/assets/facts.json`. A hand-typed count is a future red gate. |
| **Footer** (`.footer`) | wordmark, the canonical link set, the license line. It is a receipt, not a sitemap dump. |

---

## 6. Themes — and the parity contract

Dark Matter is the default. Light Matter (`[data-theme="light"]`) is the Mist
family, measured. Both ship on every door, or the door is broken.

**Every door must do all five:**

1. **Init before paint** — the inline `<head>` script reads `localStorage.bdTheme`
   and sets `data-theme` on `<html>` before the body renders. No flash.
2. **One toggle, one key** — `site/js/app.js` owns the click and writes
   `bdTheme`. A page-local handler that also flips the theme will cancel the
   shared one (`--brad-theme` did exactly that on `/glossary` and `/press`).
3. **`theme-color` follows the canvas** — `#0E1117` dark, `#D9DEE5` light.
4. **The favicon follows too** — `icon-white.svg` on dark, `icon-black.svg` on
   light. A white glyph on a light tab rail is invisible, which is what the
   Architect's phone caught first.
5. **Every image that carries brand ink swaps** — nav icon, nav wordmark, the
   "By BradVerse" mark, the **hero lockup**, the footer wordmark, the deck. The
   hero swap was the one missing rule that shipped a white-on-white wordmark;
   `content: url(...)` in the light block is where that lives.

Theme parity is a **gate, not a pass** — the wordmark bug was found by a phone,
not by the build. §11.9 owes the screenshot matrix that would have caught it.

---

## 7. Motion

GSAP 3.13.0 + ScrollTrigger 3.13.0 + Lenis 1.3.11, **vendored** and hash-gated
(`site/vendor/MANIFEST`). No CDN, no font service, no third-party script without
a hash — integrity and performance are brand properties.

Contract:

- Motion may reveal, never obscure: content is in the DOM and readable before
  the script runs.
- Reveal is ≤ 0.6s, translate ≤ 24px, and never stacks (three elements entering
  in sequence is a queue, not a flourish).
- `prefers-reduced-motion: reduce` removes reveal and parallax entirely; the
  page must be complete without them.
- No motion carries meaning. If the only way to read a state is an animation,
  the state is broken.

---

## 8. Content inside the layout

- **A claim and its tag travel together.** `SHIPPED` / `DESIGNED` / `VISION`
  sits in the same visual block as the claim, in mono. Never on another page.
- **Counts are generated.** `site/assets/facts.json` → pages. If a page needs a
  number that is not in `facts.json`, add it to the generator, not the markup.
- **The honesty surface is designed, not tolerated.** The `/docs` index prints
  "this document declares no state" rather than assigning one; the deck prints
  what is unbuilt. That is the brand working as intended, and the layout must
  not soften it.
- **Voice is a gate.** `VOICE.md` + `tools/check_voice.py` runs before every
  deploy. Banned vocabulary never reaches a door, no matter how good the
  sentence.

---

## 9. Adoption ledger — what this document still owes

1. **Type ladder adoption** (§3): 26 sizes → 8 steps. Mechanical, page by page,
   with a before/after screenshot per door. Nothing else in this list starts
   until it does, because every later layout change rides on it.
2. **Docs index, desktop** — 135+ rows in one column. Group nav + two columns +
   sticky group headings at ≥1000px. Phone is already fine.
3. **Door-header pattern** — one component, applied to all fourteen doors.
4. **Theme-parity matrix** — both themes × fourteen doors × five rules (§6),
   captured and stored as a receipt.
5. **Display-face decision** — whether the headlines get a real face of their
   own (self-hosted, licensed) or stay on the system stack. A brand decision,
   not a font pickup; it is The Architect's.
6. **Light-mode social assets** — `og.png`, the Medium covers and the LinkedIn
   receipts are dark-only. Either that is the strategy (write it down) or the
   light counterparts exist.
7. **BradOS brand kit** — boot splash, window chrome, installer art, built from
   the same tokens before the first product surface ships.

---

## 10. The gates that defend this document

| Rule | Gate |
|---|---|
| The palette resolves from the brand tokens | `tools/gen_site_tokens.py --check` |
| No hand-typed gold, no retired colour, no undefined `var()` | `tools/check_site_css.py` |
| The token source itself is valid, contrast measured | `src/bradium/tools/validate_tokens.py` |
| Every door's links resolve | `tools/check_site_links.py` |
| One ASCII mark, every consumer | `tools/check_ascii.py` |
| The doc index is current | `tools/gen_doc_index.py --check` |
| Counts are current everywhere they are quoted | `tools/gen_facts.py --check` |
| Voice and publication vocabulary | `tools/check_voice.py`, `tools/check_publication_trigger.py` |

All of them run in `tools/build_site.sh` and `tools/run_gates.sh`.

---

## 11. Known debt — measured, not hidden

1. **`--orbit-faint` measures 3.96:1** on Orbit Black. It is used for tertiary
   meta, not body copy, but 3.96 is under the 4.5 floor for any text. Decision
   owed: raise the primitive, or restrict it to non-text.
2. **Mist tertiary measures 4.18:1** on Platinum Mist — the same class of debt
   on the light side.
3. **The deck's `--line` measures 1.82:1** on Orbit Black. It is a decorative
   hairline between slides, but it is also the only separator the deck has.
4. **26 ad-hoc font sizes** (§3) — the ladder is normative, the CSS has not
   adopted it yet.
5. **The docs index is one column** at every width (§9.2).
6. **Door headers improvise** — `h1` sizes and lede measures vary between
   doors (§9.3).
7. **`.nav` is 97% opaque** (`rgba(12, 15, 20, .97)`), so hero gold can read
   through it at the wrong scroll position, and **`.hero .kicker` is absolutely
   positioned** at the top of the hero where the fixed nav sits. Both flagged in
   the session review and awaiting The Architect's word, because they change
   composition, not colour.
8. **No browser on this box** — the arm64 chromium crashes, so nothing here is
   verified by screenshot from my side. The phone is currently the only visual
   QA, which is why §9.4 exists.
9. **`.terminal` surfaces are four hand-picked values** (`#0E1218`, `#1E2530`,
   `#9AA7BC`, `#5F6C81`) that are not on any ramp. They are the console
   exception (§2.4), deliberate, and documented rather than tokenised.

---

*This document is internal. `site/.vercelignore` keeps it out of the deployed
tree — it describes the site, it is not a page of it. If it should become
public, that is a publication decision, and `COMPANY/IP_PUBLICATION_POLICY.md`
is the door.*
