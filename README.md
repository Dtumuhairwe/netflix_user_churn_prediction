# Netflix User Churn Prediction

Machine learning project to predict which Netflix users are likely to cancel their subscription, using Python and Scikit-learn.

## Overview

This project analyzes 5,000 Netflix customer records to understand what drives churn and build a predictive model. The analysis shows that churn is primarily driven by user engagement behavior rather than pricing or demographics.

## Key Findings

- Basic plan users churn at 61.8% — nearly double Premium users at 43.7%
- - Churned users had not logged in for an average of 38 days vs 22 days for active users
  - - Watch hours and daily engagement are the strongest predictors of churn
    - - Price has very little impact on whether someone cancels
      - - A data leakage check confirmed that behavioral features must be handled carefully in production
       
        - ## Tech Stack
       
        - - **Python** — data analysis and modelling
          - - **Pandas** — data cleaning and exploration
            - - **Matplotlib / Seaborn** — visualizations
              - - **Scikit-learn** — machine learning models
               
                - ## Project Structure
               
                - ```
                  netflix_user_churn_prediction/
                  ├── data/
                  │   └── netflix_customer_churn.csv
                  ├── notebooks/
                  │   ├── 01_data_exploration.ipynb
                  │   ├── 02_eda_visualizations.ipynb
                  │   ├── 03_feature_engineering.ipynb
                  │   └── 04_model_building.ipynb
                  └── requirements.txt
                  ```

                  ## Model Results

                  | Model | Accuracy |
                  |-------|----------|
                  | Logistic Regression | 89.9% |
                  | Random Forest | 97.7% |
                  | Random Forest (leakage removed) | 74.6% |

                  The honest production accuracy after removing leakage-prone features is 74.6%.

                  ## How to Run

                  1. Clone the repo
                  2. ```bash
                     git clone https://github.com/Dtumuhairwe/netflix_user_churn_prediction.git
                     cd netflix_user_churn_prediction
                     ```

                     2. Install dependencies
                     3. ```bash
                        pip3 install pandas scikit-learn matplotlib seaborn jupyter
                        ```

                        3. Open notebooks in order
                        4. ```bash
                           jupyter notebook
                           ```

                           ## Author

                           **Doreen Tumuhairwe**
                           M.S. Data Science — University of the Pacific, Stockton CA
                           [LinkedIn](https://linkedin.com/in/dtumuhairwe) | [GitHub](https://github.com/Dtumuhairwe)
