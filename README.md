# Pgp-Inhibitor-Prediction
P-gp inhibitor/substrate prediction model using LLM and GNN

# P-gp Interaction Profiling and SAR Analysis using LLMs and Explainable GNNs

## 📖 Abstract

Multidrug Resistance (MDR) remains a significant challenge in cancer chemotherapy, with the efflux pump P-glycoprotein (P-gp) playing a central role. This project introduces a novel, end-to-end computational pipeline to accelerate the discovery of P-gp modulators. We leverage Large Language Models (LLMs) to automatically curate a high-quality interaction dataset from biomedical literature. A dual-model system based on Graph Isomorphism Networks (GINE) was developed to profile compounds simultaneously as inhibitors and substrates, providing a nuanced pharmacological perspective. Rigorous external validation against the ChEMBL database revealed a significant "Domain Shift" between literature-based knowledge and assay-based data, a key finding that was confirmed with a control experiment (AUC 0.38 vs. 0.96). Finally, by employing Explainable AI (XAI) through GNNExplainer, our pipeline successfully reproduced established Structure-Activity Relationship (SAR) rules, demonstrating its validity as a powerful tool for hypothesis generation in drug discovery.

## 🚀 Key Features

* **LLM-Powered Data Curation:** A custom pipeline that uses the ChatGPT API to automatically extract and label P-gp-compound interactions from the latest PubMed abstracts, creating a dynamic and up-to-date dataset.
* **Dual-Model System:** Two independent GINE models were developed to profile compounds from two pharmacological perspectives: **inhibition** and **substrateness**.
* **P-gp Interaction Profile Map:** A novel 2D visualization method to intuitively classify compounds into four meaningful groups: *Specific Inhibitors*, *Specific Substrates*, *Dual-Role*, and *Non-Interactors*.
* **Domain Shift Investigation:** Experimentally demonstrated and quantified the "Domain Shift" between literature-based knowledge and assay-based (ChEMBL) data, clarifying the model's applicability domain.
* **XAI-driven SAR Hypothesis:** Used GNNExplainer to interpret the model's predictions and successfully reproduced known SAR rules for P-gp inhibitors and substrates from scratch.

## 🔬 Methodology & Pipeline

The research was conducted following a systematic pipeline:

1.  **Data Curation & Labeling:**
    * PubMed abstracts related to P-gp were retrieved.
    * A prompted ChatGPT API batch job was used to perform structured extraction of compound-interaction pairs.
    * Compound names were standardized against PubChem and ChEMBL to obtain SMILES and InChIKeys, followed by a confidence-based labeling strategy.

2.  **Model Development:**
    * The problem was framed as two separate binary classification tasks (Inhibitor vs. Non-Inhibitor; Substrate vs. Non-Substrate).
    * Graph Isomorphism Network (GINE) was selected as the model architecture.
    * Hyperparameters were optimized for each model using Optuna with 5-fold cross-validation.

3.  **Analysis & Validation:**
    * Out-of-Fold (OOF) predictions were used to generate a reliable `final_results` dataframe for the Profile Map.
    * External validation was performed using a filtered dataset from the ChEMBL database.
    * A control experiment (training and testing exclusively on ChEMBL data) was conducted to diagnose the cause of the initial validation failure.
    * GNNExplainer was applied to the literature-trained models to analyze the top 15 most confident predictions for inhibitors and substrates.

## 📈 Key Results & Findings

### 1. P-gp Interaction Profile Map

Our dual-model system allowed for the creation of a pharmacological map, plotting the predicted probability of inhibition against the probability of being a substrate. This provides an at-a-glance profile for any given compound.

*[Insert your P-gp Interaction Profile Map image here]*

### 2. Experimental Proof of Domain Shift

Our literature-trained model performed poorly on an external ChEMBL dataset (AUC ≈ 0.38), even after filtering for a similar chemical space. However, a control model trained and tested exclusively on ChEMBL data achieved excellent performance (AUC ≈ 0.96).

*[Insert your Chemical Space Distribution plot image here]*

**This key finding proves that the performance gap was not due to a flawed model architecture, but due to a fundamental difference in the data domains**—what is described as an "inhibitor" in scientific literature is not always equivalent to what is measured as a potent "inhibitor" in a quantitative assay.

### 3. XAI-based SAR Hypothesis

By analyzing the GNNExplainer results, our pipeline successfully rediscovered the core SAR principles for P-gp interactions without prior knowledge.

* **Key Features of Inhibitors:**
    1.  **High Hydrophobicity & Rigidity:** The presence of planar or rigid multi-cyclic systems is critical.
    2.  **Multiple Aromatic Anchors:** The most common pattern involves two or more aromatic rings that can act as hydrophobic anchors within the binding pocket.
    3.  **Bulky Structures:** Large macrocycles (e.g., Cyclosporine A) can act as physical blockers.

* **Key Features of Substrates:**
    1.  **Cationic Properties:** The presence of a basic nitrogen atom, which is positively charged at physiological pH, is a dominant feature.
    2.  **Structural Flexibility:** Compared to rigid inhibitors, substrates often possess greater conformational flexibility, which may be required to transit through the efflux pump.

## 🛠️ Installation & Usage

To run this project, first clone the repository and then install the required packages. A `requirements.txt` file is provided for easy setup.

```bash
# Clone the repository
git clone [https://github.com/](https://github.com/)[Your-Username]/Pgp-Inhibitor-Prediction.git

# Navigate to the project directory
cd Pgp-Inhibitor-Prediction

# Install required packages
pip install -r requirements.txt
