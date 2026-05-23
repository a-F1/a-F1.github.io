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

### 中文 (小红书 / 微信 / 知乎)

```
🚨 很高兴分享我们的最新论文：

《Rethinking Muon Beyond Pretraining: Spectral Failures and High-Pass Remedies for VLA and RLVR》

▸ 论文：https://arxiv.org/abs/2605.19282

▸ 博客：https://chongyu-fan.netlify.app/posts/pion/

▸ 代码：https://github.com/OPTML-Group/Pion



⚠️ 研究问题

▸ Muon 是近年来在 LLM 预训练中受到广泛关注的矩阵感知优化器。它通过 Newton–Schulz (NS) 迭代将动量矩阵的奇异值统一推向 1，即一种均匀谱白化。

▸ 但 LLM 预训练只是一个特定 setting（文本模态、分类损失、监督学习）。当我们更换模态、损失甚至学习范式时，均匀谱白化是否仍然适用？



🔍 核心发现

Muon 在两个代表性场景下都会出现谱失效。

▸ VLA 训练（modality 扩展到 language / vision / action，loss 由 classification 变为 regression）：action 梯度的秩明显低于 vision 与 language，均匀白化会把噪声尾部抬到与有效主方向相同的尺度，污染更新方向。

▸ RLVR（learning paradigm 由 supervised learning 变为 reinforcement learning）：RL 梯度的信噪比明显低于 SFT，均匀白化会放大噪声，导致策略在很短训练步内崩溃。



💡 方法概述

我们提出 Pion（sPectral hIgh-pass Optimization on momeNtum），仅修改 Muon 中 NS 的多项式系数，得到 high-pass NS。

▸ Pion 实现一种谱高通效果：将信息量较高的头部奇异值锚定在 1 附近，将噪声主导的尾部奇异值压向 0。

▸ Pion 提供两种执行 high-pass NS 的模式：default 模式以整个 layer 为单位执行，per-head 模式按 attention head 分别执行。per-head 模式对 RLVR 至关重要，能保留预训练或 SFT 模型中已有的 head 间异质性。



✅ 实验结果

▸ 在 VLA 仿真中，Pion 在 ℓ1-regression 与 flow-matching 两类结构下均优于 AdamW 和 Muon。LIBERO Object 上 Pion 训练 1,500 步即达 100% 成功率，AdamW 仅为 32.2%。在真实 Franka Research 3 抓取放置任务中 Pion 同样领先。

▸ 在 RLVR（Qwen3-1.7B / 4B × GRPO / GMPO × MATH / GSM8K）中，Muon 在全部设置下均崩溃到接近零准确率，Pion 能恢复稳定训练并优于 AdamW。



📌 一句话总结

▸ Muon 的均匀谱白化在 LLM 预训练中非常有效，但在低秩或低信噪比场景中可能成为错误的归纳偏置。

▸ Pion 将均匀白化替换为可控的谱高通优化，在零额外每步开销下使 Muon 类优化器更适用于 VLA 训练和 RLVR 后训练。



🤝 致谢

衷心感谢导师 Sijia Liu，以及合作者 Mingyi Hong (UMN)、Gaowen Liu 和 Ramana Rao Kompella (Cisco) 的宝贵讨论与支持。


#Muon #Pion #优化器 #大模型 #VLA #RLVR #强化学习 #具身智能 #机器人学习 #论文分享 #AI研究
```

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

- Section count is identical between EN and ZH versions (8 sections each).
- Bullet count per section matches between EN and ZH.
- All bullets use `▸`; section headers are the canonical 7 emojis in fixed order.
- Concrete benchmark numbers in `✅ Results` (`100% ... vs. 32.2%`).
- Acknowledgements is one paragraph, not a bulleted list.
- Paper title is verbatim, in English, in quotes — never translated, never
  paraphrased.
- LinkedIn version uses `@Name` placeholders; ZH version uses plain names
  because 小红书 / 微信 do not have a tag-by-typing-`@` workflow.
- The single-tweet X version compresses the entire post to one hook
  sentence + one URL; the thread version expands each section into one tweet.
