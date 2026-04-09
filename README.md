# bengaluru-house-price-prediction
A machine learning project designed to predict residential real estate prices in Bengaluru, India. This project focuses heavily on Data Engineering and Domain-Driven Cleaning to achieve a high-performance, interpretable model.

## Project Highlights
Accuracy: Achieved a consistent ~86% $R^2$ score through cross-validation.
Optimization: Used GridSearchCV to evaluate multiple algorithms (Lasso, Decision Trees, Linear Regression).
Robust Cleaning: Handled over 13,000 rows of messy data, including range-based square footage and non-standard bedroom configurations.

## The Data Engineering Pipeline
1. Feature Engineering & Cleaning
Structural Parsing: Converted non-numeric "size" strings into integer BHK values and handled "total_sqft" ranges by calculating their mean.
Domain-Specific Filtering: Implemented a "Business Logic" check where properties with less than 300 sq. ft. per bedroom were removed as data entry errors.
Dimensionality Reduction: Utilized One-Hot Encoding for locations, grouping low-frequency areas into an "Other" category to prevent the "Curse of Dimensionality."

2. Outlier Removal & Statistical Auditing
Price Per Sqft: Removed extreme cases using Mean and Standard Deviation (kept data within $1\sigma$).
Logical Consistency: Developed a custom function to remove instances where a 2-BHK was significantly more expensive than a 3-BHK in the same neighborhood.
Structural Ratio: Filtered out anomalies where the bathroom count exceeded the bedroom count by $\ge 2$.

## Model EvaluationA
lgorithm Selection
I utilized GridSearchCV to compare the performance of different regressors:
| Model | Best Score ($R^2$) |
| :--- | :--- |
| Linear Regression | 0.86 |
| Lasso Regression | 0.72 |
| Decision Tree | 0.74 |

Residual Analysis
I conducted a residual analysis to validate the model's assumptions. The resulting plot showed homoscedasticity for standard residential units (0–200 Lakhs), with increased variance in the luxury tier, indicating that high-end valuations are driven by latent luxury variables not present in the dataset.

## Usage
The final model is accessible via a streamlined inference function. Note that area_type was pruned from the final model after a sensitivity audit revealed it had a negligible impact on prediction accuracy compared to location and sqft. 

#### Example Usage:
predict_price(location='Indira Nagar', sqft=1500, bath=3, bhk=3, balcony=2)
##### Output: Estimated Price in Lakhs

🧪 Technologies Used
- Python (Pandas, NumPy, Matplotlib, Seaborn)
- Scikit-Learn (LinearRegression, ShuffleSplit, GridSearchCV)
- Pickle (Model Serialization)
