**Zomato Delivery Operations Analytics – Final Report**

**1. Define the Problem Statement**
Zomato’s team needs a data-driven way to reduce late deliveries and improve customer satisfaction. Two  ML objectives were set:
- Predict “Time Taken (min)” for each order so that the ETA shown to customers are more accurate.
- Classify delivery-partner rating category (Excellent vs. Poor) so that operational planners can pre-assign complex orders to highly rated partners and target training to low-rated partners.
Accurate predictions are expected to yield:
- Fewer late deliveries and refunds,
- Better workforce scheduling, and
- Higher customer retention and order frequency.

**2. Model Outcomes or Predictions**
**Delivery-time estimation**
Learning Type: Regression
Supervision: Supervised
Target Variable: Time_taken(min)
Expected Output: Continuous ETA (minutes)
**Rider Rating Flag**
Learning Type: Classification
Supervision: Supervised
Target Variable: Rating_Category (Excellent/ Poor)
Expected Output: Binary label

**3. Data Acquisition**
Primary source: Zomato Delivery Operations Analytics Dataset on Kaggle 
Food Delivert Dataset contains 45,584 rows and 20 columns, with a memory usage of 33.64 MB. This represents a substantial dataset for food delivery analysis with comprehensive coverage across multiple variables.
Additional context data introduced:
- Haversine distance between restaurant and customer calculated from lat/long.
- Calendar attributes (hour, weekday, weekend flag) derived from timestamps.
The diverse feature mix covers temporal, spatial, operational and human factors—visually confirmed in the notebook via distribution plots, correlation heat-map, and scatter of distance vs. time (right-skew with positive trend), indicating strong predictive signal.

**4. Data Pre-processing / Preparation**
a. Missing-value handling
- Numerical columns (Delivery_person_Age, Delivery_person_Ratings, etc.): imputed with median.
- Categorical columns (Type_of_vehicle, Weather_conditions, etc.): imputed with mode.
- Reduced missing-value rate to zero.
b. Train–test split
- 80% train / 20% test using stratified sampling on Rating_Category to balance classes.
c. Additional processing
- Datetime conversion to pandas datetime64 then extraction of hour, day, month.
- One-hot encoding of categorical variables (vehicle type, weather, order type).
- Standardization of continuous features for linear models; tree-based models kept raw.

**5. Modeling**
- Linear Regression (for delivery time prediction)
- Random Forest Regressor (for delivery time prediction)
- Logistic Regression (for rating categorization)
- Random Forest Classifier (for rating categorization)
Delivery Time Prediction
- Random Forest outperformed Linear Regression, achieving much higher accuracy in predicting delivery times.
- Most influential factors: delivery speed, distance, delivery person ratings, and multiple deliveries.
Rating Categorization
- Both Logistic Regression and Random Forest had moderate accuracy (under 50%) in predicting delivery person rating levels.
- Model performance was best for identifying "Poor" and "Good" categories, but overall results suggest further improvement is needed.

**Model Iterations:**
- Using confusion matrix, improvement to Rating Categories were made changing from 4 categories to 3  with Quantile-based binning (equal-size bins) instead of manually grouping values. For Rating Categorisation models, class_weight='balanced' was  also used to further improve performance.
- RandomizedSearchCV to tune the hyperparameters was also used to cross validate against models for Rating Categorisation models. Random Forest Results after tuning were inline with model results see below:

Fitting 5 folds for each of 20 candidates, totalling 100 fits
Best hyperparameters found:
{'n_estimators': 100, 'min_samples_leaf': 8, 'max_features': 'log2', 'max_depth': None, 'class_weight': 'balanced'}
Random Forest Results (after tuning):
Accuracy: 0.4872
- higher order model (neural network model using Tenserflow from keras) also used to valid accuracy of Rating Categoration models.

**6. Model Evaluation**
Based on the model evaluation results:
- Adopt the Random Forest Regressor for delivery time predictions.
Its high accuracy and ability to handle complex relationships make it very effective for operational forecasting and optimising delivery logistics.
- Rating categorization models (Logistic Regression and Random Forest Classifier) delivered only moderate accuracy.
hold off on automation; prioritize improving data quality and exploring new models. Use current classification results mainly for internal monitoring, not business-critical decision-making.
