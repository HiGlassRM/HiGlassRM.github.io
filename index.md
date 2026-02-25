---
layout: project_page
permalink: /

title: "HiGlassRM: Learning to Remove <br> High-prescription Glasses <br>via Synthetic Dataset Generation"
authors:
    <a href="https://sebinyday.github.io/">Sebin Lee</a> and <a href="https://scholar.google.com/citations?user=B1Yuz3gAAAAJ&hl=ko&oi=ao">Heewon Kim</a>
affiliations:
    Soongsil University
conference: WACV 2026
paper: https://higlassrm.github.io/
video: https://www.youtube.com/watch?v=y73z4J86Dhw
# code: https://higlassrm.github.io/
data: https://higlassrm.github.io/
---

<div class="centered-image-container">
    <img src="/static/image/compare_real_FFHQ.jpg" alt="FFHQ Dataset Examples" class="adjustable-image">
    Our HiGlassRM explicitly compensates this geometric distortion, <br> preserving identity-consistent facial geometry and background alignment.
</div>
<br>
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


## Synthetic Dataset Generation

![Synthetic Dataset Generation](/static/image/dataset_tutorial.png)
*Figure3: Overview of the HiGlass Dataset synthesis.*

<!-- From the binary eyeglass-frame mask $M$, the outermost contour $C$ is detected and drawn to create the filled silhouette mask $C_m$. A bitwise XOR with $M$ yields the lens mask $S$. The complement $\overline{C}_m$ masks the face image $O$ to produce the background-preserved image $O_m$. A flow map $F$ is computed and applied inside $S$ to obtain the lens-distorted content $L_d$. Finally, the colored frame $M_C$ is added and the result is composited with $O_m$ to form the final image $I$. -->


<ul style="list-style: none; padding-left: 0;">
<li>1️⃣Segment lens area from the binary eyeglass-frame mask ($M$)</li>
<li>2️⃣Depth-& center-aware flowmap synthesis</li>
<li>$$F(\mathbf{p}) = (\mathbf{p} - \mathbf{p}_c) \odot R_{\text{depth}}(\mathbf{p}) \odot S(\mathbf{p})$$</li>
<li>3️⃣Selective warping ($𝐿_𝑑$)+ background-preserving composition</li>
</ul>
<!-- <li style="list-style: none;">$$R_{\text{depth}}(x,y)=
\begin{cases}
R_{\text{background}}, & T_{\text{depth}}(x,y)\ge\delta\\
\operatorname{Smoothing}\!\big(r;S,x,y\big), & T_{\text{depth}}(x,y)<\delta
\end{cases}$$ </li> -->

<br>
![Synthetic Dataset Generation Results](/static/image/HiGlass_dataset_sample_v2.png)
*Figure6:  Visual examples from HiGlass Dataset.*

<ul style="list-style: none; padding-left: 0;">
<li>The HiGlass Dataset contains 29,071 paired samples, each consisting of five components ($I$, $M$, $F$, $D$, $O$). </li>
</ul>
<!-- The HiGlass Dataset provides rich visual samples showcasing diversity in frame shapes and lens powers. Each paired sample contains five core components: $(I, M, F, D, O)$. The HiGlass image $I$ is the final composite with all synthesized optical effects. The binary eyeglass-frame mask $M$ localizes only the frames. The displacement flowmap $F$ encodes the geometric warp caused by the lens (e.g., minification or magnification). The shadow-free image $D$ is a rendered variant that retains these optical effects but removes cast shadows. Finally, the face image $O$ is the eyeglass-free supervision target. -->

---

## HiGlassRM Framework

![Method Overview](/static/image/framework.jpg)
*Figure4: Overview of the proposed HiGlassRM.*

<!-- The framework begins by transforming the real image $R$ and synthetic image $I$ into unified feature maps $R_{fm}$ and $I_{fm}$ through the Domain Adaptation (DA) Network. The Glass Mask Network processes $I_{fm}$ to generate an eyeglass mask $\widehat{M}$, identifying the presence of eyeglasses. The De-Shadow Network takes $I$ and $\widehat{M}$ as input to produce a shadow-free image $\widehat{D}$. The Flowmap Network, using $I_{fm}$ and $\widehat{M}$, generates a flow map $\widehat{F}$ to correct distortion. This flow map is applied to $\widehat{D}$ through Grid Sampling, producing $D_f$. Next, element-wise multiplication with the inverted mask $\overline{M}$ yields the masked de-distorted image $D_m$. The De-Glass Network then processes $D_m$ and $\widehat{M}$ to generate the eyeglass-free image $\widehat{O}$. -->
<ul >
<li >HiGlassRM decouples geometric correction from appearance restoration.</li>
<li>The core component, the Flowmap Network, predicts a two-channel displacement field $\widehat{F}$ to explicitly model lens-induced distortion.</li>
<li>The predicted flow is applied via grid sampling to restore the warped facial geometry.</li>
<li>Once geometry is restored, the De-Glass Network synthesizes the final eyeglass-free output.</li>
</ul>
---

## Experimental Results on Real Data
![Dataset Examples](/static/image/meglass.png)
*Figure 8. Qualitative comparison on the MeGlass dataset*

## Experimental Results on HiGlass Dataset
![Dataset Examples](/static/image/experiment.jpg)
*Figure 7. Qualitative comparison of our method (HiGlassRM) against five baseline methods and the Ground Truth on the HiGlass Dataset.*
![Dataset Examples](/static/image/Table1_Quantitative.png)
*Table 1. Quantitative comparison on the test split of the HiGlass dataset.*

## Citation
```
TBD (To be published at WACV 2026)
```

## Acknowledgements
```
TBD (To be published at WACV 2026)
```