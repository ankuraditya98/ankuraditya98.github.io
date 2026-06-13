---
layout: page
title: DeformRF - Data-driven Beamforming and Direction Finding with Deformable Antenna Arrays
description: ACM MobiSys'26
img: assets/img/d2rf_thumb.jpeg
importance: 3
category: work
related_publications: true
---

<style>
p { text-align: justify; }
</style>

### Abstract

Low-frequency antenna arrays enable obstacle-penetrating communications, but their large physical footprint—a 4×4 array at 150 MHz requires 16 m²—prevents practical deployment. **DeformRF** enables portable, deformable arrays that compress to backpack-size yet maintain beamforming performance when deployed on flexible substrates. Our key insight is that data-driven methods can predict complex electromagnetic behavior under deformation without real-time simulations. DeformRF combines: (1) a 260,000-sample synthetic dataset mapping deformations to EM characteristics, (2) physics-inspired ML models achieving >94% prediction accuracy on average, and (3) smartphone-based 3D reconstruction requiring no infrastructure. In real-world experiments, DeformRF maintains beamforming gains within 1 dB of optimal despite severe deformation, while baselines degrade by 4–10 dB. For emergency response scenarios, our 4×4 flexible array achieves ±5° direction-finding accuracy when tracking signals through buildings.

<div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/system_design_dataset_and_train.png" title="Deform2EM dataset and model training" class="img-fluid rounded z-depth-1" %}
        <div class="caption">Deform2EM dataset and model training</div>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-sm-10 mt-2 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/system_design_deployment.png" title="DeformRF Deployment" class="img-fluid rounded z-depth-1" %}
        <div class="caption">DeformRF Deployment</div>
    </div>
</div>
<div class="caption text-center">
    <strong>DeformRF system overview</strong>
</div>

---

### The Challenge

Conventional antenna array design assumes rigid substrates with precisely controlled geometry. Many real-world deployments, however, involve surfaces whose shapes are dictated by external constraints—building facades, vehicle bodies, and curved architectural surfaces make mounting rigid, high-gain antennas physically impractical. This problem is especially severe at lower frequencies (≤ 2.4 GHz), where obstacle-penetrating communications demand large apertures.

A fabric-based 4×4 array can be rolled into a 30 cm cylinder to fit a backpack, then rapidly unfolded and draped over any available surface. Yet this flexibility introduces a fundamental challenge: deformations alter electromagnetic (EM) behavior in complex, non-linear ways—simultaneously changing radiation patterns, impedance, and phase.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/casefor_0829.png" title="DeformRF array prototype" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A flexible antenna array can be compactly stored
and expanded during operation for beamforming and direction estimation. However, its EM behavior varies due to
changing surface curvatures.
</div>

---

### Key Challenges

Enabling effective wireless operation with deformable antenna arrays presents two significant challenges:

1. **Infinite deformation states** — deformable substrates can assume virtually infinite configurations, making it impractical to pre-compute EM behaviors for all possibilities.
2. **Dynamic changes** — deformations occur continuously during normal use as the substrate bends, folds, and twists, rendering one-time calibration methods ineffective.

Conventional EM simulations (e.g., Method of Moments) are computationally prohibitive—simulating a single deformed antenna can take several hours on a standard PC.

---

### The DeformRF Approach

DeformRF is a lightweight end-to-end system with three core components:

#### 1. Deform2EM Dataset

A comprehensive synthetic dataset of ~260,000 unique deformed antenna configurations and their EM characteristics, generated using Blender physics simulations and full-wave EM solvers. The dataset spans frequencies from 150 MHz to 850 MHz and covers both simple dipoles and complex Yagi–Uda arrays.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        <!-- Figure 5: Deform2EM library pipeline (CG animation → Point clouds → Meshed Yagi → EM sim) -->
        {% include figure.liquid path="assets/img/deformrf/dataset_generation_detail1203.png" title="Deform2EM dataset pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <strong>Deform2EM</strong> library curves are extracted from computer-graphic animations of DeformRF in Blender, complemented by control equations, and synthesized across 8 frequencies. Each curve (dipole and Yagi) is stored as a point cloud with four simulated EM parameters. The dataset contains more than 260,000 curves.
</div>

#### 2. Physics-Inspired ML Model

A hybrid CNN–Vision Transformer architecture that predicts the complete EM behavior of a deformed antenna directly from its 3D geometry. Motivated by the computational structure of Method of Moments solvers:

- **CNN branch** (ResNet-Lite, 6 residual blocks) captures local geometric features.
- **ViT branch** (depth 8, 6 attention heads) captures global electromagnetic dependencies.
- **Four parallel MLP heads** predict radiation pattern, phase, polarization, and reflection coefficient (Γ).

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        <!-- Figure 7: CNN+Transformer hybrid model architecture diagram -->
        {% include figure.liquid path="assets/img/deformrf/pin_ml_model.png" title="CNN+Transformer hybrid model" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The CNN+Transformer hybrid model for DeformRF, predicting four EM parameters from pixelized antenna geometry.
</div>

#### 3. Smartphone-Based 3D Reconstruction

DeformRF uses commodity smartphone cameras for one-time 3D capture of the antenna array, replacing expensive optical infrastructure. The pipeline:

1. **Spatial segmentation** — isolates the antenna array from the background using tri-section clustering.
2. **RF element identification** — locates antenna elements via RGB color anchoring.
3. **Curvature extraction** — applies k-means clustering, hierarchical merging, and spline interpolation to recover each element's precise 3D shape.

<!-- This achieves sub-wavelength (λ/8) accuracy without any specialized hardware. -->

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/smartphone_based_3d_construction.png" title="Shape estimation accuracy" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    DeformRF curve reconstruction: We use a cell-phone to obtain the raw 3D image and perform background
and floor removal, followed by RGB color anchoring and clustering, and finally centroid extraction and curve smoothening.
</div>

---

### Key Evaluation Results

<br>

#### 1. Beamforming Performance

<div style="padding-left: 1.5rem;">
<p><strong>Implemented 2×2 array:</strong> At low fold all methods exceed 12 dB, as the array is nearly flat. As deformation increases, DeformRF maintains consistent performance — outperforming DeformRF-PCA, Geometry, and Assume-Flat by >3.1 dB, 3 dB, and 4 dB respectively (a 3 dB gain effectively doubles array output). Prediction error stays below 1 dB across all fold states, while baselines reach 3–6 dB error under mid/high fold.</p>

<p><strong>Simulated 5×6 array:</strong> DeformRF sustains ~30 dB gain across all 810 deformation configurations. DeformRF-PCA and Geometry drop to ~25 dB at mid/high fold; Assume-Flat falls below 20 dB. Prediction error holds at ~2 dB, while baselines exceed 5 dB as deformation increases.</p>
</div>

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/phase_shift_imp_eval_usecase_0423.png" title="Impl: Measured gain" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(a) Impl: Measured gain</div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/phase_shift_imp_eval_usecase_error_0423.png" title="Impl: Prediction error" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(b) Impl: Prediction error</div>
    </div>
</div>
<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/phase_shift_sim_eval_usecase_0423.png" title="Sim: Measured gain" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(c) Sim: Measured gain</div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/phase_shift_sim_eval_usecase_error_0423.png" title="Sim: Prediction error" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(d) Sim: Prediction error</div>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/BF_sim_windrose0417.png" title="Sim: Beam-pattern" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(e) Sim: Beam-pattern</div>
    </div>
</div>
<div class="caption">
    Performance of Beamforming: (a) and (b) show the mean gain and mean error for three folds for the implemented 2×2 array, while (c) and (d) show the mean gain and mean error for the simulated 5×6 array. (e) Beampattern of an instance of the 5×6 antenna array compared with the PCA baseline.
</div>

<br>

#### 2. Direction-of-Arrival Estimation

<div style="padding-left: 1.5rem;">
<p><strong>Indoor 4×4 array (850 MHz):</strong> DeformRF achieves <strong>±5° DOA accuracy</strong>, outperforming baselines by more than 3×.</p>

<p><strong>Outdoor-to-indoor emergency scenario:</strong> 4° DOA error — matching commercial rigid arrays despite deformation, and nearly halving the best baseline.</p>
</div>

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/DOA_room_coverage_0829.png" title="Deform2EM DOA result and estimated room coverage" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(a) <strong>Deform2EM</strong> DOA result and estimated room coverage.</div>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/deformrf/DOA_indoor0419.png" title="DOA estimation error across five tested angles" class="img-fluid rounded z-depth-1" %}
        <div class="caption">(b) DOA est. error across five tested angles (x-axis).</div>
    </div>
</div>
<div class="caption">
    (a): <strong>Left</strong> — Average DOA results of DeformRF compared to the flat assumption. <strong>Right</strong> — Estimated room coverage for DeformRF and the assume flat. (b): DOA accuracy at each test angle.
</div>

---

### Key Contributions

- **Deform2EM Dataset** — First large-scale synthetic dataset (260k samples) mapping physical antenna deformations to full EM characteristics across 150–850 MHz.
- **Physics-Inspired ML** — Hybrid CNN–ViT model achieving >94% EM prediction accuracy, running in real-time on commodity hardware.
- **Infrastructure-Free Sensing** — Smartphone-only 3D reconstruction pipeline achieving sub-wavelength deformation accuracy without LiDAR or optical base stations.
- **End-to-End System** — Integrated beamforming and DOA estimation that maintains performance within 1 dB of optimal under severe deformation.

<!-- ---

This work was published at {% cite deformrf %}. -->
