# CSC173 Data Mining Project Proposal
**Student:** John Christian Niño T. Abuel, 2022-0423 
**Date:** December 10, 2025

# CSC173 Data Mining Project Proposal

## 1. Project Title
**Seismic Patterns of the Pacific Ring: Association Rule Mining on Philippine Earthquake Clusters**

## 2. Problem Statement
The Philippines is geographically situated in the Pacific Ring of Fire, making it prone to frequent seismic activity. While standard monitoring focuses on predicting the *time* or *location* of the next earthquake, there is limited analysis on the **co-occurrence of seismic attributes**. We do not easily know, for example, if specific depth ranges in Mindanao are statistically associated with higher magnitude events compared to Luzon. Understanding these hidden "rules" (e.g., "If Depth is Shallow AND Region is Davao, then Magnitude is likely > 5.0") can help in profiling regional risk distinct from simple frequency counts.

## 3. Objectives
* To preprocess raw PHIVOLCS data by discretizing continuous variables (Magnitude, Depth) into categorical "items" suitable for mining.
* To apply the **Apriori algorithm** to discover strong association rules between earthquake location, depth, magnitude, and time of occurrence.
* To compare "Seismic Baskets" across the three major island groups (Luzon, Visayas, Mindanao) to see if they follow different association patterns.
* To visualize the resulting rules using network graphs to identify the most critical precursors to high-magnitude events.

## 4. Dataset Plan
* **Source:** [Philippine Earthquakes Dataset (Kaggle)](https://www.kaggle.com/datasets/bwandowando/philippine-earthquakes-from-phivolcs)
* **Classes (Attributes):**
    * *Latitude/Longitude* (To be converted into Regions/Provinces).
    * *Depth* (To be binned: Shallow, Intermediate, Deep).
    * *Magnitude* (To be binned: Micro, Light, Moderate, Strong, Major).
    * *Date/Time* (To be binned: Morning, Afternoon, Evening, Night).
* **Acquisition:** The dataset will be ingested via the Kaggle API into the Google Colab environment.

## 5. Technical Approach
* **Architecture Sketch:**
    1.  **Input:** Raw CSV Data.
    2.  **Preprocessing:** Binning continuous values (e.g., Mag 5.2 -> "Moderate") -> One-Hot Encoding.
    3.  **Mining:** Apriori Algorithm (Generate Frequent Itemsets -> Generate Rules).
    4.  **Filtering:** Pruning rules based on Lift > 1.0 and Confidence > 50%.
    5.  **Output:** Top 10 Association Rules & Network Graph Visualization.
* **Model:** Apriori Algorithm.
* **Framework:** Python (Libraries: `pandas`, `mlxtend`, `matplotlib`/`seaborn`).
* **Hardware:** Google Colab (Cloud CPU).

## 6. Expected Challenges & Mitigations
* **Challenge: Continuous Variables.**
    Apriori cannot handle raw numbers (e.g., depth = 123km).
    * **Solution:** I will implement **Discretization (Binning)**. I will map depths to standard geological categories (0-70km = Shallow, etc.) to turn them into categorical "items."
* **Challenge: Rare Item Problem.**
    High magnitude earthquakes (>6.0) are rare, so they have very low "Support" and might be filtered out by the algorithm.
    * **Solution:** I will set a very **low Minimum Support threshold** (e.g., 0.001) but filter strictly by **Lift** (correlation). This ensures rare but significant patterns are kept while common noise is ignored.
