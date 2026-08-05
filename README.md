# Prediction-to-Policy-PM25-XAI-RAG-Framework

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)

Official repository accompanying the research manuscript:

> **"From Prediction to Policy: An Explainable Machine Learning and Retrieval-Augmented Generation Framework for Urban Air Quality Decision Support"**

This repository contains the complete source code, datasets, execution environment, evaluation notebooks, and supporting materials required to reproduce the experiments and results presented in the manuscript.

---

## 📑 Contents

- [Framework Overview and Performance](#framework-overview-and-performance)
- [Repository Structure](#repository-structure)
- [Pipeline Architecture & API Reproducibility Guide](#pipeline-architecture--api-reproducibility-guide)
- [Quick Start and Reproducibility](#quick-start-and-reproducibility)
- [Citation](#citation)
- [License](#license)
- [Research Impact](#research-impact)

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
- Inverse Distance Weighting (**IDW**) interpolation
- Geospatial health-risk mapping and Attributable Fraction (AF) calculations across **92 administrative Thanas** in Dhaka

---

### 3. Policy RAG Agent

A Retrieval-Augmented Generation (**RAG**) framework backed by **ChromaDB** and grounded exclusively in:

- Bangladesh Air Pollution Control Rules (**APCR 2022**)
- National Air Quality Management Plan (**NAQMP 2024–2030**)
- WHO Global Air Quality Guidelines (**2021**)

#### Performance

- **Regulatory Hallucination Rate:** 0.0%
- **Context Precision:** 1.000
- **Mean Semantic Faithfulness:** 0.567 (representing a 92.2% threshold compliance pass-rate at ≥0.42 similarity)

---

### ⚡ Computational Efficiency

The complete end-to-end inference pipeline executes in **under 4 seconds** on a standard consumer-grade CPU, demonstrating suitability for real-time municipal deployment.

---

## 📁 Repository Structure

```text
Prediction-to-Policy-PM25-XAI-RAG-Framework
│
├── data/
│   └── policy_corpus/                  # Codified legal corpus (APCR 2022, NAQMP, WHO)
│
├── docs/
│   ├── EAAB_eval_diagnostics.json      # Raw multi-scenario RAGAS-proxy evaluation logs (Winter)
│   ├── EAAB_Extreme_eval_diagnostics.json # Diagnostic evaluation logs (Extreme Crisis)
│   ├── EAAB_Monsoon_eval_diagnostics.json # Diagnostic evaluation logs (Monsoon Washout)
│   ├── EAAB_Extreme_Scenario.md        # Generated Action Brief (Extreme Crisis Stage)
│   ├── EAAB_Monsoon_Scenario.md        # Generated Action Brief (Monsoon Alert Stage)
│   └── EAAB_Winter_Inversion_Scenario.md # Generated Action Brief (Winter Inversion Stage)
│
├── notebooks/
│   └── supplementary_dm_regime_analysis.ipynb # Regime-partitioned Diebold-Mariano analysis
│
├── src/
│   └── data_engineering/
│       ├── dhaka_spatial_risk_mapping.py      # Spatial IDW & Attributable Fraction risk mapping
│       ├── supplementary_dm_regime_analysis.py # Regime-partitioned statistical testing script
│       ├── Week3_EDA.py                       # Exploratory Data Analysis script
│       └── Week4_Feature_Engineering.py       # Master dataset fusion & lag engineering
│
├── vector_db/
│   └── chroma/                         # ChromaDB vector store
│
├── requirements.txt                    # Complete Python dependencies
├── .env.example                        # API environment configuration template
├── LICENSE                             # MIT Open Source License
└── README.md
```

---

## 🤖 Pipeline Architecture & API Reproducibility Guide

### Two-Stage LLM Framework

The RAG policy module operates via a two-stage LLM workflow:

1. **Primary Generator (GPT-OSS):** Synthesises spatial risk maps and XGBoost forecasts into legally grounded Environmental Action/Advisory Briefs (EAABs).
2. **Evaluator / LLM-as-a-Judge (Llama):** Conducts automated, multi-criteria expert scoring and diagnostic quality auditing of the generated briefs.

---

### ⚠️ External API Execution & Endpoint Configuration

* **Pre-Computed Diagnostic Logs & Markdown Briefs:** All generated EAAB advisories (`docs/*.md`), RAGAS-proxy diagnostic logs (`docs/*_eval_diagnostics.json`), and evaluation matrices are pre-rendered and saved inside the `docs/` and `notebooks/` directories. Reviewers can inspect all outputs directly on GitHub without re-executing code.
* **API Key Setup:** To execute the pipeline locally, rename `.env.example` to `.env` and supply your `GROQ_API_KEY` (or OpenAI credentials).
* **Handling Third-Party Model String Updates:**
  Third-party API providers periodically update, rename, or retire specific model string endpoints. If an API call returns a `400 Bad Request` or `404 Model Not Found` error during local execution:
  1. Check your provider's current active model registry (e.g., Groq Console or OpenAI Docs).
  2. Update the environment variables (`GROQ_GENERATOR_MODEL` and `GROQ_JUDGE_MODEL`) to point to the current active equivalents (e.g., `llama-3.3-70b-versatile`).

---

## ⚙️ Quick Start and Reproducibility

To ensure strict open-science reproducibility, all stochastic operations are globally fixed:

```python
RANDOM_SEED = 42
```

Researchers and reviewers can evaluate the framework in two ways:

### Option A — View Results Directly (No Installation Required)

Navigate to the `docs/` and `notebooks/` directories.

All evaluation logs and notebooks have already been executed and include:
- Diagnostic RAGAS logs (`EAAB_eval_diagnostics.json`)
- Generated scenario briefs (`EAAB_Extreme_Scenario.md`, `EAAB_Monsoon_Scenario.md`, `EAAB_Winter_Inversion_Scenario.md`)
- Regime-partitioned Diebold-Mariano statistical results (`notebooks/supplementary_dm_regime_analysis.ipynb`)

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

Rename `.env.example` to `.env` and insert your API credentials:

```text
GROQ_API_KEY=your_api_key_here
GROQ_GENERATOR_MODEL=openai/gpt-oss-120b
GROQ_JUDGE_MODEL=llama-3.3-70b-versatile
```

> **Note on Model Configuration:** By default, the framework uses the anonymized model names from the paper (`openai/gpt-oss-120b` for generating advisories and `llama-3.3-70b-versatile` for judging). To run the pipeline successfully on standard Groq API models, you can override these defaults by setting the `GROQ_GENERATOR_MODEL` (e.g., `llama3-70b-8192`) and `GROQ_JUDGE_MODEL` variables in your environment.

> **Security Note:** API keys are intentionally excluded from version control and remain strictly local.

#### 5. Execute the Framework

Run the backend modules inside the `src/data_engineering/` directory or execute the interactive notebooks within the `notebooks/` directory.

---

## 📝 Citation

If you use this repository in academic research, please cite the accompanying manuscript:

```bibtex
@article{PredictionToPolicy2026,
  title={From Prediction to Policy: An Explainable Machine Learning and Retrieval-Augmented Generation Framework for Urban Air Quality Decision Support},
  author={Hossain, Abid and Rezaul, Karim Mohammed},
  journal={Environmental Modelling \& Software},
  year={2026}
}
```

> The citation will be updated once the manuscript has been formally published.

---

## 📜 License

This project is distributed under the terms of the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

## 🌍 Research Impact

This framework bridges the gap between environmental forecasting and policy implementation by integrating:

- Machine learning prediction
- Explainable AI (XAI)
- Geospatial risk analytics
- Retrieval-Augmented Generation (RAG)

into a unified decision-support architecture designed for deployment in data-constrained urban environments.

The project demonstrates how transparent AI systems can transform air-quality forecasts into evidence-based environmental policy recommendations while maintaining reproducibility, interpretability, and regulatory grounding.
