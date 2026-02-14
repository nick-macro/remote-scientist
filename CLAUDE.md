# CLAUDE.md — Quantitative Research Project

## Project Overview

This repository is a **quantitative research** codebase. Work here involves data analysis, statistical modeling, backtesting, and numerical computation. Prioritize correctness, reproducibility, and clarity over premature optimization.

## Tech Stack

- **Language:** Python 3.10+
- **Core libraries:** numpy, pandas, scipy, statsmodels, scikit-learn
- **Visualization:** matplotlib, seaborn, plotly
- **Notebooks:** Jupyter (for exploratory analysis)
- **Data formats:** CSV, Parquet, HDF5, SQL

## Project Structure

```
├── CLAUDE.md            # This file — project guidelines for Claude
├── README.md            # Project documentation
├── pyproject.toml       # Project metadata and dependencies
├── data/                # Raw and processed datasets (not committed to git)
│   ├── raw/             # Original, immutable data
│   └── processed/       # Cleaned/transformed data
├── notebooks/           # Jupyter notebooks for exploration and reporting
├── src/                 # Source code
│   ├── data/            # Data loading, cleaning, and transformation
│   ├── features/        # Feature engineering
│   ├── models/          # Model definitions and training
│   ├── evaluation/      # Metrics, backtesting, validation
│   └── utils/           # Shared utilities
├── tests/               # Unit and integration tests
├── configs/             # Experiment configurations (YAML/JSON)
└── outputs/             # Results, figures, logs (not committed to git)
```

## Commands

- **Install dependencies:** `pip install -e ".[dev]"`
- **Run all tests:** `pytest tests/`
- **Run a single test file:** `pytest tests/test_<module>.py`
- **Run a single test:** `pytest tests/test_<module>.py::test_function_name`
- **Lint:** `ruff check src/ tests/`
- **Format:** `ruff format src/ tests/`
- **Type check:** `mypy src/`

## Code Style and Conventions

- Follow **PEP 8**. Use `ruff` for linting and formatting.
- Use **type hints** on all function signatures.
- Use **docstrings** (NumPy-style) on public functions and classes. Include parameter descriptions, return types, and a brief explanation of the method or algorithm.
- Prefer **descriptive variable names** — `daily_returns` not `dr`, `rolling_volatility` not `rv`.
- Use `pathlib.Path` for file paths, never string concatenation.
- Use `logging` instead of `print` for runtime output.

## Data Handling

- Raw data is **immutable** — never modify files in `data/raw/`.
- All data transformations must be **reproducible**: scripted in `src/data/`, not done manually.
- Use Parquet for intermediate datasets (fast I/O, preserves types).
- Always validate data shapes and types at function boundaries with assertions or explicit checks.
- Document data sources, schemas, and any known quirks in docstrings or a `data/README.md`.

## Numerical & Statistical Code

- Use **vectorized operations** (numpy/pandas) over Python loops wherever possible.
- Be explicit about **NaN handling** — document whether functions propagate, drop, or fill NaNs.
- When computing statistics, always specify and document:
  - The time period / sample window
  - Whether results are annualized and the annualization factor
  - The null hypothesis for any statistical test
- Use **random seeds** for reproducibility: pass `random_state` or `np.random.default_rng(seed)`.
- Avoid **look-ahead bias**: never use future data to compute current features or signals. Clearly separate in-sample and out-of-sample data.

## Testing

- Write tests for all data transformations, feature engineering, and model evaluation code.
- Use **pytest** with fixtures for common test data.
- Test edge cases: empty DataFrames, single-row inputs, all-NaN columns, date boundary conditions.
- For numerical code, use `np.testing.assert_allclose` with explicit `atol`/`rtol` rather than exact equality.
- Keep tests fast — mock expensive I/O and use small synthetic datasets.

## Notebooks

- Notebooks are for **exploration and presentation**, not production logic.
- Extract reusable logic into `src/` modules and import from notebooks.
- Clear all outputs before committing notebooks to keep diffs clean.
- Name notebooks with a numbered prefix: `01_data_exploration.ipynb`, `02_feature_analysis.ipynb`.

## Git Practices

- Do **not** commit data files, model artifacts, or large outputs. Use `.gitignore`.
- Commit messages should reference what analysis or experiment changed and why.
- One logical change per commit.

## Common Pitfalls to Avoid

- **Survivorship bias:** Ensure datasets include delisted/removed entities if relevant.
- **Overfitting:** Always hold out test data. Report both in-sample and out-of-sample metrics.
- **P-hacking:** Pre-register hypotheses when possible. Report all tests run, not just significant ones.
- **Unit mismatch:** Be consistent with units (returns as decimals vs. percentages, daily vs. annual).
- **Timezone confusion:** Always be explicit about timezones. Store datetimes as UTC; convert for display only.
