🎬 About the Project
In this project, we analyzed data from Pixar Animation Studios films and built two primary models:
Linear Regression: Modeling the relationship between worldwide box office revenue (box_office_worldwide) and predictors such as budget and critical scores (Metacritic, Rotten Tomatoes).
Logistic Regression: Classifying whether a film will be a commercial success (commercial_success: Yes/No).


📊 Methodology & Workflow
Data Preprocessing (EDA & Tidyverse):
Calculated net profits and created new era classifications using mutate and case_when.
Converted the categorical target variable to the required format using as.factor().
Data Splitting (Train/Test Split):
Split the data into 75% training and 25% testing sets using the rsample package.
Applied stratified sampling (strata = commercial_success) to ensure a balanced distribution of outcomes in our small dataset.
Used set.seed(314) to guarantee the reproducibility of our random splits.
Model Evaluation Metrics:
Linear Model: R 
2
(R-squared), RMSE, MAE, F-statistic, and p-values.
Logistic Model: Z-value, Residual/Null Deviance, and AIC values.


🔍 Key Findings & Limitations
Small Dataset Constraint: The analysis relies on a limited dataset of only 23 films. This small sample size makes the models highly susceptible to noise, resulting in a noticeable performance drop on unseen test data.
External Shocks (Pandemic Impact): Films like Onward and Soul were released during the COVID-19 pandemic. They acted as extreme outliers that broke traditional box-office rules and negatively impacted the model's predictive accuracy.
Statistical Inference: The lack of statistical significance across predictors highlights that budget and review scores alone are not enough to predict a Pixar film's commercial success. Other external factors (e.g., marketing budget, release timing, sequel status) are required for a robust model.


🛠️ Tech Stack & Libraries
Language: R (RStudio / R Markdown)
Packages:
tidyverse (Data manipulation and visualization)
tidymodels / rsample (Modeling and data splitting)
janitor (Data cleaning)

🚀 How to Run the Project
Open RStudio.
Load the PixarFilmAnalysis.Rmd file.
Ensure all required packages are installed (tidyverse, tidymodels, janitor, etc.).
Click the Knit button to compile and view the R Markdown report.
