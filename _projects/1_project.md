---
layout: page
title: "Diffusion Models for Privacy-Preserving Mobility Trajectory Generation"
description: "Developing a diffusion-based framework to generate realistic individual mobility trajectories from aggregate population data. Combines conditional generation with aggregation-consistency loss to balance micro-plausibility and macro-fidelity for public health applications."
img: assets/img/mobility_preview.jpg
importance: 1
category: research
---

## Motivation: Behavioral Foundations of Population Health

Human mobility is a fundamental driver of population health outcomes. Movement patterns determine how diseases spread through communities, how people access healthcare services and economic opportunities, and how cities can design equitable infrastructure. Understanding these dynamics is essential for infectious disease modeling, urban planning, transportation policy, and public health intervention design.

However, detailed mobility data face two critical barriers: **availability** and **privacy**. Individual-level trajectory data are often unavailable due to collection costs or are heavily restricted due to privacy concerns, as such data can easily reveal sensitive personal information. This creates a fundamental tension in public health research—we need detailed behavioral data to understand population dynamics, but we cannot ethically collect or share such data at individual resolution.

## The Challenge: Learning Individual Behavior from Aggregate Data

Modeling human mobility at the individual level while respecting privacy and data constraints presents several interconnected challenges:

**1. Structural Complexity of Trajectories**

Mobility trajectories are long, structured behavioral sequences governed by complex dependencies. A person's visit to a grocery store depends on their residential location, work schedule, transportation access, and daily routines. These dependencies operate across multiple timescales—from immediate decisions (next destination) to long-range constraints (total daily distance, return home).

**2. Aggregate-Only Supervision**

In many cases, we observe only aggregate statistics—such as Census Block Group (CBG) to Point of Interest (POI) visit counts—rather than individual trajectories. Models must infer plausible individual-level behaviors while maintaining statistical consistency with these population-level summaries. This is an inherently underdetermined problem: many individual trajectory distributions could produce the same aggregate statistics.

**3. Demographic and Environmental Heterogeneity**

Mobility patterns vary substantially across demographic groups and environmental conditions. Students, working professionals, and retirees exhibit different temporal rhythms. Urban and rural residents face different spatial constraints. Models must capture this heterogeneity through conditional generation that respects demographic features and local contexts.

**4. Privacy Preservation**

Even synthetic trajectories can pose privacy risks if they too closely resemble real individuals. Any generative framework must produce trajectories that are realistic and useful for analysis but non-identifiable, ensuring they cannot be linked back to specific people.

## Our Approach: Diffusion Models with Aggregate Consistency

We are developing a diffusion-based generative framework that addresses these challenges by learning to synthesize individual mobility trajectories under aggregate supervision. Our approach operates in two key stages:

### Architecture Overview

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mobility_preview.jpg" title="Training and generation pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Our framework combines a BART-based encoder for processing demographic features with a diffusion model for trajectory generation. The system learns to generate individual trajectories that are both locally realistic (micro-plausibility) and consistent with observed population distributions (macro-consistency).
</div>

**Conditional Diffusion Model**: We use a diffusion process that gradually adds and removes noise to learn the distribution of mobility trajectories. The model is conditioned on demographic features (age, income, household size) and spatial context (residential CBG, local POI distribution), allowing it to generate trajectories that reflect individual heterogeneity.

**Aggregation-Consistency Loss**: Rather than requiring individual trajectory labels, our model learns from aggregate statistics. We enforce that generated trajectories, when aggregated, match observed CBG-POI visit flows and other population-level mobility patterns. This aggregate supervision allows us to train on widely available summary statistics while still producing detailed individual behaviors.

**Two-Stage Objective**: The framework balances two complementary goals:
- **Micro-plausibility**: Generated trajectories should exhibit realistic individual behaviors—coherent daily routines, reasonable travel distances, temporally consistent patterns
- **Macro-consistency**: Aggregated trajectories should align with observed population distributions and flow statistics

This dual objective ensures that synthetic data are both locally realistic and globally faithful to observed patterns.

## Impact and Applications

This framework addresses a critical gap in public health data infrastructure. By generating realistic, privacy-preserving mobility trajectories, it enables:

**Infectious Disease Modeling**: Simulate contact patterns and transmission dynamics without accessing sensitive individual location data

**Urban Planning and Transportation**: Test infrastructure interventions and transit policies using realistic mobility patterns across demographic groups

**Health Access and Equity**: Analyze how mobility barriers affect healthcare access for different populations, informing targeted interventions

**Policy Evaluation**: Assess potential impacts of zoning changes, new transit lines, or public health restrictions on population movement


---

*This work is an ongoing project advised by Prof. [Serina Chang](https://serinachang5.github.io/).*
