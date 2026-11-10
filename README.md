# US Arrests: PCA and Clustering (Unsupervised Learning)

A Jupyter Notebook project that applies unsupervised learning to US state-level crime data. It reduces the data with Principal Component Analysis (PCA), then groups the states with two clustering techniques, K-means and agglomerative (hierarchical) clustering, and compares the results.

## Project Structure

```
.
├── unsupervised_task.ipynb   # Jupyter Notebook with the full analysis
├── UsArrests.csv             # Dataset (50 rows x 5 columns)
└── README.md
```

## Dataset

`UsArrests.csv` contains one row for each of the 50 US states. It matches the classic USArrests dataset (violent crime arrests per 100,000 residents, plus the percentage of the population living in urban areas).

| Column | Description |
|--------|-------------|
| `City` | State name (the column is labelled `City`, but each row is a US state) |
| `Murder` | Murder arrests per 100,000 residents |
| `Assault` | Assault arrests per 100,000 residents |
| `UrbanPop` | Percentage of the population living in urban areas |
| `Rape` | Rape arrests per 100,000 residents |

There are no missing values. The four numeric columns are on very different scales (for example `Assault` ranges up to 337, while `Murder` tops out at 17.4), which is why the features are standardised before PCA and clustering.

## What the Notebook Does

1. **Exploration:** shape, missing values, data types and summary statistics.
2. **Correlation analysis:** a heatmap of the correlations between the four features.
3. **PCA:** standardises the features with `StandardScaler`, runs PCA, and plots a biplot of the first two components.
4. **Choosing components:** uses the explained-variance table and a scree plot to decide how many components to keep.
5. **K-means clustering:** picks the number of clusters with silhouette scores, fits the model, plots the clusters on the PCA axes and profiles each cluster.
6. **Agglomerative clustering:** draws a Ward-linkage dendrogram, fits the model with the same number of clusters, plots it and profiles each cluster.
7. **Comparison:** checks how closely the two methods agree.

## Results

**Correlations**

- `Murder` and `Assault` are strongly correlated (r = 0.80).
- `Assault` and `Rape` are fairly strongly correlated (r = 0.67), and `Murder` and `Rape` moderately so (r = 0.56).
- `UrbanPop` has little relationship with `Murder` (r = 0.07) and only a weak to moderate one with the other crimes.

**PCA**

| Component | Variance explained | Cumulative |
|-----------|-------------------:|-----------:|
| PC1 | 62.0% | 62.0% |
| PC2 | 24.7% | 86.8% |
| PC3 | 8.9% | 95.7% |
| PC4 | 4.3% | 100% |

The first two components capture about 87% of the variance, and the scree plot elbows sharply after PC2, so two components were kept.

- **PC1** reflects overall violent crime: `Murder`, `Assault` and `Rape` all load onto it in the same direction.
- **PC2** is mostly urbanisation, driven by `UrbanPop`.

**Clustering (on the two principal components)**

| Number of clusters (k) | Silhouette score (K-means) |
|-----------------------:|---------------------------:|
| **2** | **0.476** |
| 3 | 0.401 |
| 4 | 0.444 |
| 5 | 0.408 |
| 6 | 0.396 |
| 7 | 0.373 |

k = 2 scored highest, so both methods use two clusters. Cluster averages in the original units:

| Cluster | Murder | Assault | UrbanPop | Rape |
|---------|-------:|--------:|---------:|-----:|
| K-means, lower-crime group | 4.87 | 114.43 | 63.63 | 15.94 |
| K-means, higher-crime group | 12.16 | 255.25 | 68.40 | 29.16 |
| Agglomerative, lower-crime group | 5.00 | 116.48 | 63.84 | 16.34 |
| Agglomerative, higher-crime group | 12.33 | 259.32 | 68.32 | 29.22 |

Both methods split the states into a lower-crime group (around 30 states) and a higher-crime group (around 20 states). The agglomerative model has a silhouette score of 0.470, very close to K-means. The two methods assign almost every state to the same group (in a re-run, only Missouri differed), which suggests the grouping reflects real structure in the data rather than one algorithm's quirks.

## Requirements

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab
- numpy
- pandas
- seaborn
- matplotlib
- scikit-learn
- scipy

## Getting Started

```bash

# 1. (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate

# 2. Install dependencies
pip install numpy pandas seaborn matplotlib scikit-learn scipy jupyter

# 3. Launch the notebook
jupyter notebook unsupervised_task.ipynb
```

Keep `UsArrests.csv` in the same folder as the notebook, since it is loaded with a relative path. Then run all cells in order (**Kernel > Restart & Run All**).

## Author

**Siboniso Zikhali**
