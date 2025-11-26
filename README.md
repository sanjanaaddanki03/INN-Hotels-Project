# Predicting Hotel Booking Cancellations using Machine Learning

This project builds classification models to help INN Hotels predict whether a booking is likely to be canceled. With a cancellation rate of 32.8%, accurate prediction can help reduce revenue loss, improve occupancy planning, and guide strategic decisions.

## Project Overview

INN Hotels experiences significant financial losses due to cancellations. These cancellations are associated with high lead times, high room prices, and certain market segments. The goal of this project is to analyze booking data, understand key patterns, and develop machine learning models that identify high-risk bookings before cancellations occur.

## Dataset

- **Rows:** 36,275  
- **Columns:** 19  
- **Target Variable:** Canceled (Yes/No)  
- **Class Balance:**  
  - Canceled: 32.8%  
  - Not Canceled: 67.2%  
- **Missing Values:** None  

## Exploratory Data Analysis (EDA)

Key trends observed:

- Peak booking months: August–October  
- Highest cancellation months align with peak booking season  
- Room prices rise during peak months and correlate with cancellations  
- Online market segment has the highest cancellation rates  
- Guests with more special requests tend to be more committed  
- Longer lead times strongly increase cancellation likelihood  
- Repeated guests rarely cancel  

## Data Preprocessing

- Outliers detected but not removed (natural variation in price and lead_time)  
- Created new features:
  - **no_of_family_members = no_of_adults + no_of_children**
  - **total_days = no_of_week_nights + no_of_weekend_nights**
- One-hot encoded:
  - type_of_meal_plan  
  - room_type_reserved  
  - Market_segment_type  
- Added constant term for logistic regression

## Modeling Approach

Two main models were developed:

1. **Logistic Regression**
2. **Decision Tree Classifier**  
   - With class balancing  
   - Pre-pruning and post-pruning techniques

### Logistic Regression Performance (Default Threshold)

| Metric | Value |
|-------|--------|
| Accuracy | 0.805 |
| Recall | 0.632 |
| Precision | 0.739 |
| F1 | 0.682 |

### Important Logistic Regression Predictors

- **lead_time:** increases cancellations  
- **no_of_previous_cancellations:** increases cancellations  
- **no_of_special_requests:** decreases cancellations  
- **repeated_guest:** strongly decreases cancellations  
- **market_segment_type_Offline:** decreases cancellations  

### Threshold Tuning

- **AUC-ROC suggested threshold:** 0.37 → Recall improved to 0.739  
- **Precision-Recall curve threshold:** 0.42  

Accuracy was not the primary metric because correctly predicting “not canceled” dominates the score.

## Decision Tree Model

A decision tree was trained using class weighting to address imbalance.

### Tree Versions Tested

- Default tree (overfit)  
- Pre-pruned tree  
- Post-pruned tree (best performance)

### Final Decision Tree Performance (Post-Pruned)

| Split | Accuracy | Recall |
|-------|----------|---------|
| Train | 0.888 | 0.884 |
| Test | 0.872 | 0.851 |

### Key Decision Tree Features

- avg_price_of_room  
- lead_time  
- arrival_month  
- no_of_special_requests  

## Final Model Selection

- **Best model:** Post-pruned Decision Tree  
- **Reason:** Achieves the highest recall (0.85), which is critical because the business wants to minimize false negatives (missed cancellations).

## Business Recommendations

- Monitor high-risk segments, especially the online market segment  
- Track high lead-time bookings and consider incentives to reduce cancellation risk  
- Encourage special requests, as these correlate with lower cancellations  
- Apply dynamic pricing to reduce cancellation likelihood during peak months  
- Strengthen loyalty programs since repeated guests rarely cancel  
