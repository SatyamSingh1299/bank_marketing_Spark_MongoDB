# Machine Learning Using Bank Marketing Data

This project builds a predictive machine learning pipeline on Portuguese bank marketing data to forecast whether a client will subscribe to a term deposit.  

## Project Overview

- **Goal**: Predict client term deposit subscription (yes/no) using demographic, financial, and campaign-related attributes.   
- **Dataset**: `bank-additional-full.csv` from the UCI Machine Learning Repository (41,188 rows, 20 input features, 1 binary target `y`).   
- **Approach**: Use a Decision Tree classifier implemented in PySpark on a scalable big data stack (Azure + Spark + MongoDB).   

## Tech Stack

- **Languages**: Python, PySpark   
- **Compute**: Apache Spark on Microsoft Azure VM  
- **Storage**: CSV (raw), Parquet (processed), MongoDB (NoSQL)   
- **Tools**: Jupyter Notebooks, PyMongo   

## Data and Features

- **Source**: UCI Bank Marketing dataset (`bank-additional-full.csv`, 2008–2010).    
- **Size**: 41,188 rows, 21 variables (20 features + 1 target).    
- **Sample features**:  
  - Demographic: `age`, `job`, `marital`, `education`  
  - Financial: `default`, `housing`, `loan`  
  - Campaign: `contact`, `month`, `day_of_week`, `duration`, `campaign`, `pdays`, `previous`, `poutcome`  
  - Economic: `emp.var.rate`, `cons.price.idx`, `cons.conf.idx`, `euribor3m`, `nr.employed`   
- **Target**: `y` (binary; client subscribed to term deposit: yes/no). 

<p  align="center"><img width="80%" src="https://github.com/SatyamSingh1299/bank_marketing_Spark_MongoDB/blob/main/imgs/Flow.png" /></a></p>   

<p  align="center"><img width="80%" src="https://github.com/SatyamSingh1299/bank_marketing_Spark_MongoDB/blob/main/imgs/mogoDBConnection.png" /></a></p>   


## Architecture and Pipeline

The project runs on an Azure VM and connects Spark, Parquet, and MongoDB in a unified pipeline. 

1. **Ingestion & Storage**  
   - Load CSV into Spark DataFrame.  
   - Convert CSV to Parquet to improve query performance and storage efficiency. 

2. **Preprocessing**  
   - Fix data types after CSV → Parquet conversion (convert strings back to numeric types).   
   - Remove or handle null values.   
   - Encode categorical variables with `StringIndexer`. 

3. **MongoDB Integration**  
   - Write: Convert `features` and `label` into a MongoDB‑friendly structure and insert into `Project_data.BankFull` using PyMongo.   
   - Read: Load stored data back into Spark and rebuild feature vectors (e.g., `SparseVector`).  

4. **Modeling**  
   - Assemble feature vector with `VectorAssembler`.   
   - Convert label from `yes`/`no` to binary (1/0).   
   - Train a `DecisionTreeClassifier` in PySpark on training data. 

5. **Evaluation**  
   - Use `MulticlassClassificationEvaluator` to compute accuracy and F1-score.   
   - **Accuracy** ≈ 91.05%, **F1-score** ≈ 89.91%. 

## Exploratory Data Analysis

- Most clients in the dataset did **not** subscribe to a term deposit, indicating class imbalance.   
- Common job categories include administrative, blue-collar, and technical roles.   
- Most clients were married and had either university or high school education. 

## Results, Limitations, and Future Work

- The Decision Tree model achieved strong performance while remaining interpretable, helping highlight influential features in subscription decisions.    
- Limitations include class imbalance, a single-country/single-bank dataset, and a restricted timeframe (2008–2010).    
- Future work:  
  - Experiment with other models (Random Forest, Gradient-Boosted Trees, Logistic Regression).  
  - Address class imbalance with resampling or class weights.  
  - Automate the pipeline (scheduling, orchestration) and expose a simple API or UI for marketing users.  

