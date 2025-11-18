---
layout: page
title: "BSAN: Deep Learning for Mosquito Behavior Modeling"
description: "Developed BSAN, a deep learning model that predicts mosquito flight trajectories by integrating olfactory, thermal, and visual cues. The model reveals interpretable behavioral states to inform vector control strategies."
img: assets/img/mosquito_states_diagram.jpg
importance: 2
category: research
giscus_comments: true
---

## Motivation: Why Mosquito Behavior Matters

Mosquitoes transmit diseases like malaria, dengue, and Zika to over 700 million people annually. Understanding how mosquitoes locate human hosts is critical for predicting disease transmission patterns and designing effective vector control strategies. 

Mosquitoes navigate toward hosts through a sophisticated sequence: detecting CO₂ plumes from tens of meters away, following thermal gradients at close range, and using visual cues throughout. Each stage involves different behavioral states—from wide-ranging exploration to precise tracking—driven by the dynamic integration of multiple sensory modalities.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mosquito_states_diagram.jpg" title="Mosquito behavioral states" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Mosquito host-seeking encompasses four behavioral states: hovering (close-range assessment), exploration (appetitive search), casting (plume reacquisition), and tracking (directed approach). Each state integrates CO₂, thermal, and visual cues differently.
</div>

## The Challenge: Why Traditional Models Fail

Modeling mosquito flight behavior presents fundamental challenges that break traditional approaches:

**1. Inherent Stochasticity**  
Even under identical conditions, individual mosquitoes display substantial variation in flight paths. This isn't just measurement noise—it reflects both external randomness (turbulent airflow, intermittent odor plumes) and internal neural variability in decision-making.

**2. Discontinuous Sensory Experience**  
Turbulent air creates odor plumes that arrive as intermittent packets, not smooth gradients. Thermal boundaries appear as sharp transitions. Visual features emerge suddenly from background clutter. Traditional ODE models assume smooth, continuous dynamics.

**3. Discrete Behavioral States**  
Mosquitoes don't gradually transition between behaviors—they switch abruptly between distinct states (exploration → tracking → casting) based on sensory context. Each state requires specialized sensory-motor transformations that ODEs cannot capture with predetermined equations.

**4. Unknown Governing Equations**  
We lack complete understanding of the neural circuits underlying host-seeking. Traditional physics-based modeling requires hypothesizing exact mathematical forms upfront—a near-impossible task for complex biological behavior.

## Our Solution: Behavioral State Attention Network (BSAN)

We developed BSAN, a deep learning architecture that models mosquito host-seeking as a hierarchical process of state-dependent sensory integration. Instead of assuming mathematical forms, BSAN learns the complex mapping from sensory history to behavioral output directly from data.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bsan_architecture.jpg" title="BSAN architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    BSAN architecture integrating cross-modal attention, LSTM temporal processing, behavioral state prediction, and mixture-of-experts for trajectory generation.
</div>

### Key Technical Innovations

**Cross-Modal Attention for Sensory Integration**  
Mosquitoes integrate CO₂ (effective at 50+ meters), thermal cues (within 1 meter), and visual features (throughout flight) differently depending on context. Our cross-modal attention mechanism learns to dynamically weight these modalities, mimicking biological sensory hierarchy where different cues dominate at different spatial scales.

**Mixture-of-Experts for Behavioral States**  
We explicitly model four distinct behavioral states through specialized expert networks:
- **Hovering**: Close-range assessment when both CO₂ and heat are present
- **Exploration**: Wide-ranging search when stimuli are minimal  
- **Casting**: Systematic crosswind movements for plume reacquisition
- **Tracking**: Direct upwind flight following CO₂ gradients

Each expert learns state-specific sensory-motor transformations grounded in neurobiology.

**Probabilistic Modeling for Stochasticity**  
A variational encoder captures flight path randomness through latent variables, while Mixture Density Networks predict multi-modal velocity distributions. This allows BSAN to generate diverse, realistic trajectories rather than deterministic averages.

**Temporal Processing with LSTM**  
Two-layer LSTM processes temporal sequences, enabling the model to learn behavioral state transitions based on sensory history—capturing phenomena like anticipatory casting when approaching plume edges.

## Results: Realistic Trajectories and Biological Validation

BSAN generates realistic flight trajectories that match observed mosquito behavior, with appropriate behavioral state transitions throughout the host-seeking sequence.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/trajectory_comparison.jpg" title="Trajectory predictions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    BSAN generates realistic 3D trajectories with velocity profiles and state probabilities that match observed mosquito behavior. The model successfully captures transitions between behavioral states (hovering, exploration, casting, tracking) based on sensory context.
</div>

### Biological Validation

A key strength of BSAN is that its learned representations align with established neurobiology. The model discovers sensory-behavior relationships that match decades of experimental findings:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/feature_correlation.jpg" title="Feature correlations" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Correlation matrix showing learned relationships match biological evidence: CO₂ drives searching and tracking behaviors, temperature triggers hovering at close range, and distance gates assessment behavior.
</div>

**Key Biological Validations:**

- **CO₂ → Casting (0.75) & Tracking (0.65)**: Confirms CO₂'s role as primary long-range attractant that initiates oriented search behavior
- **Temperature → Hovering (0.85)**: Matches observed thermal-guided landing behavior, where mosquitoes linger over warm surfaces before landing
- **Distance → Hovering (-0.80)**: Validates that close-range assessment only occurs within 10-20 cm of the host
- **Visual → Casting (0.65)**: Supports the role of visual cues in navigation and flight stabilization during plume search

These emergent correlations—learned purely from trajectory data without explicit supervision—demonstrate that BSAN captures biologically meaningful sensory-motor computations.

## Impact and Applications

BSAN provides a data-driven behavioral framework with practical applications in disease control:

**Vector Control Optimization**  
Test trap designs and spatial configurations virtually before field deployment. Predict how different sensory features (CO₂ emission rates, thermal signatures, visual contrast) influence mosquito approach behavior and capture success.

**Disease Transmission Modeling**  
Incorporate realistic host-seeking kernels into epidemiological models. Improve predictions of outbreak dynamics and intervention effectiveness by accounting for environment-specific mosquito movement patterns.

**Landscape Genomics**  
Generate movement kernels across heterogeneous landscapes to predict mosquito population connectivity. Inform targeted control strategies by identifying natural barriers and corridors for gene flow.

## Publications & Resources

**BSAN: Behavioral State Attention Network for Modeling Mosquito Host-Seeking Behavior**  
Haotian Sun, Jessie Zixin Li, [John M. Marshall](https://publichealth.berkeley.edu/people/john-marshall)  
*Accepted to Artificial Intelligence for Social Impact Track*, 2026

[Paper PDF](link-to-paper) | [Code](link-to-code)

---

*This research was conducted at the Division of Biostatistics, UC Berkeley.*
