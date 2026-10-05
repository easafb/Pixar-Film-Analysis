# 🎬 Pixar Film Analysis

An R Markdown report on the relationship between **critical acclaim**, **budget** and **commercial success** for Pixar Animation Studios films, using data from the [`pixarfilms`](https://github.com/erictleung/pixarfilms) R package.

**Authors:** Emir Asaf Baş, Şeyda Nur Zencirci, Tayyar Berke Tavlan, Natoli Hunduma Legesse

---

## 📌 Research Questions

1. **Can we predict a Pixar film's worldwide box office revenue from its budget and critic scores?** We answer this with linear regression.
2. **Can we predict whether a Pixar film will be commercially successful (profitable) from its budget, critic scores and runtime?** We answer this with logistic regression.

## 🗂️ Data

The report combines three tables from the `pixarfilms` package, joined on the `film` column with `left_join()`:

| Table | Contents |
|---|---|
| `pixar_films` | Film number, title, release date, runtime, MPAA rating (27 rows) |
| `public_response` | Rotten Tomatoes, Metacritic, CinemaScore, Critics' Choice scores |
| `box_office` | Budget and box office (US/Canada, other, worldwide) |

The package has no scores or financial figures for its 4 most recent entries (*Luca*, *Turning Red*, *Lightyear* and an untitled 2023 film). After removing them, the final dataset has **23 films (1995–2020)** and 13 variables.

## 📊 Methodology

**1. Data cleaning**
- Converted `number` from text to integer and replaced the `"N/A"` text in `film_rating` with a real `NA`.
- Checked for duplicate films (none found) and removed the films that have no data.
- Kept the few remaining `NA`s (`critics_choice` for the two earliest films, and `cinema_score` / `box_office_us_canada` for *Soul*, which went straight to Disney+).

**2. Exploratory data analysis**
- Created 10 new variables: `release_year`, `decade`, `profit`, `roi`, `avg_critic_score`, `commercial_success`, `era` (Early Pixar / Golden Age / Modern Pixar), `runtime_category`, `is_sequel` and `budget_category`.
- Grouped summaries by era and by sequel status, plus 10 `ggplot2` charts.

**3. Modeling** (`tidymodels`)
- 75% / 25% train-test split (`initial_split`, `set.seed(314)`), so 17 training films and 6 test films.
- **Linear regression:** `box_office_worldwide ~ budget + rotten_tomatoes + metacritic`. Evaluated with R², adjusted R², F-test, RMSE and MAE.
- **Logistic regression:** `commercial_success ~ budget + rotten_tomatoes + metacritic + run_time`, with a split stratified on `commercial_success`. Evaluated with coefficient z-tests, deviance, AIC and test accuracy.

## 🔍 Key Findings

| | Result |
|---|---|
| **Eras** | Early Pixar (1995–2005) had the highest critic scores (RT 97.3, Metacritic 86.5) and ROI (~611%). Average profit is almost the same in every era (~$470–490M). |
| **Sequels** | The 8 sequels/prequels earn more profit on average (~$641M vs ~$393M for originals), but their ROI is nearly identical (~354%) and their reviews are lower. |
| **Budget** | Budget and worldwide box office are only weakly correlated (r ≈ 0.34). Low-budget films had the highest ROI. |
| **Linear model** | On the training set, `budget` (p = 0.0025) and `metacritic` (p = 0.026) are significant and R² = 0.58. On the test set, R² falls to 0.065 (RMSE ≈ $356M, MAE ≈ $260M). |
| **Logistic model** | No predictor is significant. Test accuracy is 83.3% only because the model predicts "Yes" for every film. |

## ⚠️ Limitations

- **Very small sample:** with only 23 films, both models overfit and generalize poorly.
- **Class imbalance:** only 2 of the 23 films lost money (*Onward* and *Soul*), so the training set for the logistic model contains a single unsuccessful film.
- **External shocks:** *Onward* and *Soul* were both hit by the COVID-19 pandemic (cinema closures and a direct-to-streaming release). They are extreme outliers that ordinary box office patterns cannot explain.
- **Missing drivers:** marketing spend, release timing, competition and franchise strength are not in the data. Budget and review scores alone are not enough for a robust model.
- **Association, not causation:** regression coefficients describe associations in this sample, not causal effects.

## 🛠️ Tech Stack

- **Language:** R (4.5) with RStudio / R Markdown
- **Packages:** `pixarfilms` (data), `tidyverse` (wrangling and `ggplot2` charts), `tidymodels` (`rsample`, `parsnip`, `yardstick`)
- **Environment:** `renv`

## 🚀 How to Run

```
Pixar Film Analysis Report/
├── PixarFilmAnalysis.Rmd                  # Source report
├── PixarFilmAnalysis.html                 # Rendered report
└── Pixar Film Analysis Report.Rproj       # RStudio project
```

1. Open `Pixar Film Analysis Report/Pixar Film Analysis Report.Rproj` in RStudio.
2. Install the required packages:
   ```r
   install.packages(c("pixarfilms", "tidyverse", "tidymodels", "rmarkdown"))
   ```
3. Open `PixarFilmAnalysis.Rmd` and click **Knit** to render the HTML report.
