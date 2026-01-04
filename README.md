# NYC-Taxi-Fare-Analysis

## 📓 Project Notebook

The complete analysis, data preparation, feature engineering, exploratory analysis, and modeling workflow is available in the Jupyter Notebook below:

**[NYC_Taxi_Fare_Analysis.ipynb](NYC_Taxi_Fare_Analysis.ipynb)**  

This notebook contains:
- Data loading and sampling using Apache Spark
- Data quality checks and cleaning
- Exploratory data analysis (EDA)
- Feature engineering
- Fare prediction modeling and evaluation
- Visualizations and diagnostics

You can view the notebook directly on GitHub or run it locally using Jupyter Notebook or JupyterLab.


## Executive Summary

This project presents an end-to-end analysis of **NYC Yellow Taxi trip data (2024)** with the goal of understanding fare behavior, demand patterns, and fare predictability at scale.

Using **PySpark**, the analysis processes millions of taxi trips to:
- clean and validate real-world transportation data,
- explore temporal and geographic demand patterns,
- engineer meaningful trip-level features, and
- build predictive models to estimate total taxi fares.

A statistically representative **7% sample (~2.49 million trips)** was used to balance scalability with reproducibility. Exploratory analysis reveals strong time-of-day and weekday vs weekend demand effects, concentration of trips in commercial and transit-heavy zones, and clear non-linear relationships between trip distance, duration, and fare.

Multiple predictive approaches were evaluated. A **Random Forest regression model** achieved the best performance, significantly outperforming linear baselines and explaining the majority of fare variance. Diagnostic analysis shows the model performs reliably for common fare ranges while exhibiting higher uncertainty for extreme, long-distance trips—an expected outcome in urban mobility data.

This project demonstrates:
- scalable data processing with PySpark,
- disciplined exploratory data analysis,
- thoughtful feature engineering,
- evidence-driven model selection, and
- clear communication of insights and limitations.

The repository is designed to be **fully reproducible**, with all assumptions, data sources, and methodology explicitly documented.

---

## Data Source

- **Provider:** NYC Taxi & Limousine Commission (TLC)
- **Dataset:** Yellow Taxi Trip Records
- **Year Used:** 2024  
- **Official Portal:** https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

> Due to GitHub file size limits, the full raw dataset is not stored in this repository.  
> A sampled version is used for analysis and documented under `data/samples/`.

## Problem Statement

New York City taxi fares are influenced by multiple interacting factors such as trip distance, trip duration, time of day, day of week, passenger count, and traffic conditions. While the fare calculation rules are defined, real-world trip behavior introduces variability that makes fare outcomes non-trivial to estimate or explain at scale.

From a **business perspective**, understanding these patterns is critical for:
- forecasting revenue,
- identifying peak demand windows,
- evaluating operational efficiency, and
- supporting pricing, staffing, and policy decisions.

From an **analytics perspective**, the NYC Yellow Taxi dataset presents classic real-world challenges:
- very large data volume,
- noisy and inconsistent records,
- extreme outliers,
- strong temporal and spatial effects, and
- non-linear relationships between variables.

The objective of this project is therefore to:
1. Clean and validate large-scale taxi trip data.
2. Explore demand and fare patterns across time and geography.
3. Engineer meaningful trip-level features that capture real behavior.
4. Build and evaluate predictive models to estimate total trip fare.
5. Clearly communicate insights, limitations, and practical implications.

The analysis prioritizes **accuracy, scalability, and interpretability of results**, while acknowledging the inherent uncertainty present in urban mobility data.

## Steps Performed

This project follows a clear, end-to-end analytics workflow, designed to mirror how large-scale data problems are handled in real-world environments.

### 1. Data Ingestion and Sampling
The original NYC Yellow Taxi Trip Records dataset for 2024 contains tens of millions of rows. To enable efficient development while preserving real-world distributions, a **7% random sample** was created using PySpark and stored in Parquet format.

This approach ensured:
- scalable processing,
- consistent schema,
- and realistic statistical behavior,
while avoiding memory and storage constraints.

---

### 2. Data Quality Checks and Cleaning
Initial validation focused on identifying and removing records that violated basic business rules, including:
- zero or negative passenger counts,
- zero or negative trip distances,
- negative fare or total amounts,
- invalid trip durations where drop-off occurred before pickup.

Outliers were analyzed separately rather than removed blindly, ensuring that rare but valid trips were not discarded prematurely.

---

### 3. Exploratory Data Analysis (EDA)
EDA was performed to understand both **demand behavior** and **fare dynamics**, including:
- trip volume by hour of day and day of week,
- weekday vs weekend demand patterns,
- pickup zone concentration using mapped taxi zone names,
- revenue distribution across time,
- fare, distance, duration, and tip distributions.

Visualizations were used extensively to surface skewness, long tails, and peak-demand windows.

---

### 4. Outlier Analysis and Capping
Rather than removing extreme values, key numeric variables were **capped at the 99th percentile**.  
This preserved real-world variability while preventing extreme values from dominating model training.

Variables capped included:
- trip distance,
- fare amount,
- total amount,
- tip amount,
- trip duration.

---

### 5. Feature Engineering
Additional features were derived to better capture trip context and behavior:
- trip duration (minutes),
- pickup hour,
- pickup day of week,
- weekend indicator,
- fare per mile,
- average trip speed (mph).

These features provided temporal, behavioral, and efficiency signals beyond raw trip data.

---

### 6. Demand and Revenue Segmentation
Demand and revenue were analyzed across:
- hours of the day,
- days of the week,
- weekday vs weekend splits,
- high-volume pickup zones.

This helped distinguish commuter-driven patterns from leisure-oriented travel behavior.

---

### 7. Predictive Modeling
Multiple regression-based models were trained and evaluated to predict **total trip fare**, including:
- Linear Regression (baseline),
- Decision Tree Regression,
- Random Forest Regression.

Models were compared using RMSE and R² to balance accuracy and generalization.

---

### 8. Model Evaluation and Diagnostics
The final model was evaluated using:
- actual vs predicted fare plots,
- error distribution analysis,
- error behavior across fare ranges.

These diagnostics helped identify where predictions were strong and where uncertainty increased (especially for very high fares).

---

This structured approach ensures the analysis is **reproducible, scalable, and aligned with real-world decision-making needs**, rather than being a purely academic exercise.

## Model Results Comparison

Multiple regression-based models were trained and evaluated to predict the **total fare amount** for NYC Yellow Taxi trips. Each model served a specific purpose, from establishing a baseline to maximizing predictive accuracy.

### Models Evaluated

| Model                  | RMSE (↓) | R² Score (↑) | Key Observations |
|------------------------|----------|--------------|------------------|
| Linear Regression      | ~21.8    | ~0.12        | Underfit the data; unable to capture non-linear fare behavior |
| Decision Tree Regressor| ~6.5     | ~0.92        | Strong performance; captured complex relationships but prone to overfitting |
| Random Forest Regressor| ~5.7     | ~0.94        | Best overall performance; balanced accuracy and generalization |

---

### Why Linear Regression Fell Short
Taxi fare dynamics are inherently non-linear. Factors such as distance, duration, traffic conditions, and time-of-day interact in complex ways.  
Linear Regression failed to model these interactions effectively, resulting in high error and low explanatory power.

---

### Decision Tree vs Random Forest
The Decision Tree model showed a major improvement by learning non-linear splits and conditional logic. However, single trees tend to overfit specific patterns in the training data.

Random Forest improved upon this by:
- combining multiple decision trees,
- reducing variance through averaging,
- improving generalization on unseen data.

---

### Final Model Choice
The **Random Forest Regressor** was selected as the final model due to:
- the lowest prediction error,
- the highest R² score,
- stable performance across different fare ranges.

This model provides the most reliable foundation for downstream insights and recommendations.

## Final Predictions and Key Insights

Using the Random Forest model, the project generated accurate fare predictions across millions of taxi trips while preserving real-world variability.

### Key Predictive Insights

- **Trip distance and duration** are the strongest drivers of fare, but their impact is non-linear.
- **Time-based features** (pickup hour and day) significantly influence fare outcomes, reflecting congestion and demand patterns.
- **Weekend trips** exhibit different fare dynamics compared to weekdays, with higher variability driven by leisure travel.
- **Fare per mile** and **average speed** provide strong signals for identifying inefficient or high-cost trips.

---

### Prediction Behavior Across Fare Ranges

- Predictions are **most accurate for low to mid-range fares**, which represent the majority of trips.
- Error increases for **very high fares**, often linked to rare long-distance or long-duration trips.
- This behavior is expected and acceptable given the natural skew and rarity of such trips.

---

### Practical Implications

From a business and operations perspective, the model can be used to:
- estimate expected fares for planning and simulation,
- identify anomalous trips that deviate strongly from predicted values,
- support demand-aware pricing or operational forecasting,
- understand how time and location influence revenue patterns.

---

### Overall Outcome

The final model demonstrates that **high-quality fare prediction is achievable using engineered features and tree-based models**, even on large, real-world transportation datasets.

More importantly, the analysis highlights how **data-driven insights can explain demand, pricing, and efficiency patterns**, not just produce predictions.

## Conclusion & Limitations

### Conclusion

This project demonstrated an end-to-end analytical workflow on large-scale NYC Yellow Taxi trip data using PySpark. Starting from raw trip records, the analysis progressed through data cleaning, exploratory analysis, feature engineering, demand pattern analysis, and predictive modeling.

Key outcomes of the project include:
- Identification of clear **temporal demand patterns** by hour, day, and weekend behavior.
- Strong evidence that taxi demand and revenue are **concentrated during peak commuting and evening hours**.
- Successful construction of a **high-performing fare prediction model** using tree-based techniques.
- Validation that engineered features such as trip duration, average speed, and fare per mile significantly improve predictive accuracy.

The final Random Forest model achieved a strong balance between accuracy and robustness, making it suitable for real-world analytical use cases such as fare estimation, anomaly detection, and operational forecasting.

Overall, the project shows how scalable data processing and thoughtful feature design can turn raw transportation data into actionable insights.

---

### Limitations

While the results are strong, several limitations should be acknowledged:

- **External factors not included**  
  The dataset does not capture traffic conditions, weather, road closures, or special events, all of which can significantly affect trip duration and fare.

- **Geographic granularity**  
  Pickup and drop-off zones are treated categorically. Spatial relationships (e.g., distance between zones, routing complexity) are not explicitly modeled.

- **High-fare edge cases**  
  Prediction errors increase for rare, extremely high-fare trips. These cases are underrepresented and naturally harder to model.

- **Static pricing assumption**  
  The model assumes historical fare structures and does not account for future pricing rule changes or policy updates.

- **Not a real-time system**  
  This project focuses on batch analysis and offline modeling rather than real-time fare prediction.

---

### Future Improvements

Potential extensions to this work include:
- Incorporating weather and traffic data.
- Using geospatial features or route-level modeling.
- Exploring gradient-boosted models for further accuracy gains.
- Building a lightweight API for real-time fare estimation.

Despite these limitations, the project provides a solid, realistic foundation for data-driven analysis of urban transportation systems.

## How to Reproduce This Project (Setup Guide)

This project analyzes NYC Taxi trip data using Apache Spark in local mode.
Due to repository size constraints, large datasets are **not stored** in GitHub.
Instead, users are expected to download the data directly from the official source
and place it in the required folder structure before running the notebook.

---

### 1. Clone the Repository

Run the following commands:

    git clone https://github.com/<your-username>/NYC-Taxi-Fare-Analysis.git
    cd NYC-Taxi-Fare-Analysis

---

### 2. Prerequisites

Ensure the following are installed:

- Python 3.10 or above
- Java 11 or Java 17 (required for Spark)
- Apache Spark 3.5+
- Git
- Jupyter Notebook or VS Code with Jupyter extension
- Optional but recommended:
  Linux / WSL (Windows Subsystem for Linux) for smoother Spark execution

Spark is used in local (single-machine) mode.  
No distributed cluster is required.

---

### 3. Download the NYC Taxi Data

This project uses **NYC Yellow Taxi Trip Records**.

Official source:
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Download:
- Yellow Taxi Trip Records (Parquet format)
- Year: 2024

---

### 4. Folder Structure Setup

After downloading, organize the data as follows:

    NYC-Taxi-Fare-Analysis/
    ├── notebooks/
    │   └── nyc_taxi_analysis.ipynb
    │
    ├── data/
    │   └── raw/
    │       └── 2024/
    │           ├── yellow_tripdata_2024-01.parquet
    │           ├── yellow_tripdata_2024-02.parquet
    │           └── ...
    │
    ├── outputs/
    │   └── figures/
    │
    ├── requirements.txt
    └── README.md

The notebook assumes this directory structure.

---

### 5. Install Python Dependencies

Install required libraries using:

    pip install -r requirements.txt

---

### 6. Verify Spark Installation

Confirm Spark is available:

    spark-submit --version

Spark version 3.5.x or higher should be displayed.

---

### 7. Run the Notebook

Option A: Jupyter Notebook

    jupyter notebook

Open:
    notebooks/nyc_taxi_analysis.ipynb

Run all cells from top to bottom.

Option B: VS Code

- Open the repository folder in VS Code
- Open notebooks/nyc_taxi_analysis.ipynb
- Select a Python kernel with Spark available
- Execute cells sequentially

---

### 8. What the Notebook Does

The notebook performs the following steps:

- Loads raw Parquet files using Spark
- Creates a reproducible working sample
- Performs data quality checks and cleaning
- Conducts exploratory data analysis (EDA)
- Engineers features for modeling
- Trains and evaluates multiple regression models
- Compares model performance and prediction quality

---

### 9. Reproducibility Notes

- Sampling uses a fixed random seed
- All transformations are deterministic
- Metrics may vary slightly depending on hardware and Spark configuration



