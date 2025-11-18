---
layout: page
title: "Offline RL for Deep Brain Stimulation Control"
description: "Developed offline reinforcement learning approaches for optimizing Deep Brain Stimulation in Parkinson's patients. Proposed a reward redistribution strategy to address sparse clinical feedback, enabling patient-specific control policies from limited observational data."
img: assets/img/dbs_temporal_mismatch.jpg
importance: 4
category: research
---

## Motivation: Personalized Treatment for Parkinson's Disease

Deep Brain Stimulation (DBS) is an FDA-approved therapy for Parkinson's disease that delivers electrical pulses to specific brain regions, helping manage motor symptoms like tremor, bradykinesia (slowness of movement), and dyskinesia (involuntary movements). However, current DBS systems use fixed stimulation parameters that do not adapt to patients' changing symptoms throughout the day.

**The vision of adaptive DBS** is to create closed-loop systems that automatically adjust stimulation based on real-time neural and behavioral signals, providing personalized treatment that responds to each patient's dynamic needs. Such adaptive systems could significantly improve symptom control while reducing side effects and extending battery life.

Reinforcement learning (RL) offers a natural framework for this problem: the system learns a control policy that maps patient states (neural signals, symptom scores) to optimal stimulation actions. However, applying RL to clinical DBS data faces fundamental challenges that standard online RL cannot address.

## The Challenge: Learning from Sparse Clinical Feedback

Developing RL-based DBS control policies from clinical data presents several interconnected challenges:

**1. Temporal Mismatch Between Actions and Rewards**

The core challenge is a severe temporal mismatch:
- **Actions (stimulation decisions)** must be made every second based on electrode recordings
- **Rewards (clinical symptom scores)** are only observed every 2 minutes from wearable devices

This creates a credit assignment problem: when we observe a symptom score after 120 seconds, which of the 120 stimulation decisions should receive credit or blame? Standard RL methods require per-step rewards and cannot handle this 120:1 temporal gap.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dbs_temporal_mismatch.jpg" title="Temporal mismatch problem" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The fundamental challenge: stimulation decisions occur every second based on 5-dimensional power band features, but symptom scores (BKS, DKS, TS) are only observed every 2 minutes. This 120:1 temporal mismatch prevents direct application of standard RL.
</div>

**2. Offline Learning Constraint**

For patient safety and ethical reasons, we cannot perform online exploration—we must learn exclusively from previously collected observational data. This offline RL setting introduces distributional shift: any learned policy will encounter states and actions not well-covered in the historical data, making evaluation and policy selection challenging.

**3. Limited Patient Data**

Clinical DBS data are expensive and difficult to collect, typically involving only a few patients with limited recording sessions. Models must learn effective policies from small datasets while generalizing to each patient's individual symptom patterns.

**4. Multi-Objective Control**

DBS must balance three competing objectives:
- Minimize bradykinesia (BKS: 0-150, lower is better)
- Minimize dyskinesia (DKS: 0-200, lower is better)  
- Minimize tremor (TS: 0-30, lower is better)

These symptoms respond differently to stimulation, and optimal control requires carefully balancing all three.

## Our Approach: Reward Redistribution for Sparse Feedback

We developed a systematic pipeline for learning DBS control policies from sparse clinical feedback through offline RL with reward redistribution.

### MDP Formulation

We formulated DBS control as a Markov Decision Process:

**State Space** (5 dimensions): Power band features from electrode recordings, measured every second across 5 frequency bands that capture neural activity patterns

**Action Space** (1 dimension): Binary stimulation control (ON/OFF) or discretized amplitude levels

**Reward Signal**: Clinical symptom scores (BKS, DKS, TS) observed every 2 minutes from wearable devices

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dbs_mdp_setup.jpg" title="MDP formulation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    MDP formulation showing the data flow: electrode power bands provide per-second states, binary stimulation provides actions, and wearable devices provide sparse 2-minute symptom scores that must be redistributed across the 120-second trajectory.
</div>

### Reward Redistribution Strategy

Our core technical contribution addresses the temporal mismatch through reward redistribution. Given a 120-step trajectory ending with a single observed reward R(τ), we learn to distribute this reward across individual time steps:

**Problem Setup**: 
- Trajectory: (s₁, a₁), (s₂, a₂), ..., (s₁₂₀, a₁₂₀)
- Observed reward: R(τ) at the end
- Goal: Estimate per-step rewards R(h) for h = 1, ..., 120

**Our Approach**:
We model the per-step reward as a function of state-action features and learn this redistribution function using an objective that balances:
1. **Consistency**: Redistributed rewards should sum to the observed trajectory reward
2. **Smoothness**: Adjacent time steps should have similar rewards (temporal coherence)
3. **State-action dependence**: Rewards should correlate with neural and stimulation patterns

This redistribution transforms the sparse feedback problem into a standard per-step reward setting where conventional offline RL algorithms can be applied.

### Offline RL Pipeline

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dbs_pipeline.jpg" title="Complete DBS RL pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Our complete pipeline: (1) Data preprocessing extracts states, actions, and rewards from DBS recordings, (2) Reward redistribution maps sparse 2-minute scores to per-second signals, (3) DBS simulator enables controlled evaluation, (4) Offline RL algorithms learn policies, (5) Off-policy evaluation selects the best policy without requiring patient interaction.
</div>

After reward redistribution, we train multiple offline RL algorithms:
- **Conservative Q-Learning (CQL)**: Penalizes Q-values for out-of-distribution actions
- **Batch-Constrained Q-Learning (BCQ)**: Restricts policy to observed action distributions
- **Neural Fitted Q-Iteration (NFQ)**: Fitted Q-iteration with neural networks
- **Deep Q-Networks (DQN, Double DQN)**: Value-based methods with experience replay
- **Soft Actor-Critic (SAC)**: Off-policy actor-critic for continuous control
- **PARTED**: Trajectory-wise reward offline RL method

### Off-Policy Evaluation and Policy Selection

A critical challenge in offline RL is selecting the best policy without deploying it on patients. We implemented comprehensive off-policy evaluation using SCOPE-RL:

**Evaluation Metrics**:
- **Mean Squared Error (MSE)**: Measures estimation accuracy of policy value
- **Regret**: Gap between estimated policy performance and optimal policy
- **Rank Correlation**: Agreement between true and estimated policy rankings
- **Statistical Significance**: P-values for rank correlation validity

**Estimators**: We evaluated multiple off-policy estimators including Direct Method (DM), Inverse Propensity Scoring (IPS), Doubly Robust (DR), and Switch estimators, with **SNPDIS** (Self-Normalized Propensity-weighted Doubly Robust with Importance Sampling) showing the best performance in identifying optimal policies.

### Results and Validation

We validated our approach on data from 2 Parkinson's patients:

**Online Evaluation in Simulator**: Policies learned with our redistributed rewards successfully controlled simulated DBS systems, achieving significant symptom reduction compared to baseline stimulation patterns.

**Policy Ranking**: Off-policy evaluation correctly identified top-performing policies, with rank correlations above 0.7 and statistically significant p-values, demonstrating that our framework can reliably select policies without patient exposure.

**Algorithm Comparison**: CQL and PARTED showed the strongest performance, effectively handling the distributional shift inherent in the offline setting.

## Future Directions

This work establishes a foundation for learning DBS control policies from sparse clinical feedback. We identified several promising directions:

**1. Multi-Objective Reward Combination**

Rather than training separate policies for each symptom, combine BKS, DKS, and TS into a unified reward: R = w₁ · BKS + w₂ · DKS + w₃ · TS, with weights learned from patient preferences or clinical priorities.

**2. Action Consistency Constraints**

Add temporal consistency penalties to encourage smooth stimulation patterns, avoiding rapid ON/OFF switching that may be clinically undesirable.

**3. Transfer Learning from Simulation to Clinic**

Pre-train policies on large-scale DBS simulator data, then fine-tune on limited patient-specific data, leveraging simulation abundance to overcome clinical data scarcity.

## Impact

This work demonstrates that offline RL with appropriate reward redistribution can learn effective control policies from sparse clinical feedback, advancing the goal of adaptive, personalized DBS therapy. The framework is generalizable to other biomedical control problems with similar temporal mismatch challenges, such as insulin delivery, drug dosing, and ventilator management.

(Code)[Link]
