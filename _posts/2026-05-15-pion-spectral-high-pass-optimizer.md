---
title: 'Pion: Rethinking Muon Beyond Pretraining via a Spectral High Pass Optimizer'
date: 2026-05-15
permalink: /posts/2026/05/pion-spectral-high-pass-optimizer/
tags:
  - optimizer
  - muon
  - vla
  - rlvr
  - spectral
excerpt: "Muon orthogonalizes the momentum matrix and drives every singular value to one. This works well for LLM pretraining, but it breaks down in two regimes that matter beyond pretraining: vision language action (VLA) training and reinforcement learning with verifiable rewards (RLVR). Pion fixes this by replacing the uniform whitening of Muon with a spectral high pass that keeps the informative head of the spectrum and suppresses the noisy tail, at the same per step cost as Muon."
---

<style>
.pion-post img {
    max-width: 100%;
    height: auto;
    display: block;
    margin: 12px auto;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.08);
    background: #fff;
}
.pion-post .fig-caption {
    text-align: center;
    color: #555;
    font-size: 0.9rem;
    font-style: italic;
    margin-top: 4px;
    margin-bottom: 18px;
}
.pion-post .fig-row {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
    align-items: flex-end;
    margin: 12px 0;
}
.pion-post .fig-row figure {
    flex: 1 1 220px;
    max-width: 320px;
    margin: 0;
    text-align: center;
}
.pion-post .fig-row figure img {
    margin: 0 auto;
}
.pion-post .fig-row figcaption {
    font-size: 0.85rem;
    color: #555;
    font-style: italic;
    margin-top: 6px;
}
.pion-post blockquote.takeaway {
    background: #f0f6ff;
    border-left: 5px solid #1f4ca7;
    padding: 12px 18px;
    margin: 16px 0;
    border-radius: 0 8px 8px 0;
    color: #1f3a68;
    font-weight: 500;
}
.pion-post table {
    width: 100%;
    border-collapse: collapse;
    margin: 16px 0;
    font-size: 0.95rem;
}
.pion-post table th, .pion-post table td {
    border: 1px solid #e2e8f0;
    padding: 8px 12px;
    text-align: center;
}
.pion-post table th {
    background: #f5f9ff;
    color: #1f3a68;
}
</style>

<div class="pion-post" markdown="1">

This post walks through the motivation and design of **Pion** (s**P**ectral h**I**gh pass **O**ptimization on mome**N**tum), a drop in replacement for Muon's Newton Schulz iteration that targets two underexplored training regimes where Muon falls short: vision language action (VLA) training and reinforcement learning with verifiable rewards (RLVR). The work is joint with Gaowen Liu, Mingyi Hong, Ramana Rao Kompella, and Sijia Liu.

## 1. Background: Muon as a Matrix Aware Optimizer

For a weight matrix $\boldsymbol{\Theta} \in \mathbb{R}^{m \times n}$, Muon performs the steepest descent step under the spectral norm. Given a stochastic gradient $\mathbf{G}_t$ and momentum buffer $\mathbf{M}_t = \mu \mathbf{M}_{t-1} + \mathbf{G}_t$, the update is

$$
\boldsymbol{\Theta}_t = \boldsymbol{\Theta}_{t-1} - \eta \, \mathrm{msign}(\mathbf{M}_t),
$$

where $\mathrm{msign}(\cdot)$ is the matrix sign operator. If $\mathbf{M} = \mathbf{U} \boldsymbol{\Sigma} \mathbf{V}^\top$ is the compact SVD, then

$$
\mathrm{msign}(\mathbf{M}) = \mathbf{U}\, \mathrm{sign}(\boldsymbol{\Sigma})\, \mathbf{V}^\top = \mathbf{U} \mathbf{V}^\top.
$$

Every nonzero singular value is mapped to one. This is the **uniform spectral whitening** that Muon performs.

Computing the SVD at every step is expensive, so Muon approximates $\mathrm{msign}(\mathbf{M})$ with a small number of Newton Schulz (NS) iterations. After normalizing the input as $\mathbf{X} \leftarrow \mathbf{X} / (\\|\mathbf{X}\\|_F + \epsilon)$ so all singular values lie in $[0, 1]$, each NS step applies an odd quintic matrix polynomial:

$$
\mathbf{X} \leftarrow a\, \mathbf{X} + b\, \mathbf{X}\mathbf{X}^\top \mathbf{X} + c\, \mathbf{X}(\mathbf{X}^\top \mathbf{X})^2,
$$

with the canonical Muon coefficients $(a, b, c) = (3.4445, -4.7750, 2.0315)$. By the identity $\mathbf{X}(\mathbf{X}^\top \mathbf{X})^j = \mathbf{U}\, \boldsymbol{\Sigma}^{2j+1}\, \mathbf{V}^\top$, the NS step preserves the singular vectors and reshapes each singular value through the **scalar polynomial**

$$
f(\sigma;\, a, b, c) \,\triangleq\, a\sigma + b\sigma^3 + c\sigma^5, \qquad \sigma \in [0, 1].
$$

So designing an NS iteration reduces to designing $f$ on $[0, 1]$. Muon's NS iteration (panel a below) is constructed so that repeated application drives every $\sigma$ in the open interval $(0, 1]$ toward one.

<div class="fig-row">
<figure><img src="/images/blog/pion/iter_NS.png" alt="Muon NS"><figcaption>(a) Muon NS</figcaption></figure>
<figure><img src="/images/blog/pion/iter_P.png" alt="Promotion"><figcaption>(b) Promotion polynomial $f_{\mathrm{p}}$</figcaption></figure>
<figure><img src="/images/blog/pion/iter_S.png" alt="Suppression"><figcaption>(c) Suppression polynomial $f_{\mathrm{s}}$</figcaption></figure>
<figure><img src="/images/blog/pion/iter_pion_mix.png" alt="High pass NS"><figcaption>(d) Pion high pass NS</figcaption></figure>
</div>
<p class="fig-caption">Figure 1. Visualization of $f(\sigma)$ on $\sigma \in [0, 1]$. Muon (a) drives every singular value toward one. Pion combines a Promotion stage (b) with a Suppression stage (c) to obtain a high pass profile (d).</p>

> **Why this works for pretraining.** In LLM pretraining the gradient is approximately full rank with reasonable signal to noise ratio, so spreading optimization signal evenly across all spectral directions promotes exploration and improves sample efficiency over AdamW.

## 2. Where Muon Breaks Down Beyond Pretraining

We study two regimes that have become increasingly central but remain underexplored for matrix aware optimization.

### 2.1 VLA Training Has Strong Rank Heterogeneity

A vision language action policy is factorized into a vision encoder, a language backbone, and an action head. The three modules have very different intrinsic dimensionality. We measure this through the **effective rank** (erank) of the gradient matrix $\mathbf{G} \in \mathbb{R}^{m \times n}$:

$$
\mathrm{erank}(\mathbf{G}) \,\triangleq\, \exp\!\Big( H(\mathbf{p}) \Big), \qquad H(\mathbf{p}) = -\sum_{i=1}^n p_i \log p_i, \qquad p_i = \frac{\sigma_i(\mathbf{G})}{\sum_j \sigma_j(\mathbf{G})}.
$$

A higher erank indicates that gradient energy is spread across many directions, while a lower erank indicates concentration in a few dominant components.

<img src="/images/blog/pion/erank_heatmap_sampled.png" alt="Per module gradient erank along training">
<p class="fig-caption">Figure 2. Per module gradient erank along the trajectory of training VLA Adapter on LIBERO Object. Vision is highest, language is intermediate, and action is consistently the lowest.</p>

The ordering is stable across training: vision keeps the highest erank, language is intermediate, and the **action gradient is consistently the lowest**. This is intuitive: vision inputs encode rich pixel statistics, language tokens use high dimensional embeddings, while a robot action is a seven dimensional vector encoding incremental end effector translation, rotation, and a binary gripper command.

When Muon is applied uniformly to a low erank action gradient, it lifts the noisy tail directions to the same magnitude as the few informative leading directions. The resulting update is dominated by spectral floor noise.

<img src="/images/blog/pion/success_rate.png" alt="VLA optimizer comparison">
<p class="fig-caption">Figure 3. Test success rate on LIBERO Object at 4.5k training steps. The vision and language modules use AdamW. The action module uses AdamW, Muon, or LRMuon. Muon underperforms AdamW on the action head, while LRMuon helps but pays roughly 15x training cost.</p>

A natural fix is **Low Rank Muon (LRMuon)**, which projects the momentum onto a low rank subspace via SVD or Gaussian sketching before applying NS. LRMuon does close the gap on accuracy, but the SVD or sketch step inflates total wall clock by about an order of magnitude. It also commits to a fixed rank $k$ that cannot adapt across layers and steps.

> **Limitation 1 (lack of rank adaptiveness).** Conventional Muon is not adaptive to rank heterogeneity across modules, and explicit low rank projection introduces significant computational overhead, limiting scalability.

### 2.2 RLVR Has Low Signal to Noise Ratio Gradients

For a stochastic gradient $\mathbf{G}$ on a layer's weight matrix, define the per step signal to noise ratio as

$$
\mathrm{SNR}(\mathbf{G}) \,\triangleq\, \frac{\\|\mathbb{E}[\mathbf{G}]\\|_F^2}{\mathbb{E}\big[\,\\|\mathbf{G} - \mathbb{E}[\mathbf{G}]\\|_F^2\,\big]}.
$$

A higher SNR indicates a cleaner gradient signal. We compare SFT and GRPO on Qwen3 1.7B trained on MATH levels 3 to 5.

<img src="/images/blog/pion/grad_snr_sft_vs_rl_math3-5_step80.png" alt="SFT vs GRPO SNR">
<p class="fig-caption">Figure 4. Gradient SNR of SFT and GRPO under AdamW on Qwen3 1.7B. GRPO consistently has substantially lower SNR than SFT.</p>

GRPO has much lower SNR than SFT throughout training. Two factors drive the gap:

1. **Coarser supervision granularity.** SFT receives token level teacher signals, while GRPO uses trajectory level rewards, so each token gets a much sparser learning signal.
2. **Stabilization mechanisms.** Importance sampling, clipping, and group relative normalization in GRPO reweight or zero out parts of the per token gradients, which inflates variance.

When Muon is run on top of these low SNR gradients, the uniform whitening lifts the variance of the noisy directions to the same magnitude as the informative ones. The policy collapses within a few steps.

<img src="/images/blog/pion/acc_adamw_muon.png" alt="MATH500 accuracy AdamW vs Muon">
<p class="fig-caption">Figure 5. MATH500 accuracy of GRPO on Qwen3 1.7B with AdamW versus Muon. AdamW improves steadily; Muon collapses to near zero accuracy within a few steps.</p>

A second issue specific to RLVR is that Muon's NS treats each weight matrix as a single block. Attention projections inherit per head specialization from pretraining (different heads have different Frobenius norms and attend to different patterns). Muon ignores this structure and applies one orthogonalization to the whole projection.

> **Limitation 2 (lack of noise adaptiveness).** Muon's uniform spectral whitening amplifies noisy directions in low SNR gradients, making it ill suited for noise sensitive post training regimes.

### 2.3 Unifying Spectral View

Both limitations share a single spectral signature. In the SVD of $\mathbf{M}_t$, the few **leading** singular values carry the informative descent direction, while the long **tail** of small singular values is dominated by noise (a spectral floor when erank is low, stochastic estimation noise when SNR is low). Muon's $\mathrm{msign}$ lifts the tail to the magnitude of the head and corrupts the update in both regimes.

A natural remedy is a **spectral high pass**: anchor large singular values near one (preserve the informative head) and contract small singular values toward zero (suppress the noisy tail). This is exactly what Pion realizes.

## 3. The Pion Optimizer

Since each NS step reshapes $\sigma \in [0, 1]$ through the scalar polynomial $f(\sigma; a, b, c) = a\sigma + b\sigma^3 + c\sigma^5$, designing an NS iteration reduces to designing $f$. A single such polynomial cannot realize a sharp high pass on the unit interval, so Pion splits the five default NS steps into two stages:

* a **Promotion** polynomial $f_{\mathrm{p}}$ applied for $k_{\mathrm{p}}$ steps to first lift dominant singular values toward one while preserving their ordering;
* a **Suppression** polynomial $f_{\mathrm{s}}$ applied for $k_{\mathrm{s}} = k - k_{\mathrm{p}}$ steps to then attenuate the remaining smaller components.

Each polynomial gets its own coefficients $(a, b, c)$ and the cutoff is controlled by a single hyperparameter $k_{\mathrm{p}} \in \\{0, 1, \ldots, 5\\}$.

### 3.1 The Promotion Stage

We want $f_{\mathrm{p}}$ to monotonically amplify singular values so that as many of them as possible cross the suppression threshold while keeping their relative order. Three constraints fix the coefficients up to a slope:

1. **(P1) Fixed point.** $f_{\mathrm{p}}(1) = 1$ anchors any direction already at one.
2. **(P2) First order stationarity.** $f_{\mathrm{p}}'(1) = 0$ keeps the anchor flat.
3. **(P3) Boundary concavity.** $f_{\mathrm{p}}''(1) \leq 0$ together with (P2) ensures $\sigma = 1$ is a maximum so that the iteration does not curve upward past one near the boundary.

Solving (P1) and (P2) leaves a one parameter family. Imposing (P3) along with monotonicity on $[0, 1]$ carves out the feasible interval $a_{\mathrm{p}} \in [0, 1.875]$. Since $f_{\mathrm{p}}'(0) = a_{\mathrm{p}}$ controls how strongly each step lifts small singular values, we pick the largest feasible slope:

$$
f_{\mathrm{p}}(\sigma) = 1.875\, \sigma \,-\, 1.25\, \sigma^3 \,+\, 0.375\, \sigma^5.
$$

A pleasant byproduct is that the derivative becomes a perfect square,

$$
f_{\mathrm{p}}'(\sigma) = 1.875\, (1 - \sigma^2)^2 \,\geq\, 0,
$$

so monotonicity on $[0, 1]$ holds automatically.

### 3.2 The Suppression Stage

We want $f_{\mathrm{s}}$ to **pin** large singular values near one and **contract** smaller ones toward zero. The suppression polynomial inherits $f_{\mathrm{s}}(1) = 1$ and $f_{\mathrm{s}}'(1) = 0$ from Promotion, and adds a **spectral filtering** condition $f_{\mathrm{s}}'(0) = 0$ that strips the linear term near the origin so small singular values are driven to zero by the higher order terms. The unique solution is

$$
f_{\mathrm{s}}(\sigma) = 2.5\, \sigma^3 \,-\, 1.5\, \sigma^5.
$$

### 3.3 Putting It Together: High Pass NS

Chaining $k_{\mathrm{p}}$ Promotion steps with $k_{\mathrm{s}}$ Suppression steps gives the **high pass NS** iteration of Pion. Fixing $k = 5$ preserves Muon's per step cost. Panel (d) of Figure 1 shows the resulting profile: a sharp transition between a pinned region near one and a filtered region near zero, with $k_{\mathrm{p}}$ controlling the cutoff. Empirically, **suppression dominant** allocations with $k_{\mathrm{s}} \geq 3$ work best for both VLA and RLVR.

> **Takeaway.** Pion is a drop in replacement for Muon's NS iteration. It uses the same control flow, the same per step cost, and changes only the polynomial coefficients used inside the iteration.

### 3.4 The Per Head Mode

Pion has two application modes:

* **Default mode**: apply the iteration to each weight matrix as a single block (mirrors Muon).
* **Per head mode**: first reshape each attention projection along its head dimension into multiple per head sub matrices, then run the iteration independently on each.

Because the inner products $\mathbf{X}^\top \mathbf{X}$ are already batched along the head dimension after the reshape, the per head mode incurs no extra cost over the default mode.

We use the default mode for VLA training and the per head mode for RLVR. The reason is that RLVR starts from a pretrained LLM whose attention layers have heterogeneous per head Frobenius norms (Figure 6 top), and these per head norms govern attention sharpness and gradient magnitudes. Default mode Pion gives an essentially flat update variance across heads (Figure 6 bottom left), while the per head mode preserves the heterogeneous, layer dependent updates required to keep that pretrained structure (Figure 6 bottom right).

<img src="/images/blog/pion/acc_mh.png" alt="Per head ablation MATH500">
<p class="fig-caption">Figure 6. MATH500 accuracy of AdamW, Muon (default vs per head), and Pion (default vs per head) on Qwen3 1.7B with GRPO on MATH levels 3 to 5. Per head Pion is the only configuration that beats AdamW. Per head mode does not save Muon, since the lack of noise adaptiveness remains the primary issue.</p>

This isolates two complementary findings:

1. The spectral high pass is the **primary driver** of RLVR stability. Per head Muon (same reshape on top of Muon's NS) still collapses.
2. The per head reshape acts as an **auxiliary mechanism** that preserves head heterogeneity inherited from pretraining, and it pushes per head Pion past AdamW.

## 4. Experimental Highlights

Pion is evaluated on:

* **VLA training** with two architectures: $\ell_1$ regression based VLA Adapter and flow matching based VLANeXt, on LIBERO and LIBERO Plus.
* **RLVR post training** with GRPO and GMPO on Qwen3 1.7B and Qwen3 4B over MATH and GSM8K.

Three optimizer configurations are compared in each setting: AdamW, Muon, and Pion.

### 4.1 VLA: Pion Outperforms Muon on Both Heads

<div class="fig-row">
<figure><img src="/images/blog/pion/vlaadapter_libero.png" alt="LIBERO four tasks"><figcaption>(a) Final success rate on LIBERO</figcaption></figure>
<figure><img src="/images/blog/pion/vlaadapter_training.png" alt="Object training"><figcaption>(b) Success rate vs training steps on Object</figcaption></figure>
</div>
<p class="fig-caption">Figure 7. AdamW, Muon, and Pion for VLA Adapter on LIBERO.</p>

On VLA Adapter / LIBERO Object, Pion reaches 95.4% success at 500 steps and saturates at 100% by 1500 steps. AdamW needs many more steps and Muon stays consistently behind. Pion outperforms Muon on every one of the four LIBERO suites.

For VLANeXt (flow matching), Pion not only wins on the clean LIBERO benchmark but also amplifies its advantage on the more challenging LIBERO Plus split, especially under language ($+9$ points), noise ($+6$ points), and robot ($+6$ points) perturbations. This suggests that the high pass mechanism produces more robust policies under distribution shift, which is consistent with the picture that uniform whitening overamplifies non generalizable noise directions.

| Optimizer | LIBERO | LIBERO Plus | Background | Camera | Language | Layout | Light | Noise | Robot |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| AdamW | 79.45 | 64.57 | 68.97 | 70.38 | 54.50 | 61.80 | 76.35 | 66.37 | 47.04 |
| Muon | 93.65 | 72.34 | 82.72 | 68.00 | 77.53 | 76.21 | 86.17 | 69.98 | 57.36 |
| **Pion (Ours)** | **96.35** | **75.93** | **84.53** | **70.88** | **86.93** | **76.71** | **90.67** | **76.09** | **63.18** |

<p class="fig-caption">Table 1. AdamW, Muon, and Pion for VLANeXt on LIBERO and LIBERO Plus. The first two columns are average success rate. The remaining columns break LIBERO Plus down by perturbation.</p>

### 4.2 RLVR: Pion Succeeds While Muon Collapses

Across eight RLVR settings (GRPO and GMPO times Qwen3 1.7B and Qwen3 4B times MATH and GSM8K), Muon consistently collapses to near zero accuracy. Pion not only recovers a meaningful training signal but also outperforms AdamW with faster convergence.

<div class="fig-row">
<figure><img src="/images/blog/pion/rl_val_core_grpo_math3-5_qwen3_1.7b.png" alt="GRPO MATH 1.7B"><figcaption>(a) GRPO, Qwen3 1.7B, MATH</figcaption></figure>
<figure><img src="/images/blog/pion/rl_val_core_grpo_gsm8k_qwen3_1.7b.png" alt="GRPO GSM8K 1.7B"><figcaption>(b) GRPO, Qwen3 1.7B, GSM8K</figcaption></figure>
</div>
<p class="fig-caption">Figure 8. Validation accuracy versus training step on RLVR. Muon (red) collapses; Pion (blue) outperforms AdamW (green).</p>

### 4.3 Reverse Ablation: Direction of Spectral Shaping Matters

To isolate that the gains come specifically from the high pass direction, we construct **Low pass Muon (LPMuon)**, which mirrors Pion in NS structure and per step cost but flips the polynomial coefficients to induce a low pass mapping (contracts large singular values, amplifies small ones). LPMuon fails to train; its accuracy stays at the initial checkpoint.

<div class="fig-row">
<figure><img src="/images/blog/pion/pion0_muon_lowrankmuon.png" alt="Low pass profile"><figcaption>(a) Low pass profile</figcaption></figure>
<figure><img src="/images/blog/pion/mhlpmuon.png" alt="GSM8K accuracy"><figcaption>(b) GSM8K accuracy</figcaption></figure>
</div>
<p class="fig-caption">Figure 9. (a) Scalar map $f(\sigma)$ of LPMuon. (b) GSM8K accuracy of AdamW, Pion, and LPMuon (Qwen3 1.7B, GRPO). Combined with Muon's failure (no filtering) in Figure 8, this isolates the direction of spectral shaping as the key factor.</p>

## 5. Conclusion

Muon's uniform spectral whitening is a wonderful inductive bias for LLM pretraining, but it is mismatched with two regimes that have become increasingly important: cross modality VLA training and RLVR post training. Both regimes share a common spectral signature in the momentum SVD. The informative descent direction lives in the few leading singular values, while the noise lives in the long tail. Muon's $\mathrm{msign}$ lifts the tail and corrupts the update in both cases.

Pion fixes this by replacing the uniform whitening with a **spectral high pass**, realized as a two stage Promotion plus Suppression NS iteration. The cost stays identical to Muon, the only knob is the step allocation between the two stages, and an optional per head reshape preserves the attention head heterogeneity inherited from pretraining.

Across VLA training on LIBERO and LIBERO Plus and RLVR post training on Qwen3 1.7B and Qwen3 4B over MATH and GSM8K, Pion consistently beats AdamW and Muon, including settings where Muon collapses to zero. We see this as evidence that matrix aware optimization beyond LLM pretraining benefits from spectral filtering rather than uniform whitening, and we are excited to extend this picture to LLM pretraining itself, to adaptive cutoff schedules across layers and steps, and to combinations with other Muon variants.

</div>
