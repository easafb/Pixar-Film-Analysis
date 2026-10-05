# 🎬 Pixar Film Analysis

An end-to-end data analysis of Pixar Animation Studios' feature films in R: data cleaning, exploratory analysis, and two statistical models that ask whether **budget and critic scores** can explain a film's **box office performance** and **commercial success**.

**Authors:** Emir Asaf Bas · Seyda Nur Zencirci · Tayyar Berke Tavlan · Natoli Hunduma Legesse

📄 The full report is in [`Pixar Film Analysis Report/PixarFilmAnalysis.html`](Pixar%20Film%20Analysis%20Report/PixarFilmAnalysis.html) (download and open in a browser), with its source in [`PixarFilmAnalysis.Rmd`](Pixar%20Film%20Analysis%20Report/PixarFilmAnalysis.Rmd).

---

## 📦 Data

The data comes from the [`pixarfilms`](https://cran.r-project.org/package=pixarfilms) R package (v0.2.1). Three tables are joined on the film title:

| Table | Contents |
|---|---|
| `pixar_films` | Title, release date, runtime, MPAA rating (27 films) |
| `public_response` | Rotten Tomatoes, Metacritic, CinemaScore, Critics' Choice |
| `box_office` | Budget, US/Canada, international and worldwide gross |

Four films have no score or box office data in the package (*Luca*, *Turning Red*, *Lightyear* and an untitled 2023 entry) and are removed, leaving **23 films (1995–2020)** for analysis.

## 🔬 Methodology

1. **Cleaning** – `left_join` of the three tables, type fixes (`number` → integer, `"N/A"` → `NA`), duplicate check, missing-value audit.
2. **Feature engineering** – 10 new variables, including `profit` (worldwide gross − budget), `roi`, `avg_critic_score`, `commercial_success` (profit > 0), `era` (Early / Golden Age / Modern), `is_sequel`, `runtime_category` and `budget_category`.
3. **Exploratory analysis** – grouped summaries and 10 `ggplot2` charts.
4. **Modeling** (`tidymodels`, 75/25 train/test split, `set.seed(314)`):
   - **Linear regression:** `box_office_worldwide ~ budget + rotten_tomatoes + metacritic`
   - **Logistic regression:** `commercial_success ~ budget + rotten_tomatoes + metacritic + run_time` (split stratified on the outcome)

## 🔍 Key Findings

- **Era:** Early Pixar (1995–2005) had the highest critic scores (RT 97.3, Metacritic 86.5) and ROI (~611%). Average profit per film is nevertheless very stable across eras (~$470–490M).
- **Sequels:** The 8 sequels/prequels earn more on average (~$641M vs ~$393M profit), but their ROI is the same as original films (~354%) because they cost more.
- **Losses:** Only two films lost money by this measure, both pandemic-era 2020 releases: *Onward* and *Soul* (which skipped US cinemas for Disney+).
- **Linear model:** On the training data, `budget` (p = 0.003) and `metacritic` (p = 0.026) are significant and R² = 0.58. On the 6 test films, however, R² falls to 0.065 (RMSE ≈ $356M), largely because of *Soul*.
- **Logistic model:** No predictor is significant. The model predicts "Yes" for every film; its 83.3% test accuracy equals the majority-class baseline, and it misses the only loss-making test film (*Onward*).

## ⚠️ Limitations

- **Very small sample:** 23 films (17 train / 6 test), so test metrics depend heavily on which films land in the test set.
- **Class imbalance:** only 2 of 23 films are unprofitable; the logistic training set contains a single "No".
- **Simplified profit:** worldwide gross minus production budget. Marketing costs and the cinemas' revenue share are ignored, and figures are not adjusted for inflation.
- **External shocks:** COVID-19 disrupted the releases of *Onward* and *Soul*, making them outliers.

## 🛠️ Tech Stack

- **Language:** R, R Markdown
- **Packages:** `pixarfilms` (data), `tidyverse` (wrangling and `ggplot2` charts), `tidymodels` (`rsample`, `parsnip`, `yardstick`)

## 🚀 How to Run

1. Clone the repository and open `Pixar Film Analysis Report/Pixar Film Analysis Report.Rproj` in RStudio.
2. Install the required packages:
   ```r
   install.packages(c("pixarfilms", "tidyverse", "tidymodels", "rmarkdown"))
   ```
3. Open `PixarFilmAnalysis.Rmd` and click **Knit** to build the HTML report.

> **Note:** The project uses `renv`, but the current `renv.lock` only records `renv` itself. After installing the packages above you can run `renv::snapshot()` to lock the exact package versions.
