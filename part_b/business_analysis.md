# Part B: Business Case Analysis
## B1. Problem Formulation
**(a)** Target Variable: number of items sold.  
Candidate Input Features:
- Store Location
- Location Type
- Store Size
- Traffic
- Promotion Type
- Date
- Day
- is weekend?
- is festival?

Since we are trying to predict numerical values, rather than a categrory, this would make the machine learning problem of **Regression** type.

**(b)** Total sales revenue is more broader and general, meaning it doesn't offer much details and can be skewed by promotions. For example, promotions can increase the sales even if revenue is low. "items sold" can offer a cleaner measure of customer response to a given promotion. Hence items sold is a more reliable option. A target revenue should directly reflect business objective.

**(c)** Since stores in different locations respond very differently to the same promotion, using one single global model across all 50 stores, could confuse the model or confuse us with results. We can use separate models for different location types, so the model can learn from that particular location or location type only. Like a local shopkeeper who knows their area the best.
_______________________________________________________
## B2. Data and EDA Strategy
**(a)** Since the transactions table contains millions of rows for all the transactions, we aggregate it by grouping store and dates, calculating the items sold and bill amount, we get a table where each row is a specific store in a specific month. We join the store attributes table with the transactions table to know the location type and store size etc. We join the calender table and promotion details to see the promotions used in each month.

**(b)**
- **Box Plot or Bar chart** to see which promotions give the highest number of items sold.
- **Line chart** to look at sales trends over the months.
- **Scatter Plot**, sales vs. footfall to check for correlation. 
- **Heatmap** for a correlation matrix.

**(c)** If 80% of the transactions occured without any promotions, the model may predict that promotions contribute very less and focus on non promotions sales. To prevent this, we can use `stratify` to ensure the promotion days/months are well represented in training.
_______________________________________________________
## B3. Model Evaluation and Deployment
**(a)** Monthly store level data of three years across 50 stores is a huge dataset. In this case, we can split the data as 70 - 80% training and 20 - 30% testing data. This data is a time ordered data. If we use random split, the time order could get randomized. This would lose any patterns, and the model could train on future dates and test on past dates, which is not ideal. Hence random split is not appropriate in this case. 
RMSE (Root Mean Squared Error) and MAE (Mean Absolute Error) metrics would be used. 

**(b)** Feature importance identifies which input variables had the greatest impact on the model's decision. The model may have learnt that "loyalty points" works well, in store 12, during holiday season. That's why the model recommended "loyalty points bonus" promo during December. The model may have also learnt that sales drops during March, in store 12. So, to increase sales, giving "Flat discounts" may attract customers to buy.

**(c)** We can use libraries like `joblib` or `pickle` to the model's pipeline. This ensures that the rule for the data and prediction that we gave, stays frozen, and can be reused. Since the model needs to make new predictions at the start of every month, we can feed the model the current state of the stores, the calender of the upcoming month, and make the model run predictions for all five promotions and pick the best one from it. To maintain the model's performance, we can compare the model's predictions with what actually happened in that month. We can retrain the model using the recent data to catch newer patterns, every few months or years.