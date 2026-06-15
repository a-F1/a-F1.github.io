# Examples: Paper Announcement Posts

Reference posts that match the conventions in `SKILL.md`. Use these as the
quality bar when drafting new posts. They are NOT to be paraphrased into new
posts; they only show what "good" looks like.

## Example 1: Pion (NeurIPS-style preprint, optimizer paper)

Inputs that produced these posts:

- Title: `Rethinking Muon Beyond Pretraining: Spectral Failures and High-Pass Remedies for VLA and RLVR`
- arXiv: `https://arxiv.org/abs/2605.19282`
- Blog: `https://chongyu-fan.netlify.app/posts/pion/`
- Code: `https://github.com/OPTML-Group/Pion`
- Method name: `Pion (sPectral hIgh-pass Optimization on momeNtum)`
- Advisor: `Sijia Liu`
- Collaborators: `Mingyi Hong (UMN), Gaowen Liu and Ramana Rao Kompella (Cisco)`

### English (LinkedIn)

```
🚨 Excited to share our latest paper: "Rethinking Muon Beyond Pretraining: Spectral Failures and High-Pass Remedies for VLA and RLVR"

▸ Paper: https://arxiv.org/abs/2605.19282

▸ Blog: https://chongyu-fan.netlify.app/posts/pion/

▸ Code: https://github.com/OPTML-Group/Pion



⚠️ The Question

▸ Muon is a matrix-aware optimizer widely used in LLM pretraining. Through Newton–Schulz (NS) iterations, it pushes singular values of the momentum matrix toward 1, a form of uniform spectral whitening.

▸ LLM pretraining is only one specific setting (text modality, classification loss, supervised learning). We ask whether uniform whitening remains appropriate when we change the modality, the loss, or the learning paradigm.



🔍 Key Findings

Muon exhibits spectral failure in two representative regimes.

▸ VLA training (modality extended to language / vision / action, loss changed from classification to regression): the rank of the action gradient is much lower than those of vision and language. Uniform whitening amplifies noise-dominated tail directions to the scale of informative ones, contaminating the update.

▸ RLVR (learning paradigm changed from supervised learning to reinforcement learning): RL gradients have substantially lower SNR than SFT gradients. Uniform whitening amplifies noise and can cause the policy to collapse within very few training steps.



💡 Method

We propose Pion (sPectral hIgh-pass Optimization on momeNtum), which changes only the polynomial coefficients inside Muon's NS to obtain a high-pass NS iteration.

▸ Pion realizes a spectral high-pass effect: anchoring informative leading singular values near 1, and suppressing noise-dominated tail singular values toward 0.

▸ Pion provides two modes for applying the high-pass NS iteration: a default mode applied to the whole layer matrix, and a per-head mode applied independently to each attention head. The per-head mode is critical for RLVR, as it preserves the per-head heterogeneity inherited from pretrained or SFT models.



✅ Results

▸ In simulated VLA, Pion outperforms AdamW and Muon on both ℓ1-regression and flow-matching architectures, e.g., reaching 100% on LIBERO Object in 1,500 steps vs. 32.2% for AdamW. On a real Franka Research 3, Pion also leads on grasp-and-place tasks.

▸ In RLVR (Qwen3-1.7B / 4B × GRPO / GMPO × MATH / GSM8K), Muon collapses across all settings, while Pion trains stably and outperforms AdamW.



📌 Takeaway

▸ Muon's uniform spectral whitening is highly effective for LLM pretraining, but it can be the wrong inductive bias in low-rank or low-SNR regimes.

▸ Pion replaces uniform whitening with controllable spectral high-pass optimization, making Muon-style optimizers better suited to VLA training and RLVR at zero additional per-step cost.



🤝 Acknowledgements

I am sincerely grateful to my advisor @Sijia Liu, and to our collaborators @Mingyi Hong (UMN), @Gaowen Liu and @Ramana Rao Kompella (Cisco) for their valuable discussions and support.
```

### 中文 (小红书)

Style notes (different from LinkedIn): hook line uses declarative-with-
surprise style (ending in `！`); rhetorical-question style ending in `？`
is also acceptable. The affiliation paragraph is OPTIONAL and is omitted
in this compressed (~1000-character) version. Section headers are short
labels (`🔍 背景`, `💡 主要贡献：`, `📊 实验结果：`); the 🔍 header is a
single noun, NOT a question. Numbered items under `💡` use plain
`1. ` `2. ` `3. ` `4. ` (NOT 1️⃣ keycap emojis; NOT `▸`). Item titles
follow `<主题>（<axis>）` for problem-identification items (1, 2) and
`<verb> <name>：<one-line>` / `<变体名>：<功能>` for proposal / variant
items (3, 4). Each numbered item's title and body sit on consecutive lines
with no blank line between them. The 📊 results section also uses plain
`1.` `2.` numbering; closely related results (simulation + real-robot)
live in ONE numbered block under a single headline. Concrete benchmark
numbers (e.g. the LIBERO Object 100% / 97.0% / 32.2% breakdown) MAY be
dropped when compressing aggressively, as long as the qualitative claim
(`优于 AdamW 和 Muon`, `全部崩溃`) is preserved. No raw URLs (use arXiv
ID + GitHub path + 项目主页 path). No body-emphasis emojis. No 🤝
acknowledgements block. No hashtags. At most one mild emphasis word
(`竟然` / `失效` / `零成本` / `不再适用`) in the hook only.

```
🚨 Muon 在 LLM 预训练之外竟然会失效！我们提出"谱高通"优化器 Pion 作为解决方案。

🔍 背景
Muon 通过 Newton–Schulz (NS) 迭代把动量矩阵奇异值推向 1（即"均匀谱白化"），在 LLM 预训练击败 AdamW。但预训练只是一个特殊 setting（文本 + 分类 + 监督），更换模态、损失或范式时，均匀白化便成为错误的归纳偏置。

💡 主要贡献：
1. VLA 的失效根因（modality + loss）
VLA 中 action 梯度天然低秩（7 自由度 + regression loss），均匀白化把噪声尾部抬到与主方向同尺度，污染更新。
2. RLVR 的崩溃现象（learning paradigm）
GRPO 等 RL 的策略梯度信噪比远低于 SFT，均匀白化放大噪声方向，使策略数步内便崩溃。
3. 提出 Pion：零成本谱高通替换
Pion 仅修改 NS 多项式系数即实现谱高通：归一化后较大的奇异值锚定到 1，较小的压向 0。每步开销与 Muon 一致。
4. Per-head 模式：保留 head 间异质性
RLVR 起点是已预训练 / SFT 的 LLM，attention 各 head 权重范数本就有差异，应接收不同尺度更新；但 Muon 以整层做 NS 会拉平各 head 的更新。Pion 因此提供 per-head 模式：沿 head 维度独立做高通 NS 保留异质性，在 RLVR 上恢复稳定训练。

📊 实验结果：
1. VLA：Pion 在 ℓ1-regression 与 flow-matching 上优于 AdamW 和 Muon。真机实验 Franka 3 抓取放置任务同样领先。
2. RLVR：Qwen3-1.7B / 4B × GRPO / GMPO × MATH / GSM8K 共 8 个 setting，Muon 全部崩溃，Pion 全部优于 AdamW。

📄 论文 arXiv ID: 2605.19282
💻 源码 GitHub 搜: OPTML-Group/Pion
📝 项目主页搜: chongyu-fan.netlify.app/posts/pion
```

This compressed version is ~990 Chinese characters total. The two largest
compression moves vs. the longer reference draft were: (1) dropping the
affiliation paragraph entirely, and (2) dropping the LIBERO Object detail
numbers (`100% / 97.0% / 32.2%`) and renaming `Franka Research 3` →
`Franka 3`.

### X / Twitter — single tweet (compressed)

Attach Figure 1 (Muon NS vs Pion high-pass NS scalar map) as the cover image.

```
🚨 New preprint: "Rethinking Muon Beyond Pretraining: Spectral Failures and High-Pass Remedies for VLA and RLVR"

Muon's uniform spectral whitening is the wrong inductive bias for VLA (low-rank action gradients) and RLVR (low-SNR policy gradients). Pion fixes it with a high-pass NS at zero extra cost.

📄 https://arxiv.org/abs/2605.19282
```

### X / Twitter — thread (7 tweets)

```
1/ 🚨 New preprint: "Rethinking Muon Beyond Pretraining: Spectral Failures and High-Pass Remedies for VLA and RLVR"

📄 https://arxiv.org/abs/2605.19282
📝 https://chongyu-fan.netlify.app/posts/pion/
💻 https://github.com/OPTML-Group/Pion

🧵👇
```

```
2/ ⚠️ Muon is the de facto matrix-aware optimizer for LLM pretraining. Its NS iteration drives singular values of the momentum toward 1 — uniform spectral whitening.

But pretraining is only one setting (text + classification + supervised). What about other modalities, losses, paradigms?
```

```
3/ 🔍 We identify two failure modes:

▸ VLA: action-module gradients are inherently low-rank → uniform whitening amplifies noisy tail directions

▸ RLVR: policy gradients have low SNR → uniform whitening blows up noise, policy collapses in a few steps
```

```
4/ 💡 Pion (sPectral hIgh-pass Optimization on momeNtum) — a drop-in replacement for Muon's NS.

Same control flow, same per-step cost; only the polynomial coefficients change.

Two stages: Promotion anchors leading σ near 1, Suppression pushes tail σ to 0.
```

```
5/ For RLVR we also offer a per-head mode that applies the high-pass NS independently across attention heads. This preserves the per-head heterogeneity inherited from pretrained / SFT checkpoints — default mode flattens it.
```

```
6/ ✅ Results

VLA (LIBERO + LIBERO-Plus, VLA-Adapter & VLANeXt): Pion > Muon > AdamW. 100% on LIBERO Object in 1,500 steps. Real Franka R3 + π_0.5 + DROID also leads.

RLVR (Qwen3 1.7B/4B × GRPO/GMPO × MATH/GSM8K): Muon collapses on all 8 settings; Pion > AdamW.
```

```
7/ 🤝 Joint work with my advisor @Sijia_Liu_, Mingyi Hong (UMN), Gaowen Liu and Ramana Rao Kompella (Cisco).

Code: https://github.com/OPTML-Group/Pion
```

## Notes on this example

What makes this example match the bar:

- Concrete benchmark numbers in the results section (`100% ... vs. 32.2%`,
  `8 settings`, `Muon collapses`).
- Acknowledgements is one paragraph, not a bulleted list.
- LinkedIn version uses `@Name` placeholders for the user to convert into
  LinkedIn tags inside the composer; 小红书 version uses plain names
  because 小红书 does not have a tag-by-typing-`@` workflow.

LinkedIn / X conventions captured here:

- Section headers use the canonical fixed emoji set (🚨 ⚠️ 🔍 💡 ✅ 📌 🤝).
- All bullets use `▸`; bullet count per section matches between EN and the
  X thread.
- Paper title is verbatim, in English, in quotes — never translated, never
  paraphrased.
- The single-tweet X version compresses the entire post to one hook
  sentence + one URL; the thread version expands each section into one tweet.

小红书 conventions captured here (different from LinkedIn / X):

- Hook line uses declarative-with-surprise style with `竟然` + `失效`,
  ending in `！`. The rhetorical-question style ending in `？` is equally
  acceptable; pick whichever matches the paper's framing. All other
  sentences in the post end in `。`, never `！`.
- The affiliation paragraph is OPTIONAL. This compressed version omits
  it entirely. When included, it attaches directly to the hook line with
  no blank line in between (they form one block).
- Section headers are short labels: `🔍 背景` (single noun, NOT a
  question), `💡 主要贡献：`, `📊 实验结果：`. The earlier
  `🔍 核心问题：<question>？` form is also acceptable when the question
  framing genuinely helps; otherwise prefer the short label.
- Numbered contributions use plain `1. ` `2. ` `3. ` `4. ` — NOT 1️⃣
  keycap emojis, NOT `▸`. Each item's title and body sit on consecutive
  lines (no blank line within the item); blank lines only appear between
  items.
- Item title forms: problem-identification items (1, 2) use noun-led
  `<主题>（<axis>）` (e.g. `VLA 的失效根因（modality + loss）`); the
  proposal item (3) uses verb-led `提出 <方法>：<one-line>`; variant
  items (4) use noun-led `<变体名>：<功能>`. Avoid prefixing every item
  with `定位 / 揭示 / ...` if the noun phrase already conveys the same
  meaning more compactly.
- Each numbered item is 1–3 sentences in compressed form. Method-variant
  items (here, per-head) may be slightly longer (2–3 sentences) so a
  reader who has not seen the paper can understand what the variant
  fixes, why the default fails, and how it differs.
- The 📊 results section also uses plain `1.` `2.` numbering. Closely
  related results (VLA simulation + real-robot) are merged into ONE
  numbered block under a single headline rather than split. Concrete
  benchmark numbers (e.g. LIBERO Object 100% / 97.0% / 32.2%) may be
  dropped in compressed posts as long as a qualitative claim is kept.
- No raw `https://...` URLs anywhere in the post. Links appear as
  `arXiv ID: 2605.19282`, `GitHub 搜: OPTML-Group/Pion`,
  `项目主页搜: chongyu-fan.netlify.app/posts/pion`.
- The only mild emphasis words used here are `竟然` and `失效` (in the
  hook). The rest of the post reads like a research abstract. Avoid
  louder marketing terms like `翻车`, `全军覆没`, `硬核亮点`, `瞬时崩溃`,
  `行业级`, `大火的`.
- Body emojis appear ONLY in the 4 section headers (🚨 🔍 💡 📊) and the
  3 link prefixes (📄 💻 📝). No ✨, 🔹, 🤔, or 🤝.
- Paper title is NOT explicitly quoted in the body; readers identify the
  paper through the arXiv ID. (If the user wants the title visible,
  English title in 《 》 brackets, never translated.)
- No 🤝 acknowledgements block. No `#hashtag` line. Both are off by
  default for 小红书; the LinkedIn version is where acknowledgements live.
- Total length target: ~1000 Chinese characters or less. The compression
  knobs (in priority order) are: drop affiliation paragraph, drop benchmark
  detail numbers, shorten item titles, then trim item bodies.
