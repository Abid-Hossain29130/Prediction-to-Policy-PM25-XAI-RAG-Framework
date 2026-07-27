# Prediction-to-Policy-PM25-XAI-RAG-Framework

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)

Official repository accompanying the research manuscript:

> **"From Prediction to Policy: An Explainable Machine Learning and Retrieval-Augmented Generation Framework for Urban Air Quality Decision Support"**

This repository contains the complete source code, datasets, execution environment, evaluation notebooks, and supporting materials required to reproduce the experiments and results presented in the manuscript.

---

## 📑 Contents

- [Framework Overview and Performance](#-framework-overview-and-performance)
- [Repository Structure](#-repository-structure)
- [Quick Start and Reproducibility](#-quick-start-and-reproducibility)
- [Citation](#-citation)
- [License](#-license)

---

## 📌 Framework Overview and Performance

Urban air quality management in data-constrained megacities faces a trilemma between predictive accuracy, physical interpretability, and actionable policy translation. This framework addresses this challenge through a unified, end-to-end decision-support pipeline.

### 1. Predictive Engine (XGBoost)

Daily **T+24-hour PM₂.₅ forecasting** using strict chronological walk-forward validation to eliminate temporal data leakage.

#### Performance

- **Mean R²:** 0.744
- **Mean Absolute Error (MAE):** 10.03 µg/m³
- **Skill Score:** +3.1% over a highly competitive persistence baseline

---

### 2. Explainability and Risk Mapping

- Global feature importance using **SHAP**
- Local explanations using **LIME**
- Inverse Distance Weighting (**IDW**) interpolation
- Geospatial health-risk mapping across **92 administrative Thanas** in Dhaka

---

### 3. Policy RAG Agent

A Retrieval-Augmented Generation (**RAG**) framework backed by **ChromaDB** and grounded exclusively in:

- Bangladesh Air Pollution Control Rules (**APCR 2022**)
- National Air Quality Management Plan (**NAQMP 2024–2030**)
- WHO Global Air Quality Guidelines (**2021**)

#### Performance

- **Regulatory Hallucination Rate:** 0.0%
- **Context Precision:** 1.000
- **Semantic Faithfulness:** 0.922

---

### ⚡ Computational Efficiency

The complete end-to-end inference pipeline executes in **under 4 seconds** on a standard consumer-grade CPU, demonstrating suitability for real-time municipal deployment.

---

## 📁 Repository Structure

```text
Prediction-to-Policy-PM25-XAI-RAG-Framework
│
├── data/
│   ├── raw/
│   │   └── Raw telemetry datasets
│   │
│   ├── processed/
│   │   └── ML-ready datasets
│   │
│   └── policy_corpus/
│       ├── Markdown policy documents
│       └── Original PDF sources
│
├── src/
│   ├── data_engineering/
│   │
│   ├── dhaka_aqi_forecasting_engine.py
│   ├── dhaka_spatial_risk_mapping.py
│   └── grap_environmental_advisory_rag.py
│
├── notebooks/
│   └── Pre-executed Jupyter notebooks
│
├── docs/
│   ├── RAGAS evaluation diagnostics
│   └── Environmental Action Advisory Briefs (EAAB)
│
├── supplementary_visuals/
│   ├── Partial Dependence Plots (PDP)
│   └── SHAP Feature Interaction Plots
│
├── vector_db/
│
├── requirements.txt
├── .env.example
├── LICENSE
└── README.md
```

---

## ⚙️ Quick Start and Reproducibility

To ensure strict open-science reproducibility, all stochastic operations are globally fixed:

```python
RANDOM_SEED = 42
```

Researchers and reviewers can evaluate the framework in two ways.

### Option A — View Results Directly (No Installation Required)

Navigate to the `notebooks/` directory.

All notebooks have already been executed and include:

- SHAP visualizations
- LIME explanations
- Spatial risk maps
- Model evaluation results
- Generated policy recommendations

This allows reviewers to inspect the complete workflow without installing dependencies or executing code.

---

### Option B — Reproduce the Framework Locally

#### 1. Clone the Repository

```bash
git clone https://github.com/your-username/Prediction-to-Policy-PM25-XAI-RAG-Framework.git
cd Prediction-to-Policy-PM25-XAI-RAG-Framework
```

#### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it:

**Windows**

```bash
venv\Scripts\activate
```

**Linux / macOS**

```bash
source venv/bin/activate
```

#### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

#### 4. Configure Environment Variables

Rename:

```text
.env.example
```

to:

```text
.env
```

and insert your own:

```text
GROQ_API_KEY=your_api_key_here
```

> **Security Note:** API keys are intentionally excluded from version control and remain strictly local.

#### 5. Execute the Framework

Run the backend modules inside the `src/` directory or execute the interactive notebooks within the `notebooks/` directory.

---

## 📝 Citation

If you use this repository in academic research, please cite the accompanying manuscript:

```bibtex
@article{PredictionToPolicy2026,
  title={From Prediction to Policy: An Explainable Machine Learning and Retrieval-Augmented Generation Framework for Urban Air Quality Decision Support},
  author={Hossain, Abid},
  journal={...},
  year={2026}
}
```

> The citation will be updated once the manuscript has been formally published.

---

## 📜 License

This project is distributed under the terms of the **MIT License**.

See the [LICENSE](LICENSE) file for details.

---

## 🌍 Research Impact

This framework bridges the gap between environmental forecasting and policy implementation by integrating:

- Machine learning prediction
- Explainable AI (XAI)
- Geospatial risk analytics
- Retrieval-Augmented Generation (RAG)

into a unified decision-support architecture designed for deployment in data-constrained urban environments.

The project demonstrates how transparent AI systems can transform air-quality forecasts into evidence-based environmental policy recommendations while maintaining reproducibility, interpretability, and regulatory grounding.