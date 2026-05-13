# Report: Exploratory Clustering of IPIP-NEO Personality Response Patterns

## Introduction

This project explored whether personality response patterns in the IPIP-NEO dataset naturally form discrete clusters, or whether they are better understood as part of a continuous multidimensional structure. The broader motivation was to examine, from an unsupervised learning perspective, whether human personality data organizes itself into clearly separated groups, and whether this organization aligns in any meaningful way with traditional personality frameworks such as the Big Five.

It is important to clarify that clustering people is not the same as testing whether the Big Five dimensions are valid. The Big Five is primarily a dimensional model, whereas clustering seeks natural groupings of individuals. Therefore, this analysis was designed to investigate whether participant-level response patterns suggest distinct personality “types,” or whether the data behaves more like a continuous cloud of variation.

## Data Source and Preparation

The starting point was the `IPIP120.dat` file, whose structure was reconstructed using the documentation in `DAT120.doc`. The corresponding scoring materials from Johnson (2014) were also reviewed to understand the organization of the inventory and the handling of reverse-scored items.

### Preprocessing steps completed

- Loaded the fixed-width format dataset correctly
- Extracted the 120 survey item responses
- Removed records with missing or invalid item values
- Filtered participants to age 18 and older
- Verified score ranges
- Checked for duplicate rows and found none
- Retained the dataset as provided, without applying an additional reverse-scoring step, because the documentation indicates that reverse-scored items had already been recoded in the distributed data

### Final cleaned dataset

- **Rows:** 339,977 participants
- **Columns:** 130
- **Countries represented:** 238
- **Gender distribution:** 58.37% female, 41.63% male

## Method

The main analysis treated each participant as a vector in a high-dimensional response space.

### Representation 1

- 120 standardized item-response variables
- Each item standardized by column to mean 0 and standard deviation 1

### Representation 2

- Alternative analysis using unreversed items and 300 dimensions
- This was treated as a comparison test to see whether a different data representation produced clearer clustering structure

### Clustering procedure

K-Means clustering was applied across multiple values of `k` to test whether the data showed evidence of natural partitioning.

### Metrics used

- **Inertia / Elbow Method**
- **Silhouette Score**
- **Davies-Bouldin Index**
- **Calinski-Harabasz Index**

These metrics were used to evaluate whether any value of `k` produced meaningful internal cluster structure.

---

## Results

## 1. Main analysis using 120 standardized items

### Elbow method

K-Means was first tested from `k = 3` to `k = 20`.

The inertia curve decreased smoothly across the full range, with no strong or definitive elbow. A mild bend was visually noticeable around `k = 5` to `k = 6`, which is loosely compatible with the theoretical Big Five structure, but the decline continued steadily beyond that point.

This suggests that the data does not naturally separate into a small number of sharply distinct clusters.

### Internal validation metrics

For `k = 3` to `k = 10`:

#### Silhouette Scores

- k=3 → 0.0599
- k=4 → 0.0476
- k=5 → 0.0455
- k=6 → 0.0373
- k=7 → 0.0343
- k=8 → 0.0326
- k=9 → 0.0275
- k=10 → 0.0260

These values are very low overall, indicating substantial overlap between clusters. The best value occurred at `k = 3`, but even this was weak.

#### Davies-Bouldin Index

- k=3 → 3.3927
- k=4 → 3.4074
- k=5 → 3.3555
- k=6 → 3.5092
- k=7 → 3.5525
- k=8 → 3.6574
- k=9 → 3.6404
- k=10 → 3.4814

The best value was at `k = 5`, but differences were small. No cluster solution stood out clearly.

#### Calinski-Harabasz Index

- k=3 → 718.76
- k=4 → 583.06
- k=5 → 493.87
- k=6 → 429.07
- k=7 → 382.44
- k=8 → 342.89
- k=9 → 314.59
- k=10 → 293.18

This metric clearly favored `k = 3`, with values declining as `k` increased.

### Interpretation of main analysis

The metrics did not converge on a strong, well-separated cluster solution. At best:

- `k = 3` received the strongest support from silhouette and Calinski-Harabasz
- `k = 5` had only a slight advantage in Davies-Bouldin and a mild visual hint in the elbow curve

However, the silhouette values were uniformly very low, which is the strongest sign that the data does not naturally form clear personality “types.”

---

## 2. Alternative analysis using unreversed items and 300 dimensions

A second clustering analysis was conducted using an alternative representation of the data in order to test whether the weak clustering results depended heavily on the initial preprocessing choices.

### Inertia results

Tested for odd values from `k = 3` to `k = 19`:

- k=3 → 40,071,094.80
- k=5 → 38,527,419.53
- k=7 → 37,622,013.63
- k=9 → 37,013,790.96
- k=11 → 36,525,965.39
- k=13 → 36,157,884.39
- k=15 → 35,849,381.12
- k=17 → 35,589,171.99
- k=19 → 35,376,111.86

Again, inertia decreased smoothly with no sharp elbow.

### Internal validation metrics

- k=3 → Silhouette: 0.0508 | Davies-Bouldin: 3.6694 | Calinski-Harabasz: 7333.7014
- k=5 → Silhouette: 0.0379 | Davies-Bouldin: 3.6719 | Calinski-Harabasz: 4988.5572
- k=7 → Silhouette: 0.0301 | Davies-Bouldin: 3.7191 | Calinski-Harabasz: 3876.1179
- k=9 → Silhouette: 0.0234 | Davies-Bouldin: 3.6788 | Calinski-Harabasz: 3195.7161
- k=11 → Silhouette: 0.0209 | Davies-Bouldin: 3.7503 | Calinski-Harabasz: 2747.3113
- k=13 → Silhouette: 0.0196 | Davies-Bouldin: 3.7175 | Calinski-Harabasz: 2412.1905

### Interpretation of alternative analysis

The alternative representation did not materially improve the clustering structure.

- Silhouette remained very low
- Davies-Bouldin showed little meaningful separation
- Calinski-Harabasz again favored `k = 3`

This indicates that changing the representation to unreversed items and a larger dimensional space did not reveal strong latent clusters.

---

## Discussion

Across both representations, the clustering structure remained weak. The data did not show strong evidence of well-separated groups of people. Instead, it behaved more like a continuous, overlapping distribution in high-dimensional space.

This does not necessarily mean that the Big Five is arbitrary. Rather, it suggests that:

- personality may not naturally divide into discrete participant “types”
- broad personality structure may be better represented dimensionally than categorically
- if clusters are imposed, they are likely best interpreted as pragmatic summaries rather than naturally occurring psychological classes

The weak visual hint near `k = 5` may reflect some broad structure compatible with the Big Five, but the internal validation metrics do not support the existence of five sharply separated natural groups.

## Conclusions

1. **No strong natural clustering structure was found** in the IPIP-NEO participant response data.

2. **The elbow method did not reveal a clear elbow**, even when extended to larger values of `k`.

3. **Silhouette scores were consistently very low**, indicating substantial overlap between candidate clusters.

4. **Calinski-Harabasz repeatedly favored `k = 3`**, while **Davies-Bouldin showed only weak and inconsistent differences**, providing no decisive support for a clean cluster solution.

5. **Alternative data representations did not materially improve the results.** Using unreversed items and 300 dimensions still produced weak clustering.

6. **The findings support a continuous interpretation of personality variation** rather than a discrete “personality types” interpretation.

7. **These results do not show that the Big Five is arbitrary.** They suggest instead that personality may be better represented as a continuous multidimensional structure, of which the Big Five may be a useful broad summary rather than a set of natural, sharply bounded groups.

## Final Statement

The present unsupervised learning analysis does not support the view that IPIP-NEO personality responses naturally fall into a small number of clearly separated clusters. The results are more consistent with personality existing as a continuous multidimensional space, where any chosen cluster solution functions more as a descriptive simplification than as a discovery of true discrete personality types.
