# EV Charging Attack Detection

Benchmarking a range of machine learning, deep learning, and graph neural network models for **cyberattack detection on EV charging stations (EVSE)**. Each notebook trains one model on the same power-consumption dataset to classify each reading as **normal** or **attack** (binary classification), making it easy to compare approaches on identical data.

## Task & data

The models use power-side measurements from an EV charging station to detect intrusions. Four features are used throughout:

- `shunt_voltage`
- `bus_voltage_V`
- `current_mA`
- `power_mW`

The label is binary: `1` for attack (e.g., SYN-flood, backdoor) and `0` for normal. The dataset (`EVSE-B-PowerCombined.csv`, ~115k rows) is **not** included in this repository — place your copy at the path referenced in the notebooks (`/content/EVSE-B-PowerCombined.csv` on Colab).

> **Note on scope:** These are research/experimental notebooks for comparing models, not a production intrusion-detection system.

## Models

| Notebook | Model | Family |
|----------|-------|--------|
| `EVCharging (Random Forest).ipynb` | Random Forest | Classical ML |
| `EVCharging (Linear_Reg).ipynb` | Logistic Regression | Classical ML |
| `EVCharging (Naive_Bias).ipynb` | Gaussian Naive Bayes | Classical ML |
| `EVCharging_(ANN).ipynb` | Feedforward neural network | Deep learning |
| `EVCharging_(RNN).ipynb` | Recurrent neural network | Deep learning |
| `EVCharging (GCN).ipynb` | Graph Convolutional Network | Graph NN |
| `EVCharging (GAT).ipynb` | Graph Attention Network | Graph NN |
| `EVCharging (SAGE).ipynb` | GraphSAGE | Graph NN |
| `EVCharging_(GRNN).ipynb` | GAT + RNN hybrid | Hybrid |

For the graph-based models, rows are treated as nodes connected sequentially, and the four features form each node's feature vector.

## Results

Test-set metrics as reported in each notebook (single runs):

| Model | Accuracy | F1 | ROC-AUC |
|-------|----------|-----|---------|
| Random Forest | 0.944 | 0.968 | 0.966 |
| RNN | 0.912 | 0.952 | 0.927 |
| ANN | 0.907 | 0.949 | 0.921 |
| GRNN (GAT+RNN) | 0.875 | 0.934 | 0.820 |
| GAT | 0.875 | 0.934 | 0.732 |
| GCN | 0.875 | 0.934 | 0.726 |
| GraphSAGE | 0.875 | 0.934 | 0.721 |
| Logistic Regression | 0.875 | 0.934 | 0.675 |
| Naive Bayes | 0.453 | 0.570 | 0.508 |

> The dataset is imbalanced (~88% attack), so accuracy and F1 cluster near the majority-class baseline for several models; ROC-AUC is the more informative separator. Treat these as indicative single-run figures. Random Forest comes out strongest here.

## Requirements

Developed in Google Colab. Main dependencies:

- `torch`
- `torch_geometric`
- `scikit-learn`
- `pandas`
- `numpy`

Install everything with:

```bash
pip install -r requirements.txt
```

## Getting started

1. Obtain the `EVSE-B-PowerCombined.csv` dataset and update the path in the notebook if you're not on Colab.
2. Open any notebook in Google Colab or Jupyter.
3. Run the cells top to bottom. The graph and neural-network notebooks benefit from a GPU runtime.

## Repository structure

```
.
├── EVCharging (Random Forest).ipynb
├── EVCharging (Linear_Reg).ipynb
├── EVCharging (Naive_Bias).ipynb
├── EVCharging_(ANN).ipynb
├── EVCharging_(RNN).ipynb
├── EVCharging (GCN).ipynb
├── EVCharging (GAT).ipynb
├── EVCharging (SAGE).ipynb
├── EVCharging_(GRNN).ipynb
└── README.md
```
