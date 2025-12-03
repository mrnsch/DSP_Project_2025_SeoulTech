# DSP_Project_2025_SeoulTech

# Analyzing the Relationship between News Bias and Audience Influence

## A Data Science Practice Project

| **Course**            | Data Science Practice (DSP)                                                                                  |
| :-------------------- | :----------------------------------------------------------------------------------------------------------- |
| **University**        | SeoulTech                                                                                                    |
| **Team Members**      | Seongmin Hwang (20102127), Marion SCHMITT (25170158), Seungwon Jeon (16102288)                               |
| **GitHub Repository** | [https://github.com/mrnsch/DSP_Project_2025_SeoulTech](https://github.com/mrnsch/DSP_Project_2025_SeoulTech) |

---

## Project Objective

This project aims to quantify the use of **unethical or biased language** by major Korean news media outlets, analyze whether this linguistic behavior forms inherent **cluster structures** in the data space, and examine how these structures relate to the media outlets' real-world **audience influence** (traffic and visibility).

The core research question is:

<i>**Does the frequency of unethical language correlate with a media's influence, and can we identify distinct media archetypes based on a two-dimensional space (Bias Score, Influence Score) ?**</i>

The analysis revealed a strong tendency towards a **bipolar structure**, highlighting a fundamental cleavage between mainstream media and a highly biased fringe group.

---

## <br>Methodology and Pipeline

The analysis was executed through a rigorous three-phase pipeline, systematically comparing six model combinations (3 Dictionary Methods $\times$ 2 Clustering Algorithms) to ensure the stability and validity of the final structure.

### <br>Phase 1: Bias Dictionary Construction (NLP Feature Engineering)

Three methods were tested to create a **Bias Score**, representing different hypotheses on how biased language manifests in articles.

| Method                 | NLP Strategy                   | Rationale for Selection (Method C)                                                                                                                                                 |
| :--------------------- | :----------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Method A**           | N-gram Count (Raw Frequencies) | Prone to noise; failed to distinguish high-bias outlets clearly.                                                                                                                   |
| **Method B**           | TF-IDF (Lexical Units)         | Improved over A, but missed contextual nuances.                                                                                                                                    |
| **Method C (Optimal)** | **TF-IDF + N-grams (Bigrams)** | **Selected:** Effectively captures **linguistic patterns (framing)** and syntagmatic phrases (e.g., "shock verbal attack") that indicate bias. It showed the best data separation. |

The final Bias Score is normalized to allow comparison between media outlets of vastly different publication sizes.

### <br>Phase 2: Systematic Model Comparison

We tested all combinations of the three dictionary methods with three clustering algorithms (K-Means, Hierarchical Clustering (HAC), and DBSCAN).

**The primary evaluation criterion was the Silhouette Score,** which measures the quality and separation of the clusters.

#### Justification for K = 2 Clusters

Upon analysis of the **Silhouette Scores** across $K=2$ to $K=7$ (using Method C), a clear and dominant peak was found at $K=2$ ($\mathbf{Silhouette \approx 0.63}$).

This result is not a model failure but a reflection of the data's **intrinsic structure**: the media population consists of a large main group of **moderate values** and a small group of **extreme outliers** in both Bias and (Negative) Influence dimensions.

- **Result:** The data naturally splits into two, representing a stable, bipolar structure.

### <br>Phase 3: Final Model Selection

The optimal model selected for final interpretation based on the highest Silhouette Score and consistency with HAC was:

| Characteristic                       | Value                    |
| :----------------------------------- | :----------------------- |
| **Dictionary Method**                | Method C (TF-IDF N-gram) |
| **Clustering Algorithm**             | K-Means                  |
| **Optimal Number of Clusters ($K$)** | **2**                    |
| **Final Silhouette Score**           | **0.6330**               |

---

## <br>Key Results and Findings

### 1. Bias-Influence Correlation

A moderate **negative correlation** was found between the Bias Score (Method C) and the Influence Score: $\mathbf{r = -0.351}$.

**Interpretation:** On average, media outlets that exhibit a higher use of unethical and biased language tend to have lower audience influence/traffic. This suggests that linguistic quality and audience trust are statistically linked in this ecosystem.

### <br>2. Cluster Archetypes

The final K-Means (K=2) model reveals two distinct groups:

| Cluster       | N (Media Outlets) | Bias Score (Mean) | Influence Score (Mean) | Archetype                 |
| :------------ | :---------------- | :---------------- | :--------------------- | :------------------------ |
| **Cluster 0** | 16                | Low / Moderate    | Positive / Medium      | **Linguistic Mainstream** |
| **Cluster 1** | 2                 | **Very High**     | **Strongly Negative**  | **Extreme Fringe**        |

This structure vividly illustrates the landscape: a large, homogeneous group of established media coexisting with a small, distinct group characterized by both high linguistic bias and low market visibility.

### <br>3. Key Data Points

- **Most Biased Media:** 시사IN ($\approx 0.5024$)
- **Least Biased Media:** OBS ($\approx 0.0221$)
- **Most Influential:** 조선일보 ($\approx 3.2786$)

#### <br>Visualizations

The main finding is best represented by the scatter plot:

---

## <br>Repository Structure

The core pipeline is sequential across the three main notebooks:

| File/Folder                                  | Description                                                                                                                                                                                                                 |
| :------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Project_DSP_methodABC_visualization.ipynb`  | Initial data cleaning, feature engineering, and generation of the three Bias Scores (Methods A, B, C) and Influence Scores.                                                                                                 |
| `Dictionary_And_Clustering_Comparison.ipynb` | Systematic execution of the **6 model combinations** and storage of all evaluation metrics in `comparison_metrics.csv`.                                                                                                     |
| `Final_Model_Selection.ipynb`                | Loads comparison data, selects the optimal model, performs final K=2 clustering, generates all final plots (`03_best_model_clustering.png`, `04_cluster_radar_chart.png`), and extracts key insights to `key_findings.txt`. |
| `DSPproposal_20102127HwangSeongmin.pdf`      | Original project proposal.                                                                                                                                                                                                  |
| `comparison_metrics.csv`                     | Output file containing Silhouette scores and other metrics for all 6 model trials.                                                                                                                                          |

---

## <br>Setup and Execution

### Requirements

This project requires a standard Python data science environment.

- Python 3.x
- `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`
- Korean NLP libraries (e.g., `konlpy` or similar, depending on the dictionary construction)

### <br>Installation

```bash
# Clone the repository
git clone [https://github.com/mrnsch/DSP_Project_2025_SeoulTech](https://github.com/mrnsch/DSP_Project_2025_SeoulTech)
cd DSP_Project_2025_SeoulTech
```
