<div align="center">

<img src="https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white"/>
<img src="https://img.shields.io/badge/Tableau-Dashboard-E97627?style=for-the-badge&logo=tableau&logoColor=white"/>
<img src="https://img.shields.io/badge/SciPy-Statistics-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white"/>
<img src="https://img.shields.io/badge/Power%20BI-Reporting-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>

# 🎮 GameZone Digital — Product Analytics & Feature Performance

### Turning 1,000,000+ raw user events into prioritised product decisions for 80,000 players across 5 regions.

[Problem Statement](#-the-business-problem) · [Pipeline](#-analytical-pipeline) · [Tableau Dashboards](#-tableau-dashboards) · [Key Findings](#-key-findings-real-data) · [Recommendations](#-recommendations)

</div>

---

## 🧭 The Business Problem

GameZone Digital launched **8 new platform features** in Q1 2024. Three months in, the Product team had zero visibility into which features were working, which weren't, and — critically — *why*. This project answers all three questions end-to-end:

```
Who uses what?  →  Where do they drop off?  →  Why?  →  What do we test next?
```

The final deliverable is a **Tableau dashboard pack** + **Python analysis notebook** that the PM team can open, filter, and act on the same day.

---

## 📐 Analytical Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│  DATA LAYER  (6 tables · 1M+ events · 80K players)                  │
│  users_master · feature_adoption · events_log                       │
│  feature_funnel · daily_feature_usage · dq_check_log                │
└──────────────────────┬──────────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────────────┐
│  QUALITY GATE  (Cell 3) — 7 automated SQL-style nightly checks       │
│  NULL user_ids · Duplicate event_ids · Future timestamps             │
│  Orphan events · Negative durations · Unknown features · Bad segs   │
└──────────────────────┬──────────────────────────────────────────────┘
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
┌──────────────────┐      ┌──────────────────────┐
│  Python Notebook │      │  Tableau Workbook     │
│                  │      │  (.twbx)              │
│  · Adoption EDA  │      │                       │
│  · Seg Heatmap   │      │  Dashboard 1:         │
│  · Funnel Anlys  │      │  Feature Adoption     │
│  · DAU Trend     │      │  Overview             │
│  · Session Depth │      │                       │
│  · Mann-Whitney  │      │  Dashboard 2:         │
│  · DQ Monitoring │      │  Funnel Analysis      │
│  · Scorecard     │      │                       │
│  · Root Cause    │      │  6 Worksheets total   │
└──────────┬───────┘      └──────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  COMPOSITE FEATURE SCORECARD (Cell 11)            │
│  Adoption 50% · Funnel Completion 30% · Session 20%│
└──────────┬───────────────────────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  ROOT CAUSE + A/B HYPOTHESES · EXEC BRIEFING     │
│  6 CSV exports → Tableau / Power BI ready        │
└──────────────────────────────────────────────────┘
```

---

## 📊 Tableau Dashboards

The Tableau workbook (`GameZone_ProductAnalytics.twbx`) contains **2 dashboards** and **6 worksheets**, all powered by 3 embedded data extracts.

### Dashboard 1 — Feature Adoption Overview

| Sheet | Chart Type | What It Shows |
|---|---|---|
| `1 Adoption Rate` | Horizontal bar | Adoption % per feature, sorted DESC |
| `2 Adopted vs NonAdopted` | Stacked bar | Adopted vs non-adopter count per feature |
| `5 Daily Active Users` | Line chart | DAU trend Jan–Jun 2024 per feature |
| `6 Avg Session Minutes` | Horizontal bar | Avg session duration per feature |

### Dashboard 2 — Funnel Analysis

| Sheet | Chart Type | What It Shows |
|---|---|---|
| `3 Funnel Completion` | Stacked bar | Users at each funnel stage per feature |
| `4 Funnel Dropoff Pct` | Bar chart | Step-level drop-off % per feature |

### Data Extracts Embedded in .twbx

| Extract | Rows | Fields |
|---|---|---|
| `adoption.csv` | 8 features | `feature`, `adopted_users`, `adoption_pct` |
| `funnel.csv` | 40 rows (8 features × 5 steps) | `feature`, `funnel_step`, `users` |
| `daily.csv` | 180 days × 8 features | `date`, `feature`, `dau`, `sessions`, `avg_session_min` |

> Custom **GameZone colour palette** applied across all sheets: `#2980B9 · #27AE60 · #E74C3C · #9B59B6 · #E67E22 · #1ABC9C`

---

## 📈 Key Findings — Real Data

### Adoption Rates (from `adoption.csv`)

```
daily_spin_wheel    ████████████████████████████████  66.4%  🟢 Healthy
loyalty_pass        ████████████████████████████      60.6%  🟢 Healthy
leaderboard_v2      █████████████████████████         54.4%  🟢 Healthy
live_tournament     ████████████████████              45.0%  🟢 Healthy
custom_avatar       ████████████                      28.9%  🟢 Healthy
─────────── 20% threshold ──────────────────────────────────────────
achievement_badges  ████                              10.4%  🔴 Underutilised
friend_challenge    ███                                8.4%  🔴 Underutilised
in_game_chat        ██                                 6.6%  🔴 Underutilised
```

### Funnel Completion & Drop-off (from `funnel.csv`)

| Feature | Impression → Complete | Click → Open Drop-off | Verdict |
|---|---|---|---|
| `loyalty_pass` | **26.8%** | -4.9% (users increase) | 🟢 Best performer |
| `live_tournament` | 25.6% | -25.2% (users increase) | 🟢 Strong |
| `achievement_badges` | 25.7% | **52.6% drop** | ⚠️ High impression, poor discovery |
| `daily_spin_wheel` | 24.3% | 47.6% drop | 🟡 Acceptable |
| `in_game_chat` | 18.0% | 17.0% drop | 🟡 Mid-funnel issue |
| `leaderboard_v2` | 10.1% | 25.5% drop | 🔴 Low completion |
| `friend_challenge` | **8.7%** | **60.4% drop** | 🔴 Worst discovery rate |
| `custom_avatar` | 7.3% | 29.0% drop | 🔴 Low completion |

> Key insight: `friend_challenge` has a **60.4% Click→Open drop-off** — the highest of all 8 features — confirming this is a **discoverability problem, not a value problem**.

### Daily Active Users (from `daily.csv`)

| Feature | Avg DAU | Peak DAU | Avg Session (min) |
|---|---|---|---|
| `daily_spin_wheel` | **8,594** | 11,334 | 10.2 |
| `loyalty_pass` | 7,786 | 10,142 | 10.0 |
| `leaderboard_v2` | 6,954 | 9,234 | 9.6 |
| `live_tournament` | 5,754 | 7,342 | 10.0 |
| `custom_avatar` | 3,727 | 4,694 | 9.4 |
| `achievement_badges` | 1,323 | 1,735 | 9.6 |
| `friend_challenge` | 1,068 | 1,419 | 9.6 |
| `in_game_chat` | **839** | 1,076 | **10.6** |

> `in_game_chat` has the *lowest* DAU (839) but the *highest* avg session duration (10.6 min) — users who do find it are engaged. This is a **pure acquisition/discoverability problem**.

---

## ✅ Recommendations

| Priority | Feature | Root Cause | Action | A/B Hypothesis |
|---|---|---|---|---|
| 🔴 HIGH | `friend_challenge` | 60.4% click→open drop-off — buried 3 taps deep | Move to home screen | Home placement → +15% adoption |
| 🔴 HIGH | `in_game_chat` | 839 avg DAU despite 10.6 min sessions — dead state for new players | Global chat room for <10 friends users | Global tab → +20% adoption |
| 🔴 HIGH | `achievement_badges` | 52.6% click→open drop-off — 0 push notifications sent | Enable Day-3 push notification | D3 push → +10% D7 adoption |

---

## 🗂️ Full Dataset

| Table | Rows | Description |
|---|---|---|
| `users_master.csv` | 80,000 | Players — segment, region, device |
| `feature_adoption.csv` | 640K | Per-player feature usage & session count |
| `events_log.csv` | 1,000,000+ | Raw platform events with timestamps |
| `feature_funnel.csv` | 40 | 8 features × 5 funnel steps |
| `daily_feature_usage.csv` | ~1,440 | DAU per feature × 180 days |
| `dq_check_log.csv` | 6 months | Nightly DQ check history |

> Datasets are synthetic, generated to mirror the production Redshift schema. No real user data included.

---

## 📤 Outputs

```
outputs/
├── GameZone_ProductAnalytics.twbx        # Tableau workbook — 2 dashboards, 6 sheets
├── output_adoption_summary.csv           # Feature-level adoption rates
├── output_feature_scorecard.csv          # Composite performance scores
├── output_funnel_analysis.csv            # Step-by-step funnel KPIs
├── output_segment_adoption.csv           # Segment × Feature matrix
├── output_region_adoption.csv            # Region × Feature matrix
└── output_dq_monthly.csv                 # 6-month DQ check history
```

---

## 🛠️ Tech Stack

| | Tool | Purpose |
|---|---|---|
| 🐍 | Python 3 · pandas · numpy | Data pipeline & analysis (14 notebook cells) |
| 📊 | matplotlib · seaborn | Python visualisations |
| 🔬 | scipy.stats | Mann-Whitney U · Chi-Square hypothesis testing |
| 📉 | **Tableau Desktop** | 2 interactive dashboards · 6 worksheets · `.twbx` workbook |
| 🗄️ | SQL (Redshift-style via pandas) | 7-check nightly DQ automation |
| 📈 | Power BI | Stakeholder reporting layer |

---

## 🚀 Run It Locally

```bash
# Clone
git clone https://github.com/your-username/gamezone-product-analytics.git
cd gamezone-product-analytics

# Python analysis
pip install pandas numpy matplotlib seaborn scipy jupyter
jupyter notebook GameZone_Product_Analytics.ipynb

# Tableau dashboard
# Open GameZone_ProductAnalytics.twbx in Tableau Desktop (v10.5+)
# All data extracts are embedded — no external connection needed
```

---

## 📬 Contact

**Isfaque Ansari** · Data Analyst  
[isfaqueans6@gmail.com](isfaqueans6@gmail.com) · [LinkedIn](https://linkedin.com/in/isfaque-ansari-/) 

---

<div align="center">
  <sub>Built with Python & Tableau · GameZone Digital Analytics Portfolio · Q1/Q2 2024</sub>
</div>
