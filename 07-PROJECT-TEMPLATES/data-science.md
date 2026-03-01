# Template: Data Science / ML Project

Copy this to your project's `CLAUDE.md` and customize.

```markdown
# [Project Name] — Claude Code Agent

## PROJECT OVERVIEW
**What:** [ML/data project description]
**Goal:** [Prediction target, accuracy goal, business metric]
**Data:** [Data sources, size, update frequency]
**Stage:** [Exploration | Modeling | Production | Monitoring]

## TECH STACK
- **Language:** Python 3.12
- **Data:** pandas, polars, numpy
- **Visualization:** plotly, matplotlib, seaborn
- **ML:** scikit-learn, xgboost, lightgbm
- **Deep Learning:** PyTorch (if applicable)
- **Time Series:** prophet, statsmodels (if applicable)
- **Experiment Tracking:** MLflow
- **Notebooks:** Jupyter Lab
- **Environment:** pyproject.toml + uv

## ARCHITECTURE
```
project/
├── data/
│   ├── raw/             # Original, immutable data
│   ├── processed/       # Cleaned, transformed data
│   ├── features/        # Feature-engineered datasets
│   └── external/        # Third-party data
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_features.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_evaluation.ipynb
├── src/
│   ├── data/            # Data loading and processing
│   ├── features/        # Feature engineering
│   ├── models/          # Model definitions
│   ├── evaluation/      # Metrics and evaluation
│   └── utils/           # Shared utilities
├── models/              # Saved model artifacts
├── outputs/             # Plots, reports, predictions
├── tests/
└── configs/             # Experiment configurations
```

## CONVENTIONS
- Data pipeline: raw → processed → features → model
- Never modify files in data/raw/
- Save all artifacts with timestamps
- Log all experiments to MLflow
- Type hints on all function signatures
- Docstrings (numpy style) on all public functions
- Reproducibility: set random seeds, log all parameters

## WORKFLOW
1. **EDA first** — understand the data before modeling
2. **Baseline model** — simple model before complex ones
3. **Feature engineering** — domain-driven features
4. **Experiment tracking** — log everything to MLflow
5. **Cross-validation** — never evaluate on training data
6. **Error analysis** — understand WHERE the model fails

## CONSTRAINTS
- Never train on test data
- Always use cross-validation for model selection
- Document all assumptions about the data
- Save intermediate results for reproducibility
- Don't optimize prematurely — get the pipeline working first
```
