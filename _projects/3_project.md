---
layout: page
title: "Reproducibility in Business Statistics: A Systematic Assessment"
description: "Led a comprehensive reproducibility audit of 235 articles across seven leading statistics journals, revealing that only 17% of results could be reproduced. Identified critical barriers in code and data accessibility, reinforcing the need for transparent reporting in empirical sciences."
img: assets/img/figure1_reproducibility.jpg
importance: 3
category: research
giscus_comments: true
---

## Motivation: The Reproducibility Crisis in Science

Reproducibility constitutes a foundational principle of scientific research, serving as a critical mechanism for ensuring the reliability and validity of empirical findings. When research cannot be reproduced, it undermines public trust in science, wastes resources on building upon faulty findings, and slows scientific progress across disciplines.

Recent evidence reveals concerning patterns across scientific domains: only 15.1% of fMRI analyses in statistics articles were reproducible, social work faces a documented reproducibility crisis, and management science struggles with inadequate data access despite improved policies. These failures stem from insufficient documentation, inappropriate methods, publication bias, and lack of standardized practices for sharing code and data.

**The stakes are particularly high in business and economic statistics**, where research findings directly inform policy decisions, investment strategies, and organizational practices affecting millions of people. Yet despite clear journal policies requiring data and code sharing, the actual state of reproducibility remains largely unknown.

## The Challenge: Multiple Barriers to Reproducibility

Understanding the current state of reproducibility in business statistics requires confronting several fundamental challenges:

**1. Inadequate Data Sharing Practices**

Researchers frequently provide only simulation data or public download links to raw datasets, rather than the preprocessed data actually used in analyses. Without preprocessing scripts or clear instructions, other researchers cannot reconstruct the analytical dataset, making reproduction impossible regardless of code availability.

**2. Incomplete or Undocumented Code**

When code is shared, it often lacks critical components: missing functions, unclear execution pipelines, absent annotations, or dependencies on proprietary software. Many authors provide R packages without the specific scripts used in their articles, creating a gap between available tools and reproducible results.

**3. Execution Failures**

Even when both code and data are provided, technical issues frequently prevent reproduction: memory exhaustion, unrecognized functions, software version incompatibilities, corrupted files, and dependency conflicts. These barriers exist even for researchers with strong computational skills.

**4. Inconsistent Journal Policies**

While some journals mandate code and data sharing, others merely "encourage" or "strongly recommend" it. This variation creates confusion about expectations and results in inconsistent compliance across the field. Even journals with strict policies face enforcement challenges.

## Our Approach: Systematic Reproducibility Assessment

We conducted the first comprehensive reproducibility study specifically examining business statistics research, analyzing **235 articles published in seven prominent journals between January 2021 and July 2023**:

- Annals of Applied Statistics (AOAS)
- Annals of Statistics (AOS)  
- Journal of Business and Economic Statistics (JBES)
- Journal of Computational and Graphical Statistics (JCGS)
- Journal of the American Statistical Association (JASA)
- Journal of the Royal Statistical Society Series C (JRSS,C)
- Journal of Time Series Analysis (JTSA)

### Methodology

**Systematic Article Selection**: Multiple independent reviewers examined each issue within the timeframe, identifying all articles using business, economic, or financial data. We selected business data because it is more generalizable than specialized datasets (e.g., fMRI) and more openly accessible than proprietary data.

**Multi-Dimensional Reproducibility Assessment**: Rather than a binary reproducible/not reproducible classification, we evaluated three dimensions:
- **Table reproducibility**: Can the numerical results in tables be regenerated?
- **Figure reproducibility**: Can the visualizations be recreated?
- **Numerical results**: Can specific values mentioned in text be reproduced?

**Code Functionality Classification**: We categorized code as:
- **Working**: Results match published findings (allowing <5% relative error for numerical values)
- **Partially working**: Some results match, but with notable discrepancies or incompleteness
- **Not working**: Unable to reproduce any results due to errors, missing components, or data issues

## Results: A Sobering Picture of Reproducibility

### Overall Findings

Of 235 articles examined, **only 39 (17%) could be fully or partially reproduced**. These articles provided both executable (or partially executable) code and real data, with results matching (or partially matching) the published findings.

**Key Statistics**:
- **Data Availability**: Only 87 articles (37%) provided real data in usable format
- **Code Availability**: 121 articles (51%) provided some form of code
- **Fully Working Code**: Only 22 (9%) articles
- **Partially Working Code**: 17 (7%) articles  
- **Non-Functional Code**: 82 (35%) articles
- **No Materials**: 109 articles (46%) provided neither data nor code

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/figure1_reproducibility.jpg" title="Data and code availability across journals" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The availability and reproducibility of data and computer code across seven statistics journals from January 2021 to July 2023. Journal of Computational and Graphical Statistics (JCGS) showed the highest rates of data provision (82%) and code provision (100%), while most other journals showed significantly lower compliance with reproducibility standards.
</div>

### Journal-Specific Performance

Performance varied substantially across journals, revealing the impact of different policies and enforcement:

**Best Performer - JCGS**: Despite a small sample (11 articles), JCGS achieved the highest reproducibility rate with 82% providing real data and 100% providing code. Their policy states authors "are expected to submit code and datasets" with few exceptions.

**Mandatory Requirements - JASA**: Even with explicit requirements that "all invited revisions must include code, data, and workflow," only 60% of JASA articles provided real data and 80% provided code, with just 16% fully reproducible.

**Lenient Policies - AOS**: With the most permissive policy (authors "may upload" materials), AOS showed predictably poor results: only 17% provided data, 42% provided code, and 0% were reproducible.

### Code Execution Results

For articles that provided both code and data, we assessed whether the code actually worked:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/figure2_execution.jpg" title="Code execution success rates" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Breakdown of code execution success for articles that supplied both code and data. The figure shows overall reproducibility as well as reproducibility for tables, figures, and numerical results separately. JCGS again demonstrated the strongest performance with 27% of code fully working and 36% partially working for overall reproducibility.
</div>

**Common Failure Modes**:

- **Execution Errors (35% of articles with code)**: Memory exhaustion, unrecognized functions, version incompatibilities
- **Incomplete Materials (32%)**: Missing key scripts, absent preprocessing code, R packages without article-specific scripts
- **Data Issues (28%)**: Unavailable data files, incompatible formats, raw data without processing instructions
- **Documentation Gaps (25%)**: Unannotated code, missing usage instructions, unclear execution pipelines

### Dimensional Analysis

Breaking down by type of result:

**Table Reproducibility** (199 articles with tables): Only 9% fully reproducible, 3% partially reproducible

**Figure Reproducibility** (193 articles with figures): Only 11% fully reproducible, 6% partially reproducible

**Numerical Results** (38 articles with specific values in text): Only 8% fully reproducible, 3% partially reproducible

## Key Insights and Implications

**1. Journal Policies Matter, But Enforcement Matters More**

The stark differences between journals demonstrate that merely stating requirements is insufficient. JCGS's success suggests that consistent enforcement and editorial oversight significantly improve reproducibility rates.

**2. The "Last Mile" Problem**

Many researchers share code and data but fail at the final step: ensuring materials actually work. This suggests the need for pre-publication verification systems or reproducibility editors who test materials before acceptance.

**3. Data Accessibility Is the Primary Bottleneck**

While 51% of articles provided some code, only 37% provided usable real data. Even sophisticated code is useless without the data it operates on, making data sharing the critical constraint.

**4. Documentation Quality Matters as Much as Availability**

Providing code without clear instructions, annotations, or environment specifications creates a reproducibility theater—the appearance of openness without actual reproducibility.

## Impact and Recommendations

This work provides the first comprehensive assessment of reproducibility specifically in business statistics, extending beyond previous specialized studies to a more generalizable domain.

**For Journal Editors**:
- Implement mandatory pre-publication verification of code and data
- Establish reproducibility editors or review teams
- Enforce existing policies consistently

**For Researchers**:
- Share preprocessed data, not just raw data or simulation data
- Annotate code thoroughly with clear execution instructions  
- Test reproducibility on a clean system before submission

**For the Statistical Community**:
- Develop field-specific standards for reproducible research
- Create infrastructure supporting "frictionless reproducibility"
- Establish incentives recognizing reproducibility efforts

## Publications & Resources

**Reproducibility in Business Statistics**  
Jessie Zixin Li, Ruizhu Zhou, [Ivor Cribben](https://apps.ualberta.ca/directory/person/cribben)  
*Under Review at The Review of Economics and Statistics*, 2025

[Paper PDF](link-to-paper)
