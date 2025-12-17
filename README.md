# Association Rule Mining on Philippine Earthquake Clusters (2019-2025)
**CSC173 Intelligent Systems Final Project** *Mindanao State University - Iligan Institute of Technology* **Student:** John Christian Niño T. Abuel  
**Semester:** AY 2025-2026 Sem 1

[![Python](https://img.shields.io/badge/Python-3.13+-blue)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/Status-Completed-success)]()

## Abstract
The Philippines is a seismically active archipelago, yet standard monitoring often focuses on frequency counts rather than behavioral patterns. This project utilizes **Association Rule Mining (Apriori Algorithm)** on Philippine earthquake data from the "Modern Seismic Epoch" (2019–2025). By implementing Apriori twice on different parameters, the study successfully differentiates between low-risk "Swarm Zones" (e.g., Cebu, Batangas) and high-energy "Hazard Zones" (e.g., Davao Occidental, Surigao del Sur). The analysis uncovers distinct "Seismic Fingerprints," moving beyond simple hazard maps to reveal the probabilistic rules governing regional seismicity.

## Table of Contents
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Methodology](#methodology)
- [Experiments & Results](#experiments--results)
- [Discussion](#discussion)
- [Ethical Considerations](#ethical-considerations)
- [Conclusion](#conclusion)
- [Installation](#installation)
- [References](#references)

## Introduction
### Problem Statement
Seismic data is often presented as isolated points on a map (hypocenters). While this indicates *where* earthquakes happen, it fails to capture *how* they behave. Does a region experience harmless micro-swarms, or is it statistically prone to destructive high-energy release? Standard statistical analysis often buries rare, high-magnitude events under the noise of thousands of micro-earthquakes, creating a "blind spot" for major hazard detection. This also poses a problem for people that try to settle into an area 

### Objectives
- **Bin & Transform:** Convert continuous physics data (Magnitude, Depth, Time) into categorical "seismic baskets" suitable for rule mining.
- **Dual-Pass Mining:** Implement a two-tiered Apriori approach to overcome the Gutenberg-Richter Law:
    - *Pass 1:* Detect high-frequency swarm behaviors.
    - *Pass 2:* Detect rare, high-impact events (Moderate/Strong quakes).
- **Profile Risk:** Classify provinces into "Swarm Zones" vs. "Hazard Zones" based on Lift and Confidence metrics.
- **Visualize:** Map the "Risk Landscape" using network graphs and scatter plots.

## Related Work
- *PHIVOLCS (Philippine Institute of Volcanology and Seismology).* Annual Seismic Bulletins. (Provides the raw data but primarily focuses on descriptive statistics).
- *Gutenberg, B., & Richter, C. F. (1954).* Seismicity of the Earth and Associated Phenomena. (Establishes the frequency-magnitude power law, which this project addresses algorithmically).
- *Agrawal, R., & Srikant, R. (1994).* Fast algorithms for mining association rules. (The foundational text for the Apriori algorithm used here).

*(GAP: While seismology uses physics-based models, few studies apply Association Rule Mining to create probabilistic risk profiles for specific Philippine provinces.)*

## Methodology
### Dataset
- **Source:** USGS / PHIVOLCS Consolidated Data
- **Period:** 2019–2025 (Selected to capture the modern tectonic stress cycle post-2019 Cotabato series and upgraded sensor networks).
- **Preprocessing:**
    - **One-Hot Encoding:** Converted categorical data into binary vectors.
    - **Binning Strategy:**
        - *Magnitude:* Micro (<3), Minor (3-4.9), Moderate (5-5.9), Strong (6+).
        - *Depth:* Surface (0-15km), Crust (15-40km), Interface/Deep (>40km).
        - *Time:* Morning, Afternoon, Evening, Night (to check for detection bias).

### Dual-Pass Apriori Strategy

| Parameter | Pass 1 (Swarms) | Pass 2 (Big Quakes) |
|-----------|-----------------|---------------------|
| **Objective** | Find common, low-risk patterns | Find rare, high-risk hazards |
| **Min Support** | `0.01` (1%) | `0.0001` (0.01%) |
| **Lift Threshold** | `> 1.1` | `> 1.0` (Sorted by Max Lift) |
| **Outcome** | 80 High-Confidence Rules | 148 High-Lift Rules |

### Implementation Code Snippet
```python
# Pass 2: Capturing the "Big Ones"
# Drastically lower support to overcome Gutenberg-Richter Law
frequent_items = apriori(df_encoded, min_support=0.0001, use_colnames=True)

# Generate Rules
rules = association_rules(frequent_items, metric="lift", min_threshold=1.0)

# Filter for High Impact Targets
targets = ['Magnitude_Moderate', 'Magnitude_Strong', 'Magnitude_Major']
big_quake_rules = rules[rules['consequents'].apply(lambda x: any(t in list(x) for t in targets))]


### Demo
![Detection Demo](demo/detection.gif)
[Video: [CSC173_YourLastName_Final.mp4](demo/CSC173_YourLastName_Final.mp4)] [web:41]

## Discussion
- Strengths: Decently tells us about the correlation and co-occurence of some mental health variables (anxiety and depression) and the most common favorite music genre (rock)

- Limitations: Very high dimensionality of dataset after preprocessing and one-hot encoding.

- Insights: 
    - Pruned itemsets with support below 0.1 or 1%. 
    - Excluded low power columns/high cardinality features
    - Binned some numerical features to reduce categories
