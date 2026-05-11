# Individual capstone — used vehicle listing price

## Your project details (portfolio)

**Author:** Anthony Howell  
**Course:** Unit 3 Individual Capstone (deployed ML app)  
**Problem:** Craigslist-style used vehicle listings are noisy, but buyers and sellers still want a **sanity-check price** from a few structured fields (not a wall of text).  
**What I built:** A **Streamlit** web app that takes **seven inputs** (model year, odometer, manufacturer, fuel, transmission, drive, body type), runs two trained models, and returns:

1. **Regression:** estimated **listing price in dollars** (RandomForest pipeline from notebook 02).  
2. **Classification:** **budget / mid / premium** tier (HistGradientBoosting from notebook 03), where tiers are **tertiles** of cleaned price in the training sample (same cutoffs stored in the regression bundle metadata).

**Data:** Full **`vehicles.csv`** (Craigslist vehicles dump) cleaned in notebook 01 (price/year/odometer filters, top manufacturers, dropped URL/NLP-heavy columns). Processed table: `data/processed/cleaned_data.csv`.  
**Repo layout:** EDA + modeling in `notebooks/`, serialized models in `models/`, UI in `app/app.py`.

**Live app (fill in after you deploy):**  
`https://YOUR-APP-NAME.streamlit.app` ← replace with your Streamlit Cloud URL for the final submission box.

---

## Repository layout

```text
your-capstone-project/   (rename folder from NEW if you like)
├── README.md
├── requirements.txt       ← used by Streamlit Cloud (minimal)
├── requirements-dev.txt   ← optional: notebooks locally
├── data/
│   ├── raw/               ← optional README + .gitkeep; large CSV usually not committed
│   └── processed/         ← cleaned_data.csv from notebook 01
├── models/
│   ├── regression_model.pkl
│   └── classification_model.pkl
├── notebooks/
│   ├── 01_problem_statement_and_eda.ipynb
│   ├── 02_regression_model.ipynb
│   └── 03_classification_model.ipynb
└── app/
    └── app.py
```

---

## Local setup

```bash
cd NEW   # or your renamed project folder
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt          # enough to run the app
pip install -r requirements-dev.txt      # adds Jupyter etc. for notebooks
```

### Run the app locally

```bash
streamlit run app/app.py
```

Confirm **both tabs** work: dollar prediction and tier prediction.

---

## Reproduce data & models (notebooks)

1. Point notebook **01** at your **`vehicles.csv`** (`RAW_PATH` cell), or copy the file to `data/raw/` and update that path.  
2. Run **`01` → `02` → `03`** in order.  
3. Notebooks save models with **`joblib.dump(..., compress=3)`** so `regression_model.pkl` stays **under GitHub’s 100 MB per-file limit** (raw uncompressed forests can exceed that).

Outputs you should see:

- `data/processed/cleaned_data.csv`  
- `models/regression_model.pkl`  
- `models/classification_model.pkl`

---

## Push to your personal GitHub (required)

I **cannot** log into your GitHub account from here. You run these commands **once** on your Mac (install [GitHub CLI](https://cli.github.com/) with `brew install gh` if you want the easy path).

### Option A — GitHub website + Git (no CLI)

1. On GitHub: **New repository** → name it e.g. `used-vehicle-capstone` → **empty** (no README).  
2. On your machine:

```bash
cd /Users/jahbrae/Downloads/NEW
git init
git branch -M main
git add .
git commit -m "Capstone: notebooks, models, Streamlit app"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Use **HTTPS** or **SSH** remote URL depending on how you authenticate (GitHub now prefers a **personal access token** over a password for HTTPS).

### Option B — GitHub CLI (`gh`)

```bash
cd /Users/jahbrae/Downloads/NEW
git init
git add .
git commit -m "Capstone: notebooks, models, Streamlit app"
gh auth login          # browser flow — you complete this
gh repo create YOUR_REPO --private --source=. --remote=origin --push
```

If `git push` asks for credentials, complete **GitHub’s login / token** flow in the browser or paste a **fine-scoped PAT** when prompted.

---

## Deploy on Streamlit Community Cloud

1. Go to [share.streamlit.io](https://share.streamlit.io) and sign in with GitHub.  
2. **New app** → pick **your repo** and **branch** (`main`).  
3. **Main file path:** `app/app.py`  
4. **Python version:** leave default unless the UI lets you pick; this project targets **scikit-learn ≥ 1.4** (for `root_mean_squared_error` in notebooks) and **pandas 2.x** (Streamlit compatibility).  
5. Click **Deploy**. When it finishes, copy the URL (looks like `https://YOUR-APP-NAME.streamlit.app`) into your assignment field and into the **Live app** line at the top of this README.

If the deploy log shows missing `models/*.pkl`, you forgot to `git add` / `git push` those files (they are required in the repo for Cloud).

---

## Before you submit (checklist)

- [ ] `streamlit run app/app.py` runs locally with **no errors**  
- [ ] **Regression** tab returns a **dollar** estimate  
- [ ] **Classification** tab returns **budget / mid / premium**  
- [ ] Repo pushed to **your** GitHub (not a class org unless instructed)  
- [ ] Streamlit Cloud app is **live**; you paste **`https://....streamlit.app`** in the submission box  
- [ ] README **Your project details** section reflects you (edit name/links if needed)  

---

## Notes

- **Features in the app:** 7 fields — within the assignment’s “4–8 features” expectation (year is converted to **vehicle_age** using the same `age_base` stored in the bundle).  
- **Rubric snippet:** If your Canvas rubric still mentions **TF-IDF / NLP complaints**, that is a **different project template** — follow the instructions your instructor posted for **this** capstone; this repo is the **tabular vehicles** version.
