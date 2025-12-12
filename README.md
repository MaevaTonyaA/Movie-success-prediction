# Movie-succes-prediction
ML Project that predict the success of a new movie

## Dataset
The main dataset used is located at https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset

---

## The Team
* **Timothée ANDRIAMALALA** 
* **Maeva ANDRIAMANIRAKA** 
* **Kenza HASSANI** 

## 📄 Project Overview
[cite_start]The film industry is a high-risk sector where producing a movie often feels like a gamble [cite: 8-9]. [cite_start]The goal of this project is to transition from a "casino" approach to a data-driven decision-making process[cite: 12].

We developed an AI tool capable of:
1.  [cite_start]**Predicting Quality (Regression):** Estimating the specific rating a film will receive (e.g., 7.4/10)[cite: 17].
2.  [cite_start]**Predicting Profitability (Classification):** Classifying a project as a **Hit** or **Flop** to provide a "Green/Red Light" to investors [cite: 20-21].

## Data Pipeline & Engineering

### 1. Cleaning Strategy
To reduce noise and improve model reliability, we applied a strict cleaning process:
* [cite_start]**Noise Removal:** Dropped columns with complex text (titles, tags) and high variance names (actors/directors) to focus on quantitative signals [cite: 60-64].
* **Credibility Filter:** We only kept movies with `vote_count >= 30`. [cite_start]This removed statistical anomalies and "ghost" films, significantly improving data quality [cite: 297-299].
* [cite_start]**Imputation:** Missing values were filled with averages to maximize the training set[cite: 67].

### 2. Feature Engineering
We created custom features to help the model understand context:
* **`genre_potential`:** We used **Target Encoding** to replace text genres with their historical average rating. [cite_start]For example, Documentaries (avg 6.66) score higher than Horror movies naturally [cite: 232-238].
    * [cite_start]*Result:* This feature alone achieved a correlation of **0.306** with the final rating[cite: 242].

## Key Insights (EDA)
Before modeling, we analyzed historical patterns:
* **Seasonality:** Release timing is strategic. [cite_start]We observed clear popularity peaks (Summer/End of Year) and troughs (January/September) [cite: 124-125].
* **Budget vs. Quality:** Contrary to popular belief, a higher budget does not correlate with a higher critical rating. [cite_start]The trend line is flat [cite: 170-173].
* [cite_start]**Engagement:** The volume of votes (`vote_count`) is the strongest proxy for success[cite: 328].

## Models & Performance

We tested two main approaches.

### 1. Random Forest (Regression)
Used to predict the exact vote average.
* **Optimization:** Tuned using `RandomizedSearchCV`. [cite_start]Best parameter: `min_samples_leaf: 5` to prevent overfitting [cite: 302-303].
* **Results:**
    * [cite_start]**R² Score:** 0.473 (Explains ~47% of variance)[cite: 309].
    * [cite_start]**RMSE:** 0.675 (Average error is < 0.7 points on a scale of 10)[cite: 313].

### 2. XGBoost (Classification)
Used to predict "Hit" vs "Flop". This is the primary business tool.
* [cite_start]**Optimization:** Tuned via `GridSearch` (`max_depth: 3`, `learning_rate: 0.05`, `n_estimators: 1000`) [cite: 379-380].
* [cite_start]**Class Imbalance:** Applied a `scale_pos_weight` of 1.07 to force the model to value rare hits as much as average films[cite: 371].
* **Results:**
    * [cite_start]**F1-Score:** 0.8716[cite: 389].
    * **"Bodyguard" Capability:** The model has a **Recall of 81%** on Class 0 (Flops). [cite_start]It successfully identifies and blocks 4 out of 5 bad projects[cite: 461].

## Feature Importance
[cite_start]The top factors driving our predictions are [cite: 319-323]:
1.  **Genre Potential** (Intrinsic quality of the genre)
2.  **Runtime** (Duration)
3.  **Release Year** (Context)
4.  **Vote Count** (Social Proof)

## Simulation
The project includes a logic (`predict_my_movie`) to simulate scenarios:
* [cite_start]*Input:* "Action movie released in July" $\rightarrow$ **Potential Success**[cite: 357].
* [cite_start]*Input:* "Horror movie released in January" $\rightarrow$ **High Risk (Flop)**[cite: 358].

---
*Based on the Machine Learning Project Report: "Prédiction du Succès d'un Film" (12/12/25).*
