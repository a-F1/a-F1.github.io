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

## Output structure (apply to BOTH EN and ZH)

Use these section emoji headers in this exact order. Each header is a single
line with one emoji + a short title. Body of each section uses `▸` bullets.

```
🚨 [opener] paper title + 3 links (Paper / Blog / Code)

⚠️ The Question / 研究问题
▸ background sentence
▸ the research question

🔍 Key Findings / 核心发现
[one short framing sentence]
▸ failure mode 1 (with axis labels in parentheses)
▸ failure mode 2 (with axis labels in parentheses)

💡 Method / 方法
[one-line method intro]
▸ key mechanism
▸ design variants / modes

✅ Results / 实验结果
▸ headline result 1 (with concrete numbers)
▸ headline result 2

📌 Takeaway / 一句话总结
▸ why old approach fails
▸ what new approach achieves

🤝 Acknowledgements / 致谢
[advisor + collaborators in one line]
```

Add hashtags ONLY at the very end of the 中文 / 小红书 version. Do not add
hashtags to the English LinkedIn version unless the user asks.

## Hard formatting rules

These are non-negotiable because LinkedIn / X / 小红书 do not render markdown:

- Use the literal Unicode character `▸` as the bullet, NOT `-`, `*`, `•`,
  or `🔹`. (`-` and `*` look invisible after rendering; `▸` is the agreed
  default.)
- Use Unicode emoji characters directly (🚨 ⚠️ 🔍 💡 ✅ 📌 🤝). NEVER use
  GitHub / Slack shortcodes like `:rotating_light:` or `:warning:` — they do
  not render on LinkedIn / X / 小红书.
- Put exactly one blank line between sections. To get visibly larger spacing
  on LinkedIn, use two blank lines (LinkedIn collapses to one but renders the
  paragraph break).
- Keep the bullet symbol `▸` followed by a single space, then the body text.
- One bullet = one paragraph. Each bullet starts on a new line.
- Use original URLs (`https://arxiv.org/...`, `https://github.com/...`),
  NOT LinkedIn-shortened `https://lnkd.in/...` URLs. LinkedIn rewrites them
  itself after publishing.
- Emojis appear ONLY in section headers, not inside bullet bodies.
- For acknowledgements on LinkedIn, prefix each name with `@` so the user can
  manually convert them to LinkedIn tags in the editor. Tell the user they
  must re-type `@Name` inside LinkedIn's composer to trigger the tag picker
  (plain `@` text does not auto-link).

## Voice and tone

- First person, present tense. Opening verb: `Excited to share` /
  `很高兴分享`.
- Professional, not colloquial. Avoid filler like "super excited", "blown
  away", "this is huge".
- Acknowledge that LLM pretraining / SFT / RL terminology is fine; the
  audience is ML researchers and practitioners.
- Keep Chinese version slightly more conversational than the English one,
  but still no slang. Mixed Chinese-English is OK for technical terms
  (e.g. `loss`, `modality`, `pretraining`, `attention head`).
- Compress aggressively. Each bullet should be one or two sentences. The
  whole post should fit in roughly 250–350 words (EN).

## Do / Don't

Do:

- Mirror the EN and ZH versions one-to-one, section by section.
- Include at least one concrete benchmark number in `✅ Results` (e.g.
  `100% on LIBERO Object in 1,500 steps vs. 32.2% for AdamW`).
- End the LinkedIn version with the acknowledgements; end the 小红书 version
  with hashtags after the acknowledgements.
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

```
🚨 很高兴分享我们的最新论文：

《<Paper Title>》

▸ 论文：<arxiv url>

▸ 博客：<blog url>

▸ 代码：<github url>



⚠️ 研究问题

▸ <方法 / setting 背景>

▸ <我们要回答的问题>



🔍 核心发现

<一句话框架>

▸ <场景 1（modality / loss / paradigm 哪一项变了）>：<观察 + 为什么旧方法失败>

▸ <场景 2（同上）>：<观察 + 为什么旧方法失败>



💡 方法概述

<一句话介绍方法名 + 它替换了什么>

▸ <核心机制>

▸ <模式 / 变体>



✅ 实验结果

▸ <场景 1 的数字结论>

▸ <场景 2 的数字结论>



📌 一句话总结

▸ <旧方法为什么是错的归纳偏置>

▸ <新方法给出了什么，附带 cost 声明>



🤝 致谢

衷心感谢导师 <Advisor>，以及合作者 <Collab1> (<Aff>)、<Collab2> 和 <Collab3> (<Aff>) 的宝贵讨论与支持。


#<topic1> #<topic2> #<topic3> #论文分享 #AI研究
```

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

- Cover image is critical. Recommend the paper's headline figure (e.g. the
  Muon NS vs Pion high-pass NS scalar map for Pion-style papers).
- First two lines are the only ones visible before "展开"; put the hook
  ("🚨 很高兴分享...") and one-line takeaway in those two lines.
- Hashtags go at the very bottom. 8–12 tags is typical. Always include
  `#论文分享` and `#AI研究` plus 4–8 topic-specific tags.

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
- "换 bullet 符号": offer the prioritized list ▸ (default), ▪, →, 🔹.
  Do NOT offer `-` or `*`.
- "加致谢 / 加 acknowledgements": always close the post with the 🤝 section,
  one paragraph, no bullets unless multiple distinct groups are thanked.
- "去掉 LinkedIn 短链 / restore original links": the user means swap any
  `lnkd.in/...` URLs back to the original arxiv / github / personal-blog
  URLs. Do this without re-asking.

## Anti-patterns observed in past iterations

These are mistakes the user has explicitly corrected before; do not repeat:

- Emitting `:rotating_light:` / `:warning:` shortcodes in the final post.
- Using `-` or `*` as bullets in the final post.
- Forgetting to mirror EN and ZH bullet counts per section.
- Adding emojis inside bullet bodies (only headers should have emojis).
- Inventing `lnkd.in/...` shortened URLs.
- Adding extra "this is huge / amazing / mind-blowing" filler.
- Paraphrasing the paper title.

## Reference example

For a complete, hand-tuned reference post that meets the bar described in
this skill, see [examples.md](examples.md). It contains the EN LinkedIn,
ZH 小红书, X single-tweet, and X thread variants for the Pion paper, along
with notes on what makes that example "good". Read it before drafting if
the user has not given you all 7 inputs yet — the structure and pacing
generalize to most ML / optimization / robotics papers.
