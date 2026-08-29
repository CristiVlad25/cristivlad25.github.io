# CLAUDE.md

Conventions for this repository. Follow them exactly - consistency across pages is
the point of this file.

## What this is

A GitHub Pages site publishing interactive explainers: things you can manipulate in
order to understand them, across biology, cybersecurity, and AI. Live at
https://cristivlad25.github.io

This is a **user site** (`cristivlad25.github.io` repo), so the site root maps to the
repo root and all internal links are root-relative: `/bio/thing.html`, never
`bio/thing.html` or `./bio/thing.html`.

## Structure

```
/
├── index.html          hub page - the only page listing artifacts
├── CLAUDE.md           this file
├── .nojekyll           disables Jekyll; do not delete
├── bio/                pharmacology, physiology, medicine
├── cyber/              security, protocols, internet standards
└── ai/                 architectures, results, and the systems that serve them
```

There are exactly three sections. Do not invent a fourth without being asked.

## Two genres

Every page is one of these. Decide which before writing, and say so on the page.

**Source reading** - one document is the anchor: a paper, an RFC, a specification.
The value is in separating what the document actually supports from what people
believe it supports. Cite tightly; there is something specific to cite.

**Synthesis** - no single anchor. How a system works end to end, how a class of
attack works, what a subsystem does. Assembled from many sources. The value is in
the assembly, and a reference list at the end serves the reader better than an
inline citation on every sentence.

Both are first-class. A section is not a genre - `cyber` holds RFC readings *and*
broad security syntheses, `ai` holds paper readings *and* infrastructure syntheses.

## Adding a new artifact

1. **Create the page** at `<section>/<slug>.html`. Slug is kebab-case, derived from
   the subject rather than a title - `tirzepatide-receptor.html`, not
   `pyke-2014-glp1r-immunohistochemistry.html`. Short and guessable.
2. **Insert the nav bar** as the first thing after `<body>`, with the section label
   and href matching the folder the file lives in. The page background must be
   `--ink` exactly, or the nav bar sits on a different colour and shows a seam.
3. **Add the entry** to `index.html` inside the matching `<section>`.
4. **Bump `.sec-count`**, and delete the section's `.empty` div if this is its first
   entry.

Steps 3 and 4 are the ones most easily forgotten. A page that exists but isn't linked
from the hub is invisible.

## Page requirements

- **Self-contained single HTML file.** No build step, no bundler, no npm, no JSX.
  All CSS in `<style>`, all JS in `<script>`, same file. Inline compiled Tailwind and
  UMD React builds rather than linking a CDN if the page needs them.
- **Progressive enhancement.** The page's headline, framing paragraph, and source
  list must exist as static markup and be readable with JavaScript off. Interaction
  layers on top of text that already exists; it does not supply the text. A body of
  `<div id="root"></div>` plus a `<noscript>` apology means empty link previews,
  nothing indexed, and no in-page find.
- **No external dependencies** beyond Google Fonts. Prefer writing a visualization
  directly over pulling a charting library.
- **Load fonts once**, via `<link>` in `<head>`. Do not also `@import` the same
  families from a runtime-injected stylesheet.
- **No localStorage or sessionStorage.** Keep state in memory.
- **Responsive to 380px.** Interactive controls must survive phone width.
- **Visible keyboard focus** on every interactive element; honor
  `prefers-reduced-motion`.
- **Head block, every page:** `<title>`, `<meta name="description">`, `og:title`,
  `og:description`, `og:type`.

## Naming and typography

Two dash rules that are easy to confuse. They are different characters doing
different jobs, and both are strict.

- **Never use an em dash (—) or an en dash (–). Anywhere.** Not in page copy, not in
  headings, not in code comments, not in this file. Use a plain hyphen `-` instead,
  including for parenthetical breaks and numeric ranges. If a draft contains one,
  replace it before committing. This applies to text written by a model as much as
  text written by hand, and it is the single most frequently violated rule here.
  Grep for the escaped forms too - `—` and `–` inside a JS string render
  as dashes and are invisible to a search for the character itself.
- **The middle dot `·` separates a title from its subtitle**, and is the one piece of
  non-ASCII punctuation the site uses: `Tirzepatide · Dual-Channel Receptor Atlas`.
  Not a hyphen, not a colon. It also separates items in a mono anchor or scope line:
  `Training · Serving · Cost`. Keep it.

So: `LLM Infrastructure Explorer · from a web crawl to a word on your screen`, and
inside the prose, "the router picks a region - geography, capacity, residency - then
hands off".

- Sentence case for headings.

## Design tokens

```
--ink:#0B0E13   --panel:#10141B   --rule:#1F2630   --rule-lit:#39434F
--text:#E6E9ED  --dim:#7D8794
--bio:#4FD1A5   --cyber:#6D9BFF   --ai:#B98CFF
```

Type: **Newsreader** (300/400) is the hub display face and appears nowhere else. An
artifact's own `h1` uses **IBM Plex Sans Condensed** (600/700). Body copy is **IBM
Plex Sans**; **IBM Plex Mono** carries labels, data, anchors, and anything
letterspaced in caps. No other families.

Section accent is determined by folder; a bio page never borrows another section's
color. Inside an artifact, a working palette for data encoding is fine where the
subject demands it, but nav, rules, and body text stay on these tokens.

## Editorial standards

### Both genres

- **Say which genre the page is**, in the opening framing. A reader should know
  immediately whether they're getting a close reading or an assembled overview.
- **Distinguish hard numbers from representative ones.** Datasheet specs, published
  prices, and measured results are checkable and should read as checkable - name the
  source inline or in the reference list. Numbers chosen to make an example concrete
  are illustrations and should be visibly marked as such. Mixing the two without
  distinction is the failure mode that matters, because the reader cannot tell them
  apart on their own. 
- **Don't invent specifics a reader will go looking for.** Internal codenames,
  product families, distillation lineages, per-token prices, percentage attributions
  for a company's internal gains. If an example needs a name, make it obviously
  generic. A hypothetical future version used as a framing device is fine as long as
  the page says it's hypothetical.
- **State limits.** If the material doesn't support a claim, don't make it. Absence
  of evidence gets its own treatment, not silence.
- **Show the formula (on hovering).** Where the page computes something - cost, cache size,
  speedup, goodput, blast radius - show the expression on screen (on hovering) in a mono block below
  the control. An interactive number with a hidden formula is an assertion wearing
  a costume.
- **No medical advice.** Biology pages describe mechanism. Dosing, safety, and
  treatment decisions belong with a clinician, and the page says so.

### Source readings additionally (in a dropdown toggle v arrow)

- Cite the anchor document properly: author-year with DOI or arXiv link; RFC number
  and section with a datatracker link.
- **Mark the evidence tier in the interface**, using whatever scheme the material
  demands. For pharmacology that has been human protein/function · human transcript ·
  rodent only. For standards it is MUST · SHOULD · MAY, plus the gap between what the
  standard mandates and what implementations do. The scheme is per-page; the
  requirement that one exists is not.
- Where popular understanding rests on evidence the document doesn't actually
  provide, say so at the point the claim appears.

### Syntheses additionally (in a dropdown toggle v arrow)

- **A reference list at the end**, naming the sources the page actually rests on:
  papers, specs, vendor documentation, project repositories. Not exhaustive -
  the ones carrying weight.
- **A scope note**, top and bottom. The top one tells the reader what kind of page
  this is; the bottom one states what the numbers represent and don't.
- Techniques with a known origin get named with it - PagedAttention, FlashAttention,
  GQA, GRPO, continuous batching all came from somewhere, and naming the origin costs
  one line and adds real value for a reader who wants to go deeper.

Copy is plain and active. No hype, no "revolutionary," no throat-clearing. Titles
specific over clever.

## Copyright

**The site is all rights reserved.** There is no `LICENSE` file, which under default
copyright means no one may reproduce, distribute, or create derivative works. The
`index.html` footer must therefore carry a plain copyright line and **must not**
offer a Creative Commons license - a CC notice in the page is a real grant even
without a LICENSE file, and would contradict the repo.

If this changes, both have to change together: add the LICENSE file and the footer
notice in the same commit.

Regardless of the site's own license:

- **Redraw every figure.** Never embed or reproduce figures from papers; they are the
  publisher's or authors' copyright.
- **Do not reproduce RFC text wholesale.** IETF terms permit redistribution but
  restrict derivative works. Explain in original words; quote only short normative
  fragments where exact wording carries meaning.
- Quote sparingly and briefly from any source, and attribute it.

## index.html conventions

> Filtering will likely replace rigid section grouping once entry count grows. Until
> then, the structure below is authoritative.

```html
<a class="entry" href="/SECTION/SLUG.html">
  <div class="src">ANCHOR</div>
  <div>
    <h2>Title · Subtitle</h2>
    <p>One sentence on what the reader can actually do or learn here.</p>
  </div>
</a>
```

The left column is **the anchor** - what this page is pinned to. Its content depends
on genre:

- **Source reading:** the citation. `Pyke 2014` / `Willard 2020`, `RFC 9110`,
  `arXiv:1706.03762`.
- **Synthesis:** the scope, in the same mono style. `Training · Serving · Cost`,
  `Kernel · Userland · Network`. Three words maximum, tightest first.

Never a vague placeholder. A company name plus a year plus "others" is neither a
citation nor a scope - if the page is a synthesis, describe what it covers instead.

`<h2>` matches the page's `<title>`. `<p>` is one concrete sentence about what's
interrogable, not a summary of the subject.

## Before committing

Static files only - no package.json, build config, workflow, or anything needing
compilation. Then check; each of these has shipped broken at least once:

- Grep the diff for `—`, `–`, `—`, `–`. Any hit is a bug.
- Load the page with JavaScript off. Headline, framing, and sources must be there.
- Head block complete: `<title>`, description, `og:title`, `og:description`, `og:type`.
- The index `<h2>` is character-for-character the page's `<title>`, and the entry
  exists at all. Page background is `--ink`.
