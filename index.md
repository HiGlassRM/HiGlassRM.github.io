---
layout: project_page
permalink: /

title: "HiGlassRM: Learning to Remove High-prescription Glasses via Synthetic Dataset Generation"
authors:
    Sebin Lee, Heewon Kim
affiliations:
    Soongsil University
paper: https://higlassrm.github.io/
# video: https://www.youtube.com/results?search_query=turing+machine
code: https://higlassrm.github.io/
data: https://higlassrm.github.io/
---

<div class="centered-image-container">
    <img src="/static/image/compare_real_FFHQ.jpg" alt="FFHQ Dataset Examples" class="adjustable-image">
    <p class="figure-caption">Figure1: Qualitative comparison on FFHQ [15]. Our HiGlassRM explicitly compensates this geometric distortion, preserving identity-consistent facial geometry and background alignment.overview</p>
</div>
<!-- Using HTML to center the abstract -->
<div class="columns is-centered has-text-centered">
    <div class="column is-four-fifths">
        <h2>Abstract</h2>
        <div class="content has-text-justified">
Existing eyeglass removal methods can handle frames and shadows but fail to correct lens-induced geometric distortions, as public datasets lack the necessary supervision. To address this, we introduce the <span style="color: teal;font-weight: bold; font-size: 1.1em;">HiGlass Dataset</span>, the first large-scale synthetic dataset providing explicit flow-based supervision for refractive warping. We also propose <span style="color: teal;font-weight: bold; font-size: 1.1em;">HiGlassRM</span>, a novel pipeline whose core is a network that explicitly estimates a displacement flowmap to de-warp distorted facial geometry. Experiments on both synthetic and real images show that this flowmap-centric approach, trained on our data, significantly improves identity preservation and perceptual quality over existing methods. Our work demonstrates that explicitly modeling and correcting geometric distortion via flowmap estimation, enabled by targeted supervision, is key to faithful eyeglass removal.
        </div>
    </div>
</div>

---


## HiGlass Dataset Overview

![Dataset Generation Overview](/static/image/dataset_tutorial.png)
*Figure3: HiGlass Dataset synthesis overview*

From the binary eyeglass-frame mask $M$, the outermost contour $C$ is detected and drawn to create the filled silhouette mask $C_m$. A bitwise XOR with $M$ yields the lens mask $S$. The complement $\overline{C}_m$ masks the face image $O$ to produce the background-preserved image $O_m$. A flow map $F$ is computed and applied inside $S$ to obtain the lens-distorted content $L_d$. Finally, the colored frame $M_C$ is added and the result is composited with $O_m$ to form the final image $I$.


![Dataset Examples](/static/image/HiGlass_dataset_sample_v2.png)
*Figure6: Examples from HiGlass Dataset*

The HiGlass Dataset provides rich visual samples showcasing diversity in frame shapes and lens powers. Each paired sample contains five core components: $(I, M, F, D, O)$. The HiGlass image $I$ is the final composite with all synthesized optical effects. The binary eyeglass-frame mask $M$ localizes only the frames. The displacement flowmap $F$ encodes the geometric warp caused by the lens (e.g., minification or magnification). The shadow-free image $D$ is a rendered variant that retains these optical effects but removes cast shadows. Finally, the face image $O$ is the eyeglass-free supervision target.


## HiGlassRM Overview

![Method Overview](/static/image/framework.jpg)
*Figure4: Overview of the proposed HiGlassRM.*

The framework begins by transforming the real image $R$ and synthetic image $I$ into unified feature maps $R_{fm}$ and $I_{fm}$ through the Domain Adaptation (DA) Network. The Glass Mask Network processes $I_{fm}$ to generate an eyeglass mask $\widehat{M}$, identifying the presence of eyeglasses. The De-Shadow Network takes $I$ and $\widehat{M}$ as input to produce a shadow-free image $\widehat{D}$. The Flowmap Network, using $I_{fm}$ and $\widehat{M}$, generates a flow map $\widehat{F}$ to correct distortion. This flow map is applied to $\widehat{D}$ through Grid Sampling, producing $D_f$. Next, element-wise multiplication with the inverted mask $\overline{M}$ yields the masked de-distorted image $D_m$. The De-Glass Network then processes $D_m$ and $\widehat{M}$ to generate the eyeglass-free image $\widehat{O}$.


## Experimental Results on Real Data
![Dataset Examples](/static/image/meglass.png)

<!-- ## Key Ideas
1. Turing first presented the concept of a "computable number," which refers to a number that can be computed by an algorithm or a definite step-by-step process.
2. He introduced the notion of a Turing machine, an abstract computational device consisting of an infinite tape divided into cells and a read-write head. The machine can read and write symbols on the tape, move the head left or right, and transition between states based on a set of rules.
3. Turing demonstrated that the set of computable numbers is enumerable, meaning it can be listed in a systematic way, even though it is not necessarily countable.
4. He proved the existence of non-computable numbers, which cannot be computed by any Turing machine.
5. Turing showed that the Entscheidungsproblem is undecidable, meaning there is no algorithm that can determine, for any given mathematical statement, whether it is provable or not.



## Table: Comparison of Computable and Non-Computable Numbers

| Computable Numbers | Non-Computable Numbers |
|-------------------|-----------------------|
| Rational numbers, e.g., 1/2, 3/4 | Transcendental numbers, e.g., π, e |
| Algebraic numbers, e.g., √2, ∛3 | Non-algebraic numbers, e.g., √2 + √3 |
| Numbers with finite decimal representations | Numbers with infinite, non-repeating decimal representations |

He used the concept of a universal Turing machine to prove that the set of computable functions is recursively enumerable, meaning it can be listed by an algorithm.

## Significance
Turing's paper laid the foundation for the theory of computation and had a profound impact on the development of computer science. The Turing machine became a fundamental concept in theoretical computer science, serving as a theoretical model for studying the limits and capabilities of computation. Turing's work also influenced the development of programming languages, algorithms, and the design of modern computers.

## Citation
```
@article{turing1936computable,
  title={On computable numbers, with an application to the Entscheidungsproblem},
  author={Turing, Alan Mathison},
  journal={Journal of Mathematics},
  volume={58},
  number={345-363},
  pages={5},
  year={1936}
}
``` -->
