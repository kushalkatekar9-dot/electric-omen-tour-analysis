# Electric Omen Tour Analysis

## Project Overview
This project applies machine learning techniques to analyze historical tour logs for the band **"Electric Omen."** The primary goal is to predict **Crowd Energy** levels at various venues and use these insights to optimize tour revenue, scheduling, and performance details.

By analyzing factors such as venue type, weather, ticket prices, and the lead singer's theories, this project provides data-driven recommendations to maximize both fan engagement and profitability.

## Repository Structure
* **`ANALYSIS_NOTEBOOK.ipynb`**: The core Jupyter Notebook containing data cleaning, exploratory data analysis (EDA), feature engineering, and model training/evaluation.
* **`Findings_Report.pdf`**: A summary of venue-specific insights, hypothesis testing (Singer's theories), and model justification.
* **`revenue_optimisation.pdf`**: A strategic report detailing the profit formula and suggestions for maximizing revenue based on crowd energy and attendance.
* **`tour_logs_train.csv`**: The historical dataset used for training the models.
* **`predictions.csv`**: (If applicable) Model output containing predicted energy levels.

## Methodology

### 1. Data Cleaning & Preprocessing
The raw data contained missing values and potential data leakage columns. The following steps were taken:
* **Leakage Removal**: Columns such as `Crowd_Size` and `Merch_Sales_Post_Show` were removed from the training features because they are not known before a show starts.
* **Outlier Handling**: Rows with invalid `Crowd_Energy` values (outside the 0-100 range) were removed.
* **Imputation**: Missing values in `Volume_Level` and `Crowd_Size` (for retrospective analysis) were filled using venue-specific medians.

### 2. Modeling
Several regression models were evaluated to predict Crowd Energy:
* **Linear Regression**: Used as a baseline but failed to capture non-linear relationships.
* **Gradient Boosting**: Evaluated for performance.
* **Random Forest Regressor**: **Selected Model**. It delivered the lowest Root Mean Squared Error (RMSE) on the test set and handled the distinct behaviors of different venues effectively.
* **Hyperparameter Tuning**: The model was optimized using `GridSearchCV` with 5-fold cross-validation, tuning parameters like `n_estimators`, `max_depth`, and `min_samples_split`.

## Key Findings & Insights

### Venue-Specific Strategies
The analysis revealed that a standardized approach does not work; each venue has unique drivers for high energy:

| Venue | Best Day | Best Time | Best Outfit | Key Insight |
| :--- | :--- | :--- | :--- | :--- |
| **V_Alpha** (The Holy Grounds) | Sunday | 12:00 PM | Leather | Openers have little effect. |
| **V_Beta** (The Vampire's Den) | Sunday | 11:00 PM | Denim | Highest energy at night. Openers don't matter much. |
| **V_Gamma** (The Snob Pit) | Saturday | 3:00 PM | Spandex/Leather | **Openers matter highly.** Expensive tickets are seen as exclusive. |
| **V_Delta** (The Mosh Pit) | Sunday | 4:00 PM | Denim | Volume drives energy. Openers don't matter. |

### Testing the "Singer's Theories"
The data was used to validate the Lead Singer's intuition:
*  **"Tuesdays are a curse"**: False.
*  **"Goths only come alive at night"**: Correct (especially for V_Beta).
*  **"Time of day matters"**: Correct.
*  **"Snobs (Gamma) care about openers"**: Correct.
*  **"Full Moon = Magic"**: Partially Correct.
*  **"Rain definitely sucks"**: False (Rainy weather is actually best for V_Beta).

## Revenue Optimization
A specific profit formula was derived to balance ticket prices with attendance and ancillary spending (merch/drinks), which relies on Crowd Energy:

$$
\text{Profit} = (\text{Ticket Price} + \text{Spend}) \times \text{Size} - (\text{Fixed Cost} + \text{Cost Per Person} \times \text{Size})
$$

**Strategic Suggestions:**
* **V_Beta**: Schedule shows at night to maximize energy and revenue.
* **V_Gamma**: Increase ticket prices (exclusive appeal) and invest in high-rating openers to boost energy.
* **V_Delta**: Do not waste budget on expensive openers; focus on volume and weekend slots.
* **General**: Weekends consistently generate higher energy and should be prioritized for scheduling.
