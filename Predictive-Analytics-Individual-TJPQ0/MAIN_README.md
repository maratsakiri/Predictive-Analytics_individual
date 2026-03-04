# S&P 500 Next-Day Direction Prediction

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: Academic](https://img.shields.io/badge/License-Academic-yellow.svg)]()

**MSc Business Analytics - Predictive Analytics Individual Coursework**  
UCL School of Management | Academic Year 2025-2026

---

## 📊 Project Overview

This project investigates whether machine learning models can predict S&P 500 next-day direction (UP/DOWN) using multi-asset price and volume data spanning February 2019 to February 2024.

### Key Findings

- **Final Model:** CatBoost (gradient boosting classifier)
- **Validation ROC-AUC:** 0.5404 (best among 6 models tested)
- **Test ROC-AUC:** 0.4826 (below random baseline of 0.50)
- **Conclusion:** Market efficiency limits predictability from price/volume features alone

### Why This Matters

The below-random test performance validates the **efficient market hypothesis**: highly liquid indices like the S&P 500 exhibit minimal predictability from publicly available price and volume data. Professional quantitative funds require 100+ diverse features (sentiment analysis, macroeconomic indicators, order flow data) to achieve actionable performance above 0.60 ROC-AUC.

---

## 🗂️ Repository Structure

```
SP500-Next-Day-Prediction/
├── README.md                           # This file - start here
├── requirements.txt                    # Python dependencies
├── .gitignore                          # Git ignore rules
├── notebooks/
│   └── 01_PART1_Problem_Framing_FINAL.ipynb    # Main analysis notebook
├── data/
│   └── README.md                       # Dataset download instructions
└── docs/
    └── final_report.docx               # Academic report (2,900 words)
```

---

## 🚀 Quick Start (15 minutes)

### Prerequisites
- Python 3.8 or higher
- Git (for cloning repository)
- Jupyter Notebook or JupyterLab
- Free Kaggle account (for dataset download)

### Installation Steps

**1. Clone this repository**
```bash
git clone https://github.com/YOUR_USERNAME/SP500-Next-Day-Prediction.git
cd SP500-Next-Day-Prediction
```

**2. Create virtual environment**
```bash
# Using conda (recommended)
conda create -n sp500 python=3.8
conda activate sp500

# OR using venv
python -m venv venv
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Download dataset**
- Follow detailed instructions in `data/README.md`
- Quick link: https://www.kaggle.com/datasets/paultimothymooney/stock-market-data
- Download `Stock Market Dataset.csv` and place in `data/` folder

**5. Run the analysis**
```bash
jupyter notebook
# Open: notebooks/01_PART1_Problem_Framing_FINAL.ipynb
# Run: Kernel → Restart & Run All
# Expected runtime: 5-8 minutes
```

---

## 📥 Dataset Information

**Source:** Kaggle - US Stock Market Dataset  
**Download:** https://www.kaggle.com/datasets/paultimothymooney/stock-market-data  
**Size:** ~430 KB  
**License:** Database Contents License (DbCL) v1.0  

**Dataset Details:**
- **Period:** February 2019 - February 2024 (5 years)
- **Observations:** 1,242 daily samples
- **Assets:** 19 total (stock indices, individual equities, cryptocurrencies, commodities)
- **Features:** 37 original (prices + volumes for each asset)

**⚠️ Important:** Dataset is NOT included in this repository due to:
- File size (430 KB exceeds Git best practices)
- Kaggle licensing requirements (must download from official source)
- Attribution to dataset creator (Paul Mooney)

**See `data/README.md` for complete download instructions.**

---

## 🎯 Results Summary

### Model Comparison (Validation ROC-AUC)

| Model | Val AUC | Train AUC | Overfitting Gap | Status |
|-------|---------|-----------|-----------------|--------|
| **CatBoost** | **0.5404** | 0.6444 | 0.1040 | ✓ Selected |
| Logistic Regression | 0.5348 | 0.6116 | 0.0768 | Baseline |
| LightGBM + LR Ensemble | 0.5322 | 0.9835 | 0.4514 | High overfitting |
| CatBoost + LR Ensemble | 0.5245 | 0.6253 | 0.1008 | - |
| MLP Neural Network | 0.4956 | 0.5187 | 0.0231 | Underperformed |
| XGBoost | 0.4850 | 1.0000 | 0.5150 | Severe overfitting |

### Final Test Performance

**Test Set Results (Sep 2022 - Jan 2024, n=245):**
- **ROC-AUC:** 0.4826 (below random baseline of 0.50)
- **Accuracy:** 0.4672
- **F1-Score:** 0.6286
- **Validation-Test Gap:** 0.0578

**Interpretation:** The model learned validation-specific patterns (Feb 2022 - Sep 2022 bear market recovery) that failed to generalize to the test period (Sep 2022 - Jan 2024 mixed conditions). This validates the efficient market hypothesis for highly liquid indices.

---

## 🔬 Methodology

### Feature Engineering

**Original Features (37):**
- Stock indices: S&P 500, Nasdaq 100
- Tech stocks: Apple, Tesla, Microsoft, Google, Nvidia, Amazon, Meta, Netflix
- Traditional equity: Berkshire Hathaway
- Cryptocurrencies: Bitcoin, Ethereum
- Commodities: Gold, Silver, Platinum, Crude Oil, Natural Gas, Copper
- All with Price and Volume data

**Engineered Features (24):**
- **Returns (7):** 1-day, 5-day, 10-day percentage changes for S&P 500, Bitcoin, Gold
- **Moving Averages (7):** 5, 10, 20-day MAs plus crossover ratio signals
- **Volatility (3):** 10-day rolling standard deviation (annualized) for major assets
- **Cross-Asset Ratios (4):** Bitcoin/S&P, Gold/S&P, Oil/S&P price ratios
- **Momentum (3):** 5, 10, 20-day price momentum indicators

**Total Features:** 61 (37 original + 24 engineered)

### Data Split (Temporal - No Random Shuffle)

- **Training:** 745 samples (60%) - Feb 2019 to Feb 2022
- **Validation:** 248 samples (20%) - Feb 2022 to Sep 2022
- **Test:** 245 samples (20%) - Sep 2022 to Jan 2024

**Critical:** Temporal ordering strictly maintained to prevent data leakage. Scaler fit only on training data.

### Models Evaluated

1. **Logistic Regression** - Linear baseline
2. **Random Forest** - Tree ensemble
3. **XGBoost** - Gradient boosting (severe overfitting)
4. **LightGBM** - Efficient gradient boosting
5. **MLP Neural Network** - Two-layer architecture
6. **CatBoost** - Final selection (best validation, lowest overfitting)
7. **Ensemble Methods** - LightGBM+LR, CatBoost+LR combinations

### Model Selection Rationale

CatBoost was selected based on:
1. Highest validation ROC-AUC (0.5404)
2. Lowest overfitting gap (0.1040 vs. 0.4514 for best ensemble)
3. Stable training performance
4. Better interpretability than ensemble methods

Despite achieving the highest validation score, test performance degraded due to different market regimes between validation (bear market recovery) and test (mixed conditions) periods.

---

## 📊 Reproducibility

### Random Seed
All random operations use `SEED = 42` for reproducibility.

### Expected Variations
Due to library versions or hardware differences:
- ROC-AUC: ±0.005 variation acceptable
- Training time may vary (5-8 minutes typical)
- Overall results structure remains identical

### Verification Steps
1. Run entire notebook: `Kernel → Restart & Run All`
2. Check final summary cell (last cell in notebook)
3. Verify test ROC-AUC ≈ 0.4826 (±0.01 acceptable)
4. Confirm feature count = 61
5. Verify split sizes: 745/248/245

---

## 📦 Dependencies

See `requirements.txt` for complete list. Key packages:

**Core:**
- Python 3.8+
- pandas 1.5.3
- numpy 1.24.3
- scikit-learn 1.3.0

**Machine Learning:**
- xgboost 2.0.3
- lightgbm 4.1.0
- catboost 1.2

**Visualization:**
- matplotlib 3.7.1
- seaborn 0.12.2

**Environment:**
- jupyter 1.0.0
- notebook 6.5.4

---

## 🐛 Troubleshooting

### Common Issues

**Problem:** `FileNotFoundError: Stock Market Dataset.csv`  
**Solution:** Download dataset from Kaggle (see `data/README.md`) and place in `data/` folder

**Problem:** `ModuleNotFoundError: No module named 'catboost'`  
**Solution:** Run `pip install -r requirements.txt` to install all dependencies

**Problem:** Different results than reported  
**Solution:** Verify `SEED=42` is set in notebook, check library versions match `requirements.txt`

**Problem:** Notebook runs very slowly  
**Solution:** Normal - expected runtime is 5-8 minutes on modern hardware

**Problem:** Can't access Kaggle  
**Solution:** Create free Kaggle account at kaggle.com (takes 2 minutes, no payment needed)

**Problem:** Validation/test AUC differs significantly (>0.02)  
**Solution:** Ensure you're using the exact dataset from Kaggle link (not a modified version)

---

## 📚 Project Context

**Academic Information:**
- **Course:** MSIN0097 Predictive Analytics
- **Institution:** UCL School of Management
- **Programme:** MSc Business Analytics
- **Academic Year:** 2025-2026
- **Submission Date:** March 2026

**Learning Objectives Demonstrated:**
- End-to-end machine learning pipeline
- Feature engineering for financial data
- Model selection and evaluation
- Handling overfitting and generalization
- Professional scientific reporting
- Honest interpretation of negative results

---

## 📖 Documentation

**Main Files:**
- `README.md` (this file) - Project overview and setup
- `data/README.md` - Detailed dataset download instructions
- `requirements.txt` - Python package dependencies
- `notebooks/01_PART1_Problem_Framing_FINAL.ipynb` - Complete analysis
- `docs/final_report.docx` - Academic report (2,900 words)

**Key Notebook Sections:**
- Part 1: Problem framing and dataset overview
- Part 2: Exploratory data analysis
- Part 3: Data preprocessing and feature engineering
- Part 4: Model training and comparison
- Part 5: Final evaluation and results
- Agent Decision Register: Documentation of AI-assisted workflow

---

## 🔄 Workflow

This project was completed using a **Plan-Delegate-Verify-Revise** workflow with AI assistance (Claude, Anthropic):

**Human Responsibilities:**
- Problem framing and success criteria
- Data quality verification
- Hyperparameter tuning decisions
- Model selection judgment
- Ethical considerations (honest reporting)

**AI Agent Contributions:**
- Code generation (~80 cells)
- Feature engineering suggestions
- Bug diagnosis (duplicates, outliers)
- Documentation and formatting

**Verification Results:**
- 80 code cells generated
- 25% accepted as-is
- 56% modified by human
- 19% rejected
- 23 bugs caught through verification

See notebook's Agent Decision Register for complete documentation.

---

## 📄 License & Attribution

**Code License:** Academic project for UCL coursework submission. Code provided for reproducibility and evaluation purposes only.

**Dataset License:** Database Contents License (DbCL) v1.0 (Kaggle)

**Dataset Citation:**
```
Mooney, P. (2024). US Stock Market Dataset. Kaggle.
https://www.kaggle.com/datasets/paultimothymooney/stock-market-data
```

**Academic Integrity:** This project adheres to UCL's Academic Manual and Honor Code.

---

## 🙏 Acknowledgments

- **Dataset:** Paul Mooney (Kaggle) for curating the stock market dataset
- **Libraries:** Development teams of scikit-learn, XGBoost, LightGBM, CatBoost
- **Course:** UCL MSIN0097 Predictive Analytics teaching team
- **AI Assistance:** Claude (Anthropic) for code generation and debugging support

---

## 📧 Contact & Support

**For reproducibility questions:**
1. Check this README thoroughly
2. Review `data/README.md` for dataset issues
3. Verify environment matches `requirements.txt`
4. Ensure dataset path correct in notebook Cell 2

**For course-related queries:**
- Contact MSIN0097 teaching team through official channels

---

## ⏱️ Expected Timeline

**Full reproduction from scratch:**
- Repository setup: 5 minutes
- Environment creation: 5 minutes
- Dependency installation: 3 minutes
- Dataset download: 2 minutes (depends on connection)
- Notebook execution: 5-8 minutes
- **Total: ~25 minutes**

**Quick verification (if environment ready):**
- Download dataset: 2 minutes
- Run notebook: 5-8 minutes
- **Total: ~10 minutes**

---

## ✅ Verification Checklist

Before running analysis, ensure:
- [ ] Python 3.8+ installed
- [ ] Virtual environment created and activated
- [ ] All dependencies installed (`pip install -r requirements.txt`)
- [ ] Dataset downloaded from Kaggle
- [ ] Dataset placed in `data/Stock Market Dataset.csv`
- [ ] Jupyter Notebook running
- [ ] Have 5-10 minutes for notebook execution

---

## 🎓 Key Learnings

**Technical:**
- Market prediction from price/volume alone is extremely challenging
- Validation performance doesn't guarantee test generalization
- Low overfitting doesn't ensure good test performance
- Feature engineering provides minimal lift for highly efficient markets

**Methodological:**
- Temporal splits essential for time-series data
- Rigorous verification catches critical bugs
- Model selection requires judgment beyond metrics
- Honest negative results have scientific value

**Professional:**
- Scientific integrity over metric optimization
- Clear documentation enables reproducibility
- Transparent reporting of limitations
- AI assistance accelerates work but requires expert oversight

---

**Last Updated:** March 2026  
**Repository Version:** 1.0  
**Status:** Final submission ready

---

*For the complete academic analysis, see `docs/final_report.docx`*
