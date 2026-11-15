# Explainable AI for Cyber Threat Detection

### An Offline Network Intrusion Detection Prototype Using Random Forest, GCN, SHAP and GNNExplainer

## 1. Project Overview

Cyber threats such as denial-of-service attacks, scanning, and brute-force attempts can compromise network security. Machine Learning can help identify suspicious network activity, but predictions alone do not explain why a particular flow is classified as malicious.

This project develops an **Explainable AI (XAI)-based cyber threat detection prototype** using the CICIDS2017 dataset.

It compares two machine-learning approaches:

- **Random Forest (RF):** A traditional machine-learning classifier for network-flow classification.
- **Graph Convolutional Network (GCN):** A graph-based deep-learning model that learns from similarities between network flows.

Explainability techniques are incorporated to understand model predictions:

- **SHAP:** Explains Random Forest feature contributions.
- **GNNExplainer:** Explains selected GCN predictions through important features and graph connections.

## 2. Project Objectives

- Analyse and preprocess CICIDS2017 network traffic data.
- Identify benign and malicious network flows.
- Train a Random Forest classifier.
- Construct a feature-similarity graph and train a GCN.
- Evaluate both models using accuracy, precision, recall and F1-score.
- Apply SHAP to interpret Random Forest predictions.
- Apply GNNExplainer to interpret selected GCN predictions.
- Compare model performance and explanation results.

## 3. Dataset

**Dataset:** CICIDS2017  
**Source:** Canadian Institute for Cybersecurity, University of New Brunswick.

Dataset link: https://www.unb.ca/cic/datasets/ids-2017.html

The project uses the MachineLearningCSV collection containing pre-extracted network-flow features.

### Dataset statistics

| Property | Value |
|---|---:|
| Original records | 2,830,743 |
| Original columns | 79 |
| Records after cleaning | 2,522,362 |
| Numeric input features | 78 |
| Benign records after cleaning | 2,096,484 |
| Attack records after cleaning | 425,878 |
| Experimental sample | 10,000 |

A balanced sample was created for the model experiments:

- 5,000 benign flows
- 5,000 attack flows

All non-BENIGN labels were grouped into a single Attack class for binary classification.

## 4. Methodology

The project follows these stages:

1. Load and combine CICIDS2017 CSV files.
2. Inspect the dataset and perform exploratory data analysis.
3. Remove duplicate records.
4. Replace infinite values and handle missing numeric values using median imputation.
5. Create binary labels: Benign (0) and Attack (1).
6. Generate a balanced sample of 10,000 records.
7. Split the sample into training, validation and testing sets (70:15:15).
8. Train and evaluate the Random Forest model.
9. Standardise GCN features using training data only.
10. Construct separate K-nearest-neighbour similarity graphs for each data partition.
11. Train and evaluate the corrected GCN model.
12. Apply SHAP and GNNExplainer.
13. Compare performance and explanation results.

### Graph construction

For the GCN, each network flow is represented as a node. Connections are created between similar flows using a K-nearest-neighbour algorithm (k = 5).

**Important:** This is a feature-similarity graph, not a real network topology graph. Its edges represent similarity between flow records, not communication between actual computers or IP addresses.

## 5. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Google Colab | Development environment |
| Pandas and NumPy | Data processing |
| Matplotlib and Seaborn | Data visualisation |
| Scikit-learn | Preprocessing and Random Forest |
| PyTorch | Deep learning |
| PyTorch Geometric | Graph Neural Networks |
| SHAP | Random Forest explainability |
| GNNExplainer | GCN explainability |
| NetworkX | Graph representation and visualisation |

## 6. Model Performance

Both models were evaluated on the same balanced test set containing 1,500 records.

| Metric | Random Forest | Corrected GCN |
|---|---:|---:|
| Accuracy | 99.67% | 96.13% |
| Precision | 100.00% | 95.53% |
| Recall | 99.33% | 96.80% |
| F1-score | 99.67% | 96.16% |

### Key observation

Random Forest achieved higher overall performance than the corrected GCN in this experiment. The GCN also achieved an F1-score above 96%.

These results apply to the selected balanced sample and test partition; they do not guarantee the same performance on real-world network traffic.

## 7. Explainable AI Results

### SHAP – Random Forest

SHAP was used to estimate global feature importance across the Random Forest test set.

Some of the most influential features included:

- Bwd Packet Length Std
- Packet Length Std
- Packet Length Variance
- Max Packet Length
- Bwd Packet Length Mean
- Average Packet Size
- Bwd Packet Length Max

### GNNExplainer – GCN

GNNExplainer was applied to a correctly classified attack node (node 8502).

Some of its most influential features were:

- Idle Mean
- FIN Flag Count
- Bwd Packet Length Max
- Fwd IAT Std
- Flow IAT Std
- Packet Length Mean
- Init_Win_bytes_forward

It also identified important neighbouring nodes in the feature-similarity graph.

**Note:** The reported GNNExplainer feature ranking is a local explanation for one selected node. It should not be interpreted as global GCN feature importance. SHAP and GNNExplainer also operate at different explanation scopes.

## 8. How to Run the Project

The project is implemented in Google Colab.

### Step 1: Open the notebook

Upload `Explainable_AI_Cyber_Threat_Detection.ipynb` to Google Colab or open it directly from your Google Drive.

### Step 2: Download the dataset

Download the CICIDS2017 MachineLearningCSV dataset from:

https://www.unb.ca/cic/datasets/ids-2017.html

Extract the CSV files and upload them to Google Drive.

### Step 3: Set up the dataset directory

The notebook uses this Google Drive directory:

```text
/content/drive/MyDrive/CICIDS2017/
```

The combined dataset is stored as:

```text
combined_CICIDS2017.csv
```

### Step 4: Execute the notebook

Run the notebook cells in order. The workflow covers:

- Dataset loading and preprocessing
- Exploratory data analysis
- Random Forest training and evaluation
- Feature-similarity graph construction
- GCN training and evaluation
- SHAP explanations
- GNNExplainer visualisations
- Model comparison

Some graph-learning libraries may need to be installed in the Colab runtime before execution.

## 9. Project Outputs

Important generated files include:

```text
CICIDS2017/
│
├── combined_CICIDS2017.csv
├── gnn_sample.csv
├── random_forest_model.pkl
├── random_forest_comparison.pkl
├── gcn_model_corrected.pth
│
└── GNN_Explanations/
    ├── corrected_gnn_explanation.pt
    ├── corrected_feature_importance.csv
    ├── corrected_edge_importance.csv
    ├── random_forest_shap_importance.csv
    ├── shap_gnn_feature_comparison.csv
    ├── final_model_comparison.csv
    └── model_performance_comparison.png
```

## 10. Project Scope and Limitations

This is an **offline research prototype** based on pre-extracted CICIDS2017 network-flow features.

The current implementation does not include:

- Live packet capture
- Real-time network monitoring
- Automatic attack blocking
- Live intrusion prevention
- Actual IP communication graph construction

Other limitations include binary classification of attack categories, evaluation on a balanced sample, and a local GNN explanation for one selected attack flow.

## 11. Future Scope

Potential improvements include:

- Multi-class classification of individual attack families.
- Generating GNN explanations for multiple benign and attack flows.
- Evaluating the models on additional intrusion detection datasets.
- Testing model robustness under naturally imbalanced traffic distributions.
- Developing an interactive dashboard for offline threat analysis.
- Exploring real-time network-flow extraction and deployment as a separate future phase.

## 12. Research Question

**Can Explainable AI identify cyber threats while helping analysts understand why an attack was detected?**

---

**Project:** Explainable AI for Cyber Threat Detection  
**Domain:** Artificial Intelligence, Machine Learning and Cybersecurity  
**Institution:** Dayananda Sagar University  
**Department:** Computer Science and Engineering (Artificial Intelligence and Machine Learning)  
**Academic Year:** 2026–2027
