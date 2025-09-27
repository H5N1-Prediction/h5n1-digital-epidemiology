# H5N1 Digital Epidemiology: Early-Warning Signals from Online Discourse

A reproducible toolkit to ingest, analyze, and model social/search/news signals around H5N1, evaluate pre/post tone shifts around CDC announcement dates of the outbreak events, and explore whether outbreaks can be anticipated from discourse dynamics.

Data Science Capstone - Term: Fall 2025, Harvard HSE.\
Noura Almansoori, Spencer Beaupre, Byeongsu Kim, Gazi Saief Mahmud, Renee Van Siclen



---

## Overview

This repository operationalizes the project proposal “Predicting H5N1 Outbreaks Using Social, Search, and Environmental Data” by implementing a practical, modular pipeline for:

* ingesting and harmonizing multi-source digital signals,
* computing sentiment, emotion, and tone features,
* running before/after analyses keyed to 2024-04-04 (CDC announcement),
* testing early-warning potential via lagged/temporal modeling,
* producing transparent, governance-ready outputs (provenance, lineage, and reproducibility).

The approach is grounded in digital epidemiology, information disorder, and the Social Amplification of Risk Framework (SARF), with hypotheses on awareness signals, emotional spikes, and regional sensitivity. 

---

## Research questions and hypotheses (from proposal)

**Core questions**

* Do online signals (social posts, search volume, news) rise before official outbreak communications?
* How do fear, uncertainty, urgency, and safety/trust tones evolve pre/post official announcements?
* How do patterns differ by region and platform?

**Hypotheses**

1. Awareness Signal: measurable increases in digital activity precede official announcements by several days.
2. Emotional Spike: uncertainty/fear peaks pre-confirmation and declines post-communication.
3. Regional Sensitivity: regions with less prior exposure show earlier, stronger fear/speculation spikes. 

---

## Data sources (extensible)

* Social platforms (e.g., X/Twitter, Reddit) with timestamps, basic engagement, and optional geotags
* Search interest (Google Trends)
* News signals (GDELT)
* Official anchors (CDC/WHO dates, USDA APHIS) for alignment
* Optional environmental/regional covariates (for sensitivity analyses) 

> Ethics, privacy, and platform ToS are respected; content is de-identified and aggregated where appropriate. 

---

## Repository structure

```
h5n1-digital-epidemiology/
├─ data/
│  ├─ raw/               # do not commit PII or proprietary data
│  ├─ interim/
│  └─ processed/
├─ notebooks/
│  ├─ 01_eda_sentiment.ipynb
│  ├─ 02_prepost_analysis.ipynb
│  └─ 03_time_lag_modeling.ipynb
├─ src/
│  ├─ ingest/            # API clients, loaders, SOMAR harmonization
│  ├─ features/          # sentiment, tone, topic, metadata features
│  ├─ modeling/          # CV, lag tests, classifiers, changepoints
│  ├─ eval/              # metrics, reports
│  └─ utils/             # config, logging, lineage helpers
├─ reports/
│  ├─ figures/
│  └─ artifacts/
├─ configs/
│  └─ params.yaml        # dates, lags, feature toggles
├─ requirements.txt
├─ README.md
└─ LICENSE
```

---

## Quickstart

1. Python environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

2. Place your CSV(s)

* Put social/search/news CSVs into `data/raw/` (or set paths in `configs/params.yaml`).

3. Run the baseline notebook

* Open `notebooks/01_eda_sentiment.ipynb` (or the provided `H5N1_EDA_Modeling.ipynb`) to:

  * infer columns (time, text, user),
  * compute sentiment (VADER fallback included),
  * add tone features: fear, uncertainty, urgency, safety/trust,
  * build daily aggregates and charts,
  * mark `pre` vs `post` using 2024-04-04.

4. Pre/Post analysis

* Use `notebooks/02_prepost_analysis.ipynb` to:

  * compare means with Mann–Whitney tests,
  * report effect sizes,
  * visualize changes in volume and tone.

5. Early-warning exploration

* Use `notebooks/03_time_lag_modeling.ipynb` to:

  * create lagged daily features (e.g., 3–14-day windows),
  * run lag correlations and Granger tests,
  * fit time-aware classifiers/regressors.

---

## Configuration

Edit `configs/params.yaml` to set:

* `announcement_date`: `2024-04-04`
* `time_col`, `text_col`, `user_col`, `lang_col`
* tone/sentiment lexicons or model choices
* lag windows, CV strategy, evaluation dates
* platform keys (if you enable live ingestion)

---

## Features

* Sentiment: VADER or transformer-based classifiers
* Tone counts: fear, uncertainty, urgency, safety/trust
* Structure: hashtags, mentions, URLs, is_retweet
* Topics: optional BERTopic (UMAP + HDBSCAN) for narratives
* Governance: SOMAR-style provenance, dataset descriptors, and lightweight lineage tracking to keep outputs reproducible and auditable. 

---

## Modeling & evaluation

* Time-aware splits (no leakage), with safeguards when a fold has a single class
* Metrics: ROC-AUC, F1, accuracy for classifiers; coherence for topics; lag correlation and Granger for early-warning tests
* Changepoint detection (optional) for structural breaks around key dates
* Reporting: tabular summaries and exportable figures

Evaluation emphasizes transparency and reproducibility; results are exploratory (not clinical surveillance). 

---

## Ethics and responsible use

* Public content only; de-identification and aggregation where possible
* Avoid stigmatizing users/regions; communicate uncertainty clearly
* Document bias and coverage limits; English-only analyses are noted when used
* Keep provenance/lineage with every transformation and model artifact 

---

## Roadmap

* Add regional sensitivity modeling and geovisualizations
* Incorporate domain-tuned language models for sentiment/emotion
* Extend to multi-platform ingestion and multilingual pipelines
* Build a lightweight “early-warning score” dashboard with lagged features and confidence bands

---

## How to cite

If you use this repository or derive work from it, please cite the accompanying project proposal that informed this implementation. 

---

## License

MIT
