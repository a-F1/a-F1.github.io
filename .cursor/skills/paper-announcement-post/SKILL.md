---
name: paper-announcement-post
description: >-
  Draft research-paper announcement posts for LinkedIn, X, and 小红书 (Xiaohongshu)
  in Chongyu Fan's voice. Use when the user asks to write, draft, or polish a
  social-media post / advertisement / announcement for one of their own papers,
  blog posts, or preprint releases, especially when LinkedIn, X, Twitter,
  小红书, 公众号, or "paper post" / "广告" / "宣传" is mentioned.
---

# Paper Announcement Post

Helps Chongyu Fan (the user) draft an English and Chinese announcement post
when releasing a new paper / preprint / blog. The voice is **first person**
("I am excited to share our latest paper..."), modeled after Sijia Liu's
LinkedIn release posts but presenting Chongyu's own work.

## Inputs to gather

Before drafting, make sure you have:

1. Paper title.
2. arXiv link, blog link, code (GitHub) link.
3. 1–2 sentences on the **research question / motivation**.
4. The **key findings / failure modes / observations** the paper identifies.
5. The **method** name + 1–2 sentence summary.
6. Headline **experimental results** (concrete numbers preferred).
7. Advisor name + collaborator names with affiliations.

If anything is missing, ask the user before drafting.

## Output structure

The EN LinkedIn / X version and the ZH 小红书 version use DIFFERENT default
structures because the platforms render and rank content differently.

### LinkedIn (English) — 7-section layout

Use these section emoji headers in this exact order. Each header is a single
line with one emoji + a short title. Body of each section uses `▸` bullets.

```
🚨 [opener] paper title + 3 links (Paper / Blog / Code)

⚠️ The Question
▸ background sentence
▸ the research question

🔍 Key Findings
[one short framing sentence]
▸ failure mode 1 (with axis labels in parentheses)
▸ failure mode 2 (with axis labels in parentheses)

💡 Method
[one-line method intro]
▸ key mechanism
▸ design variants / modes

✅ Results
▸ headline result 1 (with concrete numbers)
▸ headline result 2

📌 Takeaway
▸ why old approach fails
▸ what new approach achieves

🤝 Acknowledgements
[advisor + collaborators in one line]
```

Hashtags are off by default for the LinkedIn version (only add if the user
asks).

### 小红书 (Chinese) — 4-section compressed layout

Different from LinkedIn: the compressed default has 4 sections (🚨 / 🔍 /
💡 / 📊), uses plain `1.` `2.` numbers (not `▸`), no clickable URLs,
short single-noun section labels, no 🤝 / hashtag blocks.

```
🚨 hook line (single sentence + one-line answer)

🔍 背景
[2–4 sentence paragraph: prior method, why it works in the original
setting, why it fails when modality / loss / paradigm changes]

💡 主要贡献：
1. <场景> 的 <现象>（<axis>）
[1–3 sentence body]
2. <场景> 的 <现象>（<axis>）
[1–3 sentence body]
3. 提出 <方法>：<one-line>
[1–3 sentence body]
4. <变体名>：<功能>
[2–3 sentence body — the variant explanation may be slightly longer]

📊 实验结果：
1. <场景 1>：<结果 + 关键数字>
2. <场景 2>：<结果 + 关键数字>

📄 论文 arXiv ID: <bare arxiv id>
💻 源码 GitHub 搜: <OWNER/REPO>
📝 项目主页搜: <domain/path without https>
```

Optional add-ons (only when the user explicitly asks): an affiliation
paragraph attached directly under the hook line; a 🤝 acknowledgements
block; a `#hashtag` line (placed last).

## Hard formatting rules

These are non-negotiable because LinkedIn / X / 小红书 do not render markdown.

Common to all platforms:

- Use Unicode emoji characters directly (🚨 ⚠️ 🔍 💡 ✅ 📊 📌 🤝). NEVER use
  GitHub / Slack shortcodes like `:rotating_light:` or `:warning:` — they do
  not render on LinkedIn / X / 小红书.
- Put exactly one blank line between sections. To get visibly larger spacing
  on LinkedIn, use two blank lines (LinkedIn collapses to one but renders the
  paragraph break).
- One bullet / numbered item = one paragraph. Each item starts on a new line.

LinkedIn (English) and X:

- Use the literal Unicode character `▸` as the bullet, NOT `-`, `*`, `•`,
  or `🔹`. (`-` and `*` look invisible after rendering; `▸` is the agreed
  default.)
- Keep the bullet symbol `▸` followed by a single space, then the body text.
- Emojis appear ONLY in section headers, not inside bullet bodies.
- Use original URLs (`https://arxiv.org/...`, `https://github.com/...`),
  NOT LinkedIn-shortened `https://lnkd.in/...` URLs. LinkedIn rewrites them
  itself after publishing.
- For acknowledgements on LinkedIn, prefix each name with `@` so the user can
  manually convert them to LinkedIn tags in the editor. Tell the user they
  must re-type `@Name` inside LinkedIn's composer to trigger the tag picker
  (plain `@` text does not auto-link).

小红书 (Chinese) — different conventions:

- Use plain `1. ` `2. ` `3. ` `4. ` for the numbered key contributions
  section (`💡`) and for the results section (`📊`). Do NOT use 1️⃣ 2️⃣
  3️⃣ 4️⃣ — the user finds the keycap emoji too informal. Do NOT use
  `▸` either. The `🔍 背景` section uses a plain paragraph with no
  bullet / number prefix.
- Body text should contain NO emojis. The only emojis allowed are: the 4
  section headers (🚨 🔍 💡 📊) and the link prefixes (📄 💻 📝).
  Do NOT use ✨, 🔹, 🤔, 🤝, 1️⃣–4️⃣, or any other body-emphasis emoji.
- Section headers are short labels, NOT questions:
  - `🔍 背景` (single noun preferred over the older `🔍 核心问题：<...>？`
    form; the older form is still acceptable when the question framing
    genuinely helps).
  - `💡 主要贡献：`
  - `📊 实验结果：`
  - Avoid the older longer forms like `💡 论文核心贡献：` /
    `📊 主要实验结果：`; trim to the short labels above.
- Item title forms under `💡`:
  - Problem-identification items (typically items 1 and 2): noun-led
    `<主题>（<axis 标签>）`. Example: `1. VLA 的失效根因（modality + loss）`.
    Do NOT prefix with `定位 / 揭示 / 分析` if the noun phrase already
    conveys the same meaning more compactly.
  - Proposal item (typically item 3): verb-led `提出 <方法>：<one-line>`.
    Example: `3. 提出 Pion：零成本谱高通替换`.
  - Variant / extension item (typically item 4): noun-led `<变体名>：
    <功能>`. Example: `4. Per-head 模式：保留 head 间异质性`.
- Block structure rule: a "block" is a section header + its content, or
  a numbered item title + its body. Within a block, do NOT insert blank
  lines — the body line follows the title line directly. Between blocks,
  use exactly one blank line.
  Concretely:
  - 🚨 hook line stands alone (compressed default, no affiliation
    paragraph). When an affiliation paragraph IS included, it attaches
    directly to the hook line with no blank line in between.
  - 🔍 background block: header line + paragraph, no blank line between.
  - Under 💡, each numbered item (`1. <headline>` + its body) is one
    block; the 💡 header line attaches to item 1 with no blank line.
  - Under 📊, each numbered result item is one block; the 📊 header line
    attaches to result 1 with no blank line.
  - Combine closely related results into ONE result block. Example: VLA
    simulation results and real-robot results belong in one block under
    a single `1. VLA：` headline; do not split them with a blank line
    or a separate number.
- Do NOT include raw external URLs (`https://...`). 小红书 actively filters
  or down-ranks posts with external links. Instead use:
  - `📄 论文 arXiv ID: 2605.xxxxx` (give the bare arXiv ID).
  - `💻 源码 GitHub 搜: OWNER/REPO` (give the GitHub path).
  - `📝 项目主页搜: domain/path` (give the domain path without `https://`).
- The first line is the only thing visible in the feed before "展开" — make
  it a single hook + a one-line answer. Two styles are acceptable:
  - Rhetorical question: `🚨 LLM 预训练之外，Muon 还适用吗？我们用一个零成本的"谱高通"给出了答案。`
  - Declarative-with-surprise: `🚨 Muon 在 LLM 预训练之外竟然会失效！我们提出"谱高通"优化器 Pion 作为解决方案。`
  The hook line may end in `？` `?` `！` or `。`. All other sentences in
  the post must end in `。`.
- The affiliation paragraph is OPTIONAL. The compressed default omits it.
  When included, it goes immediately under the hook with no blank line
  between, no leading emoji, and gives collaborating institutions plus a
  one-sentence positioning of the work.
- No 🤝 acknowledgements section by default. No hashtags by default.
  Only add either if the user explicitly asks.

## Voice and tone

LinkedIn (English):

- First person, present tense. Opening verb: `Excited to share`.
- Professional, not colloquial. Avoid filler like "super excited", "blown
  away", "this is huge".
- LLM pretraining / SFT / RL terminology is fine; the audience is ML
  researchers and practitioners.
- Compress aggressively. Each bullet should be one or two sentences. The
  whole post should fit in roughly 250–350 words.

小红书 (Chinese):

- First-person, professional. The first line is a hook + a one-line
  answer. Two hook styles are acceptable, pick whichever matches the
  paper's framing:
  - Rhetorical question style:
    `🚨 LLM 预训练之外，Muon 还适用吗？我们用一个零成本的"谱高通"给出了答案。`
  - Declarative-with-surprise style (use a mild emphasis word like
    `竟然` / `居然` / `失效` to flag the counter-intuitive finding):
    `🚨 Muon 在 LLM 预训练之外竟然会失效！我们用"谱高通"优化器 Pion 给出了解决方案。`
  Avoid the louder 自媒体 register:
  - ❌ Too loud: `🚨 大火的 Muon 一离开 LLM 预训练就翻车？我们用 ... 修好了它！`
- Hook punctuation: the hook line may end in `？` `?` `！` or `。`,
  whichever fits the chosen style. All OTHER sentences (background,
  numbered items, results) must end in `。`, not `！`.
- Mixed Chinese-English is encouraged for technical terms
  (`loss`, `modality`, `pretraining`, `attention head`, `setting`).
- Marketing vocabulary is allowed but heavily restricted: at most ONE
  light-emphasis phrase per post, used in the hook line only. Acceptable:
  `零成本`, `失效`, `竟然`, `居然`, `不再适用`. AVOID: `翻车`,
  `全军覆没`, `硬核亮点`, `长尾盲区`, `瞬时崩溃`, `打脸`, `行业级`,
  `惊人`, `大火的`.
- Numbered contributions under `💡` use the item-title forms specified
  in the formatting rules above: noun-led `<主题>（<axis>）` for
  problem-identification items, verb-led `提出 <方法>：<one-line>` for
  the proposal item, and noun-led `<变体名>：<功能>` for variant /
  extension items. Avoid prefixing every item with `定位 / 揭示 / ...`
  when the noun phrase is enough.
- Compress aggressively. The whole post should fit in roughly 1000
  Chinese characters total (including links). Compression knobs in
  priority order: (1) drop the affiliation paragraph; (2) drop benchmark
  detail numbers in `📊` while keeping a qualitative claim; (3) shorten
  item titles to noun phrases; (4) trim item bodies to 1–3 sentences.

## Do / Don't

Do:

- Mirror the EN and ZH versions one-to-one, section by section, when
  drafting both. (Note: the EN LinkedIn version uses the longer 7-section
  layout including ⚠️ / ✅ / 📌 / 🤝; the ZH 小红书 compressed default
  uses only 4 sections — 🚨 / 🔍 / 💡 / 📊 — so the one-to-one mirror
  applies to the technical content, not section count.)
- Include at least one concrete benchmark number in `✅ Results` for the
  LinkedIn version (e.g. `100% on LIBERO Object in 1,500 steps vs. 32.2%
  for AdamW`). For 小红书, benchmark numbers are encouraged but may be
  dropped in compressed mode as long as the qualitative claim survives.
- End the LinkedIn version with the 🤝 acknowledgements block. The 小红书
  version has no acknowledgements and no hashtags by default.
- Offer the user a short single-tweet variant for X if the post is long.

Don't:

- Don't use markdown headings (`#`, `##`) or markdown bullets (`-`, `*`).
- Don't use horizontal rules (`---`) — they render as plain text.
- Don't add `[image]`, `[figure]`, or other placeholder markers in the body.
- Don't paraphrase the paper title — keep it verbatim, in quotes.
- Don't introduce new claims not in the paper / blog. If a number is not
  given, omit it; do not estimate.

## Reference template (English, LinkedIn)

This is the canonical layout. Do not copy the wording, only the structure.

```
🚨 Excited to share our latest paper: "<Paper Title>"

▸ Paper: <arxiv url>

▸ Blog: <blog url>

▸ Code: <github url>



⚠️ The Question

▸ <one-sentence background on the prior method / setting>

▸ <one-sentence research question>



🔍 Key Findings

<one short framing sentence>

▸ <regime 1, with the axis it changes in parentheses>: <observation> + <why old method fails>

▸ <regime 2, with the axis it changes in parentheses>: <observation> + <why old method fails>



💡 Method

<one-line introduction of the method name + what it replaces>

▸ <core mechanism in one sentence>

▸ <design variants or modes; explain when each is needed>



✅ Results

▸ <result on setting 1, with concrete numbers; mention baselines beaten>

▸ <result on setting 2, with concrete numbers>



📌 Takeaway

▸ <why the prior approach is the wrong inductive bias>

▸ <what the new approach gives you, including any cost claims>



🤝 Acknowledgements

I am sincerely grateful to my advisor @<Advisor>, and to our collaborators @<Collab1> (<Aff>), @<Collab2> and @<Collab3> (<Aff>) for their valuable discussions and support.
```

## Reference template (中文, 小红书)

The 小红书 layout differs from LinkedIn's: plain `1.` `2.` `3.` numbers
instead of `▸` bullets, no clickable URLs, short single-noun section
labels, restrained academic register. The compressed default has 4
sections (🚨 / 🔍 / 💡 / 📊) plus the link block; the affiliation
paragraph and the 🤝 / hashtag blocks are off by default. Within each
block, no blank lines; between blocks, exactly one blank line.

```
🚨 <一句话钩子>。或！或？

🔍 背景
<2–4 句的背景：旧方法是什么，在原 setting 下表现如何，更换 modality / loss / 范式后为什么失效。最后一句简短指出旧方法是"错误的归纳偏置"。>

💡 主要贡献：
1. <场景> 的 <现象>（<axis 标签>）
<贡献 1 详细说明，1–3 句>
2. <场景> 的 <现象>（<axis 标签>）
<贡献 2 详细说明，1–3 句>
3. 提出 <方法名>：<一句话定位>
<方法详细说明，1–3 句，必须包含"零成本 / 同等开销 / 原地替换"之类的实用属性>
4. <变体名>：<一句话功能>
<详细说明，2–3 句，使一般读者无需读论文就能理解为什么这个变体是必要的、它修复了什么、它和默认模式的区别>

📊 实验结果：
1. <场景 1（包含子场景，如 simulation + real robot）>：<结果 + 关键数字。后续相关结果直接在同一段内继续，不另起空行>
2. <场景 2>：<结果 + 关键数字>

📄 论文 arXiv ID: <bare arxiv id, e.g. 2605.19282>
💻 源码 GitHub 搜: <OWNER/REPO>
📝 项目主页搜: <domain/path without https>
```

Optional add-ons (only when the user explicitly asks):

- An affiliation paragraph immediately under the hook line (no blank line
  between hook and affiliation): `<这项工作由 <机构 1>、<机构 2> 与
  <机构 3> 联合完成，<一句话定位本工作>，并提出 <方法名> 作为 <解决方案>。>`.
- A 🤝 acknowledgements block (mirroring the LinkedIn version, but with
  plain names since 小红书 has no `@` tagging workflow).
- A `#hashtag` line. If included, place it as the LAST line of the post.

The LinkedIn version always keeps the 🤝 block.

If the user wants a 微信公众号 / 知乎 version instead of 小红书, fall back
to the LinkedIn structure (with `▸` bullets and full URLs) but in Chinese.
微信 / 知乎 are more text-friendly than 小红书 and tolerate links.

## Platform-specific notes

LinkedIn:

- The post above is the canonical version. Use it as-is.
- Tell the user that after pasting, they must re-type each `@Name` inside
  LinkedIn's composer to trigger the autocomplete and turn it into a tag.
- LinkedIn auto-shortens links to `lnkd.in/...` on render — that's expected,
  do not pre-shorten them in the source.

X / Twitter:

- If the user wants a thread, split into 6–7 tweets following the same
  section order. Each tweet starts with `n/` and may use 1–2 emojis.
- For a single tweet, compress to: opener + paper link + Figure 1 image
  cue. Tell the user to attach Figure 1 (Muon NS vs new method comparison
  for optimizer-style papers, or the analogous summary figure).
- Use `@username` X handles in acknowledgements; if unknown, leave the
  plain name and tell the user to fill in the handle.

小红书:

- Use the 小红书-specific Chinese template: short single-noun section
  labels (`🔍 背景`, `💡 主要贡献：`, `📊 实验结果：`), plain `1.` `2.`
  `3.` `4.` numbers (NOT 1️⃣ keycap, NOT `▸`), no clickable URLs, no
  body-emphasis emojis.
- Cover image is critical. Recommend the paper's headline figure (e.g. the
  Muon NS vs Pion high-pass NS scalar map for Pion-style papers).
- First line is the only one visible in the feed before "展开"; it must
  be a single hook + a one-line answer. Either rhetorical-question style
  or declarative-with-surprise style is OK; the hook line may end in
  `？` `?` `！` or `。`, all other sentences must end in `。`.
- Affiliation paragraph is OPTIONAL. The compressed default omits it.
  When included, it goes directly under the hook (no blank line, no
  leading emoji), giving collaborating institutions and one-sentence
  positioning.
- Do NOT include raw external URLs. Use bare IDs / paths
  (`arXiv ID: 2605.xxxxx`, `GitHub 搜: OWNER/REPO`,
  `项目主页搜: domain/path`).
- Do not include the 🤝 acknowledgements block or the hashtag line by
  default. Add them only when the user explicitly requests.

## Workflow

When the user asks for a paper announcement post:

1. Confirm you have all 7 inputs above. Ask for any missing ones.
2. Draft the English LinkedIn version following the EN template.
3. Draft the 中文 version following the ZH template, mirroring the structure
   one-to-one (same number of bullets per section).
4. If the user asks for X, also produce a 6–7 tweet thread plus a single-tweet
   compressed variant.
5. Show all versions in fenced code blocks (` ``` `) so the user can copy
   raw text without losing the `▸` and emoji characters.
6. After the post, list any platform-specific actions the user must take
   manually (e.g. "type `@Sijia` inside LinkedIn's editor to convert the
   tag", "attach Figure 1 as the cover image on 小红书").

## Iteration shortcuts

When the user asks for refinements, support these common requests:

- "压缩一点 / shorter": tighten each bullet to one sentence; drop
  parentheticals; combine adjacent bullets if they share a subject.
- "更正式 / less colloquial": remove emojis from bullet bodies (they should
  not be there anyway), replace casual verbs ("crashes", "blows up") with
  neutral ones ("collapses", "diverges").
- "换 bullet 符号": for LinkedIn / X, offer the prioritized list
  ▸ (default), ▪, →, 🔹. For 小红书, the default is plain `1.` `2.` `3.`
  for the numbered contributions section and plain paragraphs for the
  results section — do NOT switch to ▸ or 1️⃣ on 小红书. Do NOT offer `-`
  or `*` on any platform.
- "加致谢 / 加 acknowledgements": always close the post with the 🤝 section,
  one paragraph, no bullets unless multiple distinct groups are thanked.
- "去掉 LinkedIn 短链 / restore original links": the user means swap any
  `lnkd.in/...` URLs back to the original arxiv / github / personal-blog
  URLs. Do this without re-asking.

## Anti-patterns observed in past iterations

These are mistakes the user has explicitly corrected before; do not repeat:

- Emitting `:rotating_light:` / `:warning:` shortcodes in the final post.
- Using `-` or `*` as bullets in any final post.
- Adding emojis inside bullet bodies of the LinkedIn / X version (only
  headers should have emojis there). 小红书 is the exception — body emojis
  are allowed but used sparingly.
- Inventing `lnkd.in/...` shortened URLs.
- Adding extra "this is huge / amazing / mind-blowing" filler.
- Paraphrasing the paper title.

Specific to 小红书:

- Using `▸` on 小红书 (it does not match the platform's visual idiom).
- Using 1️⃣ 2️⃣ 3️⃣ keycap emojis on 小红书 — the user finds them too
  informal. Use plain `1. ` `2. ` `3. ` instead, for both `💡` and `📊`
  sections.
- Using the older long section labels `💡 论文核心贡献：` /
  `📊 主要实验结果：` in compressed posts. Prefer the short
  `💡 主要贡献：` / `📊 实验结果：` labels.
- Phrasing the `🔍` header as a question (`🔍 核心问题：<...>？`) by
  default. The compressed default uses the single-noun label `🔍 背景`;
  the question form is acceptable only when the question framing
  genuinely helps.
- Prefixing every numbered contribution with `定位 / 揭示 / 分析` when
  the noun phrase already conveys the meaning more compactly. Use
  `<场景> 的 <现象>（<axis>）` for problem items by default.
- Including the affiliation paragraph by default in compressed posts.
  It is OPTIONAL and is the FIRST compression knob to drop when the
  post exceeds the ~1000-character budget.
- Inserting blank lines inside a single block (e.g. between a section
  header and its body, or between a numbered item title and its body).
  Within a block, the body line must immediately follow the title line.
- Splitting closely related results (e.g. simulation + real-robot) into
  separate paragraphs or separate numbers. Keep them in one numbered
  block under one headline.
- Pasting full `https://...` URLs (the platform filters / down-ranks them).
- Using the LinkedIn-style "very high-quality work" opener — open with a
  rhetorical-question or declarative-with-surprise hook instead.
- Adding loud marketing words like `翻车`, `全军覆没`, `硬核亮点`,
  `瞬时崩溃`, `行业级`. The user wants the post to read like a research
  abstract, not a 自媒体 announcement. At most one light phrase per post,
  in the hook line only.
- Decorating the body with extra emojis (✨ 🔹 🤔 🤝). The 小红书 version
  uses emojis ONLY in the 4 section headers (🚨 🔍 💡 📊) and the 3 link
  prefixes (📄 💻 📝).
- Adding a 🤝 acknowledgements section or a `#hashtag` line by default —
  these are explicitly OFF by default for 小红书; only include if asked.
- Translating the paper title to Chinese — if the title is mentioned, keep
  it in the original English inside 《 》 brackets. (If the title itself
  is Chinese, no brackets needed.)

## Reference example

For a complete, hand-tuned reference post that meets the bar described in
this skill, see [examples.md](examples.md). It contains the EN LinkedIn,
ZH 小红书, X single-tweet, and X thread variants for the Pion paper, along
with notes on what makes that example "good". Read it before drafting if
the user has not given you all 7 inputs yet — the structure and pacing
generalize to most ML / optimization / robotics papers.
