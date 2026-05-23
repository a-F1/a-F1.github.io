---
name: write-paper-blog
description: >-
  Author a bilingual (English + Chinese) project-style blog post about a research paper in this Jekyll site, using the `project` layout. Use when the user asks to write, draft, or edit a post under `_posts/` about a paper, to convert a paper README into a blog, or to add the EN/ZH language toggle, multi-panel figure grid, MathJax, or BibTeX block used in `_posts/2026-05-15-pion.md`.
---

# Writing Paper Blogs (a-F1.github.io project layout)

This skill captures the conventions established while shipping `_posts/2026-05-15-pion.md`. It applies to any new post that should reuse the project layout (custom header with author/affiliations/links, side TOC, EN/ZH toggle, multi-panel figure grid, MathJax, BibTeX).

## When to apply

- Drafting a new `_posts/YYYY-MM-DD-<slug>.md` summarizing a paper.
- Editing an existing project-style post (TL;DR, Method, Experiments).
- Adding the EN/ZH toggle, figure grid, table-scroll, or BibTeX block to an existing page.

Skip this skill for plain `single`-layout pages, `_pages/*.md`, or non-paper posts.

## Workflow checklist

```
- [ ] Create _posts/YYYY-MM-DD-<slug>.md with the front-matter template
- [ ] Wrap content in <div class="post-lang post-lang-en" markdown="1">...</div>
      and a parallel post-lang-zh block (hidden markdown="1")
- [ ] Section order: TL;DR -> Try It -> Background -> Motivation -> Method -> Experiments -> BibTeX
- [ ] Every figure / table / video has a unique id; ZH copies use id="<x>-zh"
- [ ] All in-text mentions of figures / tables / videos are markdown links to those ids
- [ ] BibTeX block <pre class="bibtex-block"><code>...</code></pre> is identical in EN (id=bibtex) and ZH (id=bibtex-zh)
- [ ] Run bundle exec jekyll build; check _site/posts/<slug>/index.html
```

Always polish the Chinese version first to lock semantic structure, then sync English. Keep section count, figure numbering, and proposition order identical between languages.

## File layout

- Posts: `_posts/YYYY-MM-DD-<slug>.md`. Use `layout: project`, NOT `single`.
- Images: `images/blog/<slug>/`. Videos / GIFs: `videos/blog/<slug>/`.
- Layout source of truth: `_layouts/project.html` (CSS, language toggle JS, TOC builder, MathJax retypeset).
- Canonical example: `_posts/2026-05-15-pion.md` — read before drafting.

## Front matter template

```yaml
---
layout: project
title: "Paper Title (no leading code name)"
date: YYYY-MM-DD
permalink: /posts/<slug>/
author_profile: false
read_time: false
share: false
comments: false
related: false
authors:
  - name: "Author One"
    url: "https://example.com/"
    affiliation_id: 1
  - name: "Author Two"
    affiliation_id: "1,2"
affiliations:
  - id: 1
    name: "Institution One"
  - id: 2
    name: "Institution Two"
venue: "Preprint, YYYY"
links:
  - { label: "arXiv",  url: "https://arxiv.org/abs/...", icon: "📄" }
  - { label: "Code",   url: "https://github.com/...",   icon: "⌨︎" }
  - { label: "BibTeX", url: "#bibtex",                  icon: "❝" }
tags: [tag1, tag2]
excerpt: "1-2 sentence pitch shown on /blog/."
---
```

The `links[].url: "#bibtex"` value is rewritten on the fly by `_layouts/project.html`'s `setLang(lang)` to `#bibtex-zh` when ZH is active. Leave it as `#bibtex`.

## Bilingual structure

```html
<div class="post-lang post-lang-en" markdown="1">
[ ... full English content here ... ]
</div>

<div class="post-lang post-lang-zh" hidden markdown="1">
[ ... full Chinese content here, mirroring EN structure ... ]
</div>
```

EN is shown by default. Toggle, TOC rebuild, MathJax retypeset, and BibTeX-anchor swap are all wired in `project.html`. Do not add per-post JS.

### Section skeleton (used in both languages)

```markdown
<div class="tldr" markdown="1">
## TL;DR
* …
</div>

## Try It: <one-line interactive demo question>
<!-- optional interactive widget; can be omitted -->

## Background
### <Concept 1>
### <Concept 2>

## Motivation
### Beyond <axis 1>: <Setting 1>
### Beyond <axis 2>: <Setting 2>

## Method
### <Method component 1>
### <Method component 2>
### Algorithms
<!-- side-by-side pseudocode -->

## Experiments
### <Setting 1>
### <Setting 2>

<h2 id="bibtex">BibTeX</h2>
<pre class="bibtex-block"><code>@misc{...}</code></pre>
```

The ZH copy uses `<h2 id="bibtex-zh">BibTeX</h2>`, `## TL;DR` becomes `## TL;DR`（保留英文标题），and section titles get translated. Keep section ORDER identical to EN.

## Figures, tables, videos

### Multi-panel figure (canonical)

```html
<div id="fig-N" class="fig-grid"
     style="grid-template-columns: minmax(0, 2fr) minmax(0, 1fr); max-width: 90%; margin: 0.6em auto 0.05em; padding: 2px 10px 2px; row-gap: 0;">
  <div class="fig-legend" style="margin-top: 0; margin-bottom: -10px;">
    <img src="/images/blog/<slug>/legend.png" alt="Legend">
  </div>
  <figure style="min-width: 0; height: auto; justify-content: flex-start; margin: 0;">
    <img src="/images/blog/<slug>/panel_a.png" alt="…"
         style="width: 100%; height: auto; max-height: none; max-width: 100%; display: block; margin: 0;">
    <figcaption style="white-space: normal; margin: 0;">(a) …</figcaption>
  </figure>
  <figure style="min-width: 0; height: auto; justify-content: flex-start; margin: 0;">
    <img src="/images/blog/<slug>/panel_b.png" alt="…"
         style="width: 100%; height: auto; max-height: none; max-width: 100%; display: block; margin: 0;">
    <figcaption style="white-space: normal; margin: 0;">(b) …</figcaption>
  </figure>
</div>
<p class="fig-caption">Figure N. <full caption>. (a) … (b) …</p>
```

Invariants you must keep:

- Use `minmax(0, Xfr)` and `figure { min-width: 0 }` so wide images don't blow out the grid track.
- Use `width: 100%; height: auto; max-height: none` on `<img>` to avoid `object-fit: contain` letterboxing (the cause of phantom vertical gaps).
- Subcaptions must be short. If they overflow the column, shorten the text rather than removing `white-space: nowrap` globally.
- ZH copy uses `id="fig-N-zh"` and `<p class="fig-caption">图 N. …</p>`.

### In-text references (every mention is a hyperlink)

```markdown
[Figure 1](#fig-1)(a) shows ...
[Table 1](#tab-1) summarizes ...
[Video 1](#video-1) shows the rollout.

<!-- ZH -->
[图 1](#fig-1-zh)(a) 表明 ...
[表 1](#tab-1-zh) 给出 ...
[视频 1](#video-1-zh) 给出 rollout。
```

### Wide tables

Wrap with `<div class="results-table" markdown="1">…</div>`. The layout JS auto-wraps the inner `<table>` with `.table-scroll` so it scrolls horizontally instead of overflowing the content column.

### Side-by-side algorithms

Wrap pseudocode blocks in `<div class="algo-row">…</div>`. Highlight diffs via `<span class="lg-item"><span class="swatch s-1"></span> A vs B</span>` legend chips. The CSS supports horizontal scroll on narrow viewports — do not break it by setting `overflow: hidden`.

### Video / GIF panels

Use the same `<div class="fig-grid">` pattern with `<video src="…" autoplay loop muted playsinline>` or `<img src="….gif">` inside each `<figure>`. Place this block AFTER the experimental table it illustrates, not before.

## Math conventions

- Use `$$...$$` for both inline and display math (kramdown handles both).
- Bold symbols: `\mathbf{...}`. This renders correctly inside Chinese text too.
- Do NOT add a `<script>MathJax...</script>` block per post; `setLang(lang)` already calls `MathJax.typesetPromise` after toggle.

## BibTeX block

```html
<h2 id="bibtex">BibTeX</h2>
<pre class="bibtex-block"><code>@misc{key,
      title={…},
      author={… and … and …},
      year={YYYY},
      eprint={NNNN.NNNNN},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/NNNN.NNNNN},
}
</code></pre>
```

- EN heading id is `bibtex`; ZH heading id is `bibtex-zh`.
- Keep BibTeX content **identical** in both blocks.
- `setLang` swaps the header BibTeX button anchor (`href="#bibtex"` ↔ `#bibtex-zh`) automatically.

## Writing-style rules

### Both languages

- **No dashes (`—`, `--`, `——`).** Use commas, semicolons, or recast as separate sentences.
- Refer to figures / tables / videos by hyperlink; do not narrate code or restate captions in prose.
- Put proposition / definition statements first, then formula, then prose.
- Keep EN and ZH structurally aligned: same sections, same figure numbers, same proposition order.
- After significant ZH edits, re-read EN and adjust phrasing to mirror the polished ZH logic.

### English

- Avoid leaky internal nouns ("our axis 1", "axis 3"). Promote concrete names: "modality / loss", "learning paradigm".
- Prefer "comprehensively outperforms", "substantially reduces", "without exception" over "consistently" / "clearly" when describing experiment results.
- Parallel subcaption phrasing: e.g. `(a) Success rates on LIBERO`, `(b) Success rate vs. steps on Object`.

### Chinese

- 不要太口语。"放大 / 缩小"，不要"推到"；"benchmark 为 …"，不要"benchmark 选 …"。
- 专有名词用英文：`setting`, `head`, `benchmark`, `loss`, `learning paradigm`, `modality`, `vision encoder`, `language backbone`, `action head`, `flow matching`, `regression`, `Gaussian sketching`, `rollout`, `episode`, `instance`, `erank`。
- 评估词区分语境：VLA 用 **成功率**，RLVR 用 **准确率**，禁用"精度"。
- 数值波动用"大幅"、"全面"、"明显"，避免一句话内重复"明显"。
- 检查"对于"是否多余：经常把"对于 X 的 Y"改成"对 X 的 Y"或直接"X 的 Y"。
- 替换易错搭配："至此 + 都是" → "目前为止，都将"；"等列" → "等几项"；"在的 LIBERO" → "在 LIBERO"。
- 避免逗号粘连长句：长句优先拆成两句而不是堆叠分号。

## Common pitfalls and fixes

| Symptom | Cause | Fix |
| --- | --- | --- |
| Figure column ratio not 2:1 even with `2fr 1fr` | Wide image's intrinsic width inflates the column track | Use `minmax(0, 2fr) minmax(0, 1fr)` + `figure { min-width: 0 }` |
| Tall vertical gap between image and figcaption | `object-fit: contain` letterboxes inside `height: Npx` box | Use `width: 100%; height: auto; max-height: none` on `<img>` |
| Subcaption pokes out of figure box | `.fig-grid figcaption { white-space: nowrap }` clips long text against `overflow: hidden` ancestors | Shorten subcaption text first; only as last resort override per-figure to `white-space: normal` |
| Legend "squished" after edits | `align-items: stretch` on grid + `line-height: 0` on legend collapses its flex container | Keep default `align-items: end`; control gap with `row-gap` and legend `margin-bottom` |
| ZH BibTeX button does not scroll | Anchor still points at `#bibtex` (hidden EN heading) | Confirm both `<h2 id="bibtex">` and `<h2 id="bibtex-zh">` exist; `setLang` does the rest |
| Math not rendering after toggle | New math added without retypeset | `setLang` already retypesets; if regression, restore the `MathJax.typesetPromise` call in `project.html` |
| `GitHub Metadata: No GitHub API authentication` warning | Missing GH token, harmless | Ignore; build still succeeds |

## Build and verify

```bash
# Live preview
bundle exec jekyll serve --livereload --port 4000
# Visit http://127.0.0.1:4000/posts/<slug>/

# One-shot
bundle exec jekyll build
# Output at _site/posts/<slug>/index.html
```

After every batch of edits, rebuild and confirm the EN and ZH blocks render identical structure (same figure numbering, same section count, parallel propositions).

## Companion: news entry on the home page

In `_pages/about.md`, add to the top of `<div class="news-container">`:

```html
<div class="news-item">
    <div class="news-date">YYYY.MM</div>
    <div class="news-content">
        ✨ One first-author paper <a href="https://arxiv.org/abs/...">CodeName</a>, a short description, on arXiv! See its <a href="/posts/<slug>/">blog</a>.
    </div>
</div>
```

Keep it short, parallel to existing entries (e.g. `SimNPO`, `LLM Unlearning Bench`, `CyclicReflex`).

## Files to read on demand

- `_posts/2026-05-15-pion.md` — the canonical example, read before drafting a new post.
- `_layouts/project.html` — CSS / JS / TOC source of truth, read when something renders unexpectedly.
