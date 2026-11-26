# Explainable Graph Neural Network for Cyberattack Detection

Undergraduate Final-Year Project

This project develops an explainable Graph Neural Network (GNN) based system for detecting cyberattacks in network traffic using the UNSW-NB15 dataset. The system not only classifies network flows as normal or malicious but also provides explanations for its predictions using GNNExplainer.

---

## Project Overview

Traditional machine learning models treat each network flow independently. In contrast, this project models network traffic as a graph, allowing the model to learn from relationships between similar flows.

Key Objectives:
- Detect cyberattacks (binary classification: Normal vs Attack)
- Compare Graph Convolutional Network (GCN) and GraphSAGE
- Provide model explanations using GNNExplainer
- Benchmark against a traditional MLP baseline

---

## Dataset

UNSW-NB15 (Australian Centre for Cyber Security)

| Split              | Records  |
|--------------------|----------|
| Training Set       | 175,341  |
| Testing Set        | 82,332   |
| Total          | 257,673 |

- Contains normal traffic and 9 attack categories (Fuzzers, Analysis, Backdoors, DoS, Exploits, Generic, Reconnaissance, Shellcode, Worms)
- Official train/test split is used for fair benchmarking
- Note: Official train/test files do not contain source/destination IP addresses. Therefore, a flow-similarity graph is constructed instead of a real communication topology graph.

---

## Methodology

### 1. Preprocessing
- Drop leakage columns (id, label, attack_cat)
- Handle missing values
- Standardize numerical features
- One-hot encode categorical features (proto, service, state, etc.)
- Fit preprocessor only on training data

### 2. Graph Construction
Because IP addresses are unavailable in the official split, a K-Nearest Neighbors (KNN) similarity graph is built:

- Nodes → Individual network flows
- Edges → Connections between flows with similar feature vectors (cosine similarity)
- Separate graphs are constructed for training and testing sets to prevent data leakage

### 3. Models
| Model       | Description                          |
|-------------|--------------------------------------|
| GCN     | Graph Convolutional Network          |
| GraphSAGE | Inductive GraphSAGE                |
| MLP     | Multi-Layer Perceptron (baseline)    |

### 4. Explainability
GNNExplainer is used to identify:
- Most important node features
- Important neighboring connections that influenced the prediction

---

## How to Run (Google Colab)

1. Open the notebook in Google Colab.
2. Mount Google Drive and upload the dataset files:
   - UNSW_NB15_training-set.csv
   - UNSW_NB15_testing-set.csv
3. Run the cells sequentially.
4. Recommended runtime: GPU (if available).

### Sample Size Used in Experiments
To keep memory usage manageable on free Colab:

- Training sample: 10,000 flows  
- Testing sample: 3,000 flows  

(Stratified sampling is used to preserve class distribution.)

---

## Results (Sample Experiments)

| Model       | Accuracy | Precision | Recall | F1-Score |
|-------------|----------|-----------|--------|----------|
| MLP (Baseline) | 0.874   | 0.859     | 0.923  | 0.890    |
| GraphSAGE      | 0.857   | 0.825     | 0.940  | 0.879    |

> Note: Results are based on the 10k/3k sample. Performance may improve with larger samples or better graph construction.

---

## Key Features of the Implementation- Proper train/test separation (no data leakage)
- Stratified sampling
- Early stopping during training
- Comprehensive evaluation metrics (Accuracy, Precision, Recall, F1, Classification Report, Confusion Matrix)
- Feature importance visualization via GNNExplainer
- Clean modular notebook structure

---

## Limitations

1. Graph Type: Uses a feature-similarity graph instead of a real IP-based communication graph (due to missing IP addresses in the official split).
2. Sample Size: Experiments were conducted on a subset of the data due to Colab memory constraints.
3. Class Imbalance: The dataset is imbalanced (more attack samples than normal in some splits).

---

## Future Improvements

- Use the original UNSW-NB15 CSV files (with IP addresses) to build a real communication graph
- Experiment with larger sample sizes or approximate nearest neighbors (FAISS)
- Add class weights or focal loss to handle imbalance
- Compare additional GNN architectures (GAT, Graph Transformer)
- Extend to multi-class attack category classification
- Deploy a simple demo interface

---

## Requirements

torch
torch-geometric
numpy
pandas
scikit-learn
matplotlib
seaborn
joblib
Install in Colab:
!pip install -q torch-geometric
---

## Author

Tabassum
Undergraduate Final-Year Project  

---

## Acknowledgments

- UNSW Canberra / Australian Centre for Cyber Security for the UNSW-NB15 dataset
- PyTorch Geometric team
- GNNExplainer authors

---

## License

This project is developed for academic purposes.
