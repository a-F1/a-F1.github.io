---
layout: archive
title: ""
permalink: /publications/
author_profile: true
---

<style>
/* ----------  通用排版  ---------- */
.publication {
    font-family: "Microsoft YaHei", sans-serif;
    line-height: 1.55;
    margin-bottom: 20px;
    padding: 14px 18px;
    border-radius: 10px;
    background: #fff;
    box-shadow: 0 1px 2px rgba(0,0,0,0.08);
}
.pub-title {
    font-weight: 700;
    font-size: 16.5px; /* 略微缩小 */
    margin: 0 0 8px;
    color: #000;
}
.pub-authors {
    font-size: 15px;
    margin: 3px 0 6px;
    color: rgba(0,0,0,0.9);
}
.pub-venue-links {
    font-style: italic;
    margin-top: 2px;
}
.pub-venue {
    display: inline-block;
    margin-right: 6px;
    font-weight: bold;
}
.pub-links {
    display: inline-block;
}
.pub-links a {
    color: #0645AD;
    text-decoration: none;
}
.pub-links a + a {
    margin-left: 0;
}

/* ----------  页面级标题  ---------- */
.section-title {
    font-family: "Microsoft YaHei", sans-serif;
    font-weight: 900;
    font-size: 36px;
    color: #374151;
    margin: 10px 0 32px;
    letter-spacing: 0.7px;
    position: relative;
}
.section-title::after {
    content: "";
    display: block;
    width: 72px;
    height: 3px;
    background: #4F7FD9;
    margin-top: 10px;
    border-radius: 3px;
}

/* ----------  分类标题  ---------- */
.category-header {
    font-family: "Microsoft YaHei", sans-serif;
    font-weight: 900;
    font-size: 24px; /* 再稍微缩小一点 */
    margin: 40px 0 22px;
    padding: 6px 14px;
    border-left: 8px solid;
    border-radius: 0 6px 6px 0;
    letter-spacing: 0.5px;
}

/* Efficient Reasoning 主题色 */
.category-header.efficient {
    color: #1f4ca7;
    background: #f0f6ff;
    border-color: #1f4ca7;
}
.papers-section.efficient .publication {
    border-left: 4px solid #1f4ca7;
}

/* Machine Unlearning 主题色 */
.category-header.unlearning {
    color: #a72828;
    background: #fff5f5;
    border-color: #a72828;
}
.papers-section.unlearning .publication {
    border-left: 4px solid #a72828;
}

/* 细节分隔线 */
hr {
    margin: 18px 0;
    height: 1px;
    background-color: #e2e8f0;
    border: none;
}
</style>

<div class="section-title">Conference&nbsp;Papers</div>

<!-- ================ Efficient Reasoning ================ -->
<section class="papers-section efficient">
  <h1 class="category-header efficient">Efficient Reasoning</h1>

  <div class="publication">
    <div class="pub-title">CyclicReflex: Improving Large Reasoning Models via Cyclical Reflection Token Scheduling</div>
    <div class="pub-authors"><span style="font-weight:bold;text-decoration:underline;color:#1f4ca7;">Chongyu Fan</span>, Yihua Zhang, Jinghan Jia, Alfred Hero, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-links">[<a href="https://arxiv.org/abs/2506.11077">Paper</a>] [<a href="https://github.com/OPTML-Group/CyclicReflex">Code</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">EPiC: Towards Lossless Speedup for Reasoning Training through Edge-Preserving CoT Condensation</div>
    <div class="pub-authors">Jinghan Jia, Hadi Reisizadeh, <span style="font-weight:bold;text-decoration:underline;color:#1f4ca7;">Chongyu Fan</span>, Nathalie Baracaldo, Mingyi Hong, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-links">[<a href="https://arxiv.org/abs/2506.04205">Paper</a>] [<a href="https://github.com/OPTML-Group/EPiC">Code</a>]</span>
    </div>
  </div>
</section>

<!-- ================ Machine Unlearning ================ -->
<section class="papers-section unlearning">
  <h1 class="category-header unlearning">Machine Unlearning</h1>

  <div class="publication">
    <div class="pub-title">Towards LLM Unlearning Resilient to Relearning Attacks: A Sharpness-Aware Minimization Perspective and Beyond</div>
    <div class="pub-authors"><span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan*</span>, Jinghan Jia*, Yihua Zhang, Anil Ramakrishna, Mingyi Hong, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-venue">ICML 2025</span>
      <span class="pub-links">[<a href="https://arxiv.org/abs/2502.05374">Paper</a>] [<a href="https://github.com/OPTML-Group/Unlearn-Smooth">Code</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">Simplicity Prevails: Rethinking Negative Preference Optimization for LLM Unlearning</div>
    <div class="pub-authors"><span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan*</span>, Jiancheng Liu*, Licong Lin*, Jinghan Jia, Ruiqi Zhang, Song Mei, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-links">[<a href="https://arxiv.org/pdf/2410.07163">Paper</a>] [<a href="https://github.com/OPTML-Group/Unlearn-Simple">Code</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">UnlearnCanvas: A Stylized Image Dataset to Benchmark Machine Unlearning for Diffusion Models</div>
    <div class="pub-authors">Yihua Zhang, <span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan</span>, Yimeng Zhang, Yuguang Yao, Jinghan Jia, Jiancheng Liu, Gaoyuan Zhang, Gaowen Liu, Ramana Rao Kompella, Xiaoming Liu, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-venue">NeurIPS 2024</span>
      <span class="pub-links">[<a href="https://arxiv.org/abs/2402.11846">Paper</a>] [<a href="https://github.com/OPTML-Group/UnlearnCanvas">Code</a>] [<a href="https://unlearn-canvas.netlify.app/">Website</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">Defensive Unlearning with Adversarial Training for Robust Concept Erasure in Diffusion Models</div>
    <div class="pub-authors">Yimeng Zhang, Xin Chen, Jinghan Jia, Yihua Zhang, <span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan</span>, Jiancheng Liu, Mingyi Hong, Ke Ding, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-venue">NeurIPS 2024</span>
      <span class="pub-links">[<a href="https://arxiv.org/abs/2405.15234">Paper</a>] [<a href="https://github.com/OPTML-Group/AdvUnlearn">Code</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">Challenging Forgets: Unveiling the Worst-case Forget Sets in Machine Unlearning</div>
    <div class="pub-authors"><span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan*</span>, Jiancheng Liu*, Alfred Hero, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-venue">ECCV 2024</span>
      <span class="pub-links">[<a href="https://arxiv.org/abs/2403.07362">Paper</a>] [<a href="https://github.com/OPTML-Group/Unlearn-WorstCase">Code</a>]</span>
    </div>
  </div>

  <div class="publication">
    <div class="pub-title">SalUn: Empowering Machine Unlearning via Gradient-based Weight Saliency in Both Image Classification and Generation</div>
    <div class="pub-authors"><span style="font-weight:bold;text-decoration:underline;color:#a72828;">Chongyu Fan*</span>, Jiancheng Liu*, Yihua Zhang, Dennis Wei, Eric Wong, Sijia Liu</div>
    <div class="pub-venue-links">
      <span class="pub-venue">ICLR 2024 <span style="color:red">Spotlight</span></span>
      <span class="pub-links">[<a href="https://arxiv.org/abs/2310.12508">Paper</a>] [<a href="https://github.com/OPTML-Group/Unlearn-Saliency">Code</a>] [<a href="https://www.optml-group.com/posts/salun_iclr24">Website</a>]</span>
    </div>
  </div>
</section>

<div style="height: 150px;"></div>
