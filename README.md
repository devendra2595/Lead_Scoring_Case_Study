# Lead_Scoring_Case_Study

# Problem Statement :
An education company named X Education sells online courses to industry professionals. On any given day, many professionals who are interested in the courses land on their website and browse for courses.

The company markets its courses on several websites and search engines like Google. Once these people land on the website, they might browse the courses or fill up a form for the course or watch some videos. When these people fill up a form providing their email address or phone number, they are classified to be a lead. Moreover, the company also gets leads through past referrals. Once these leads are acquired, employees from the sales team start making calls, writing emails, etc. Through this process, some of the leads get converted while most do not. The typical lead conversion rate at X education is around 30%.

Now, although X Education gets a lot of leads, its lead conversion rate is very poor. For example, if, say, they acquire 100 leads in a day, only about 30 of them are converted. To make this process more efficient, the company wishes to identify the most potential leads, also known as ‘Hot Leads’. If they successfully identify this set of leads, the lead conversion rate should go up as the sales team will now be focusing more on communicating with the potential leads rather than making calls to everyone.


# Business Goal :
X Education has appointed you to help them select the most promising leads, i.e. the leads that are most likely to convert into paying customers. The company requires you to build a model wherein you need to assign a lead score to each of the leads such that the customers with a higher lead score have a higher conversion chance and the customers with a lower lead score have a lower conversion chance. The CEO, in particular, has given a ballpark of the target lead conversion rate to be around 80%.

# Aim :
Our aim is to build a Classification model (logistic regression model) and identify 'Hot-Leads' from the pool of leads and assign them lead score based on the probability of conversion.

# Flow of Case Study:

1) Reading and understanding the data
2) Data Cleaning and Transformation
3) Exploratory Data Analysis
4) Data Preprocessing for model building
5) Model Building using StatsModels
6) Predictions and Model evaluation
7) Using PCA to verify the model performance.
8) Conclusion
9) Building Classification Model using Support Vector Machine and Random Forest
10) Ensembling classification models.


# 1) Reading and understanding the data
The data contains various attributes related to each enquiry.
Some of them are :
- origin and source of lead
- Total visits, time spent on website, number of pages viewed per visit
- City, Country
- Specialization opted, Occupation
- Last activity etc.

The data contains 9240 records and 37 variable with majority of variables as categorical.
The data imbalance is 38.54%.

# 2) Data Cleaning and Transformation:
  2.1) Handling Missing Values:
    - Some variables contained 'Select' value which is as good as Null, so imputed 'Select' with np.NaN.
    - Dropped variables with more than 40% missing values. imputing them with any suitable value would have caused skewness in the data and ultimately misinterpretation.
      - 2.1.1) Handling Missing values in Categorical Variables:
          - Formed a new category named 'Unspecified' in each variable having missing values less than 40% and more than 5%. e.
City, Specialization, Occupation.
          - The missing values from 'Lead Source' variable were missing at random, imputed them with 'Reference'.
      - 2.1.2) Handling Missing values in Numerical Variables:
          - The majority of missing values from 'TotalVisits' variable had Lead Origin as 'Lead Add From' and 'Lead Import', and both these categories may signify that the lead has been added manually by the sales person into the system from a different Source which could be the reason TotalVisits not being captured.
          - Almost all the records of 'TotalVisits' variable with Lead Origin as 'Lead Add Form' or 'Lead Import' were 0. so imputed missing values with with 0.
          - As 'TotalVisits' were 0, the 'Page Views Per Visit' also became 0.

  2.2) Data Transformation:
    - Dropped unnecessary variables whch were not suitable for model building, like highly skewed, conveying similar information etc.
    - Merged low records categories together like, City, Occupation etc., merged categories with similar interests like, management specialization, Business Specialization etc.
    - Dropped variables with only single category.
    - Dropped two category variables which were highly skewed.
    
# 3) Exploratory Data Analysis:
  3.1) EDA- Numerical Variables:
  
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/98abfd0d-74ad-4f87-b797-2895f4f77345)

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/4abe061e-bc3b-4663-9516-d9e79b5b71a6)

- No significant correlation present between numerical variables

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/719bc98c-a8ce-4d02-878a-5f069bc58098)

  3.2) EDA - Categorical Variables
  
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/b464d1ed-fbb0-404a-9176-d6ff4faa2f9b)
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/55c653af-5a7d-41d7-ba1a-8a9a268dc481)
- "Lead Origin"- although "Landing page submission" and "API" have higher contribution, "Lead Add Form" have higher percentage of conversion.
- "Lead Source"- "direct traffic" & "Google" have highest contribution but "reference" and 'welingak website' ensures the conversion in most of the cases. 
- "Do not email"- we can say that those who agree to not receive email have slightly lower conversion rate.
- "Specialization" - Most of the clients opt for management specialization.
- "Occupation" - a lot of clients are unemployed but there is quite high chance of conversion if client is employed(working professional).
- "City" - although most of the people are from mumbai, but the conversion rate is quite similar for non-mumbai or other cities.
- 'A free copy of Mastering The Interview' - although most of the people do not want a free copy, the variable does not seem to have any significant impact on the conversion rate.
- 'Last Notable Activity' - If the Last notable activity is 'SMS Sent' then there is high chance of conversion. also, Modified and email opened categories have good number of conversions but not converted lead count is still higher.

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/79c769f0-5e21-45d9-8c0c-f8f44501cf1e)

- For any specialization, If the Lead origin is 'Lead Add Form' then the conversion chances are higher.
- Customer with 'Busisess' Specialization and lead origin as 'Lead Import' have high chances of conversion.
- If Specialization is 'Unspecified' then its better to keep the lead as as last priority except if Lead origin is 'Lead Add Form'.

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/2c721eef-4032-45ef-8a37-7721d60ce494)

- If the Lead origin is 'Lead Add Form' then the conversion chances are higher for all the cities.
- Lead Origin 'API' also has moderate chances of conversion across any city except if city is 'Unspecified'
- If City is not mentioned then its better to not prioritize that lead unless it has Lead origin  as 'Lead Add Form'

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/433baccd-79cf-485f-80a7-aec543ed3c02)

- Majority of conversions happen when Lead origin is 'Lead Add Form','Landing Page Submission' or 'API' across all 'Occupation' except 'Unspecified'.
- Leads in which Occupation is 'Unspecified', should be our last priority.
- Working professionals have highest conversion ratio across all the Lead Origin

# 4) Data Preprocessing for model building:
  4.1) Dummy Variable Creation (One-Hot Encodin) 
  - Mapped variables with categories as Yes/No to 1/0.
  - Once we create dummy variables, we have to drop one column from each variable to avoid multi-collinearity and dummy variable trap. This can be done using, pd.get_dummies(df,drop_first=True), but manually dropped variables which we do not want to have as a predictors in our final model to make our model more interpretable. e.g. City_Unspecified, Lead_source_others etc.
  4.2) Handling Outliers :
    - Capped 'TotalVisits' and 'Page Views Per Visit' at 99th percentile to remove outliers. (150 records were removed)
  4.3) Splitting data into Train and Test Set:
    - Splitted into train and test set in 70/30 ratio.
  4.4) Scaling the numerical data (Normalization):
    - Scaled train data using MinMax Scaler. (fit_transform)
    - Transformed test data using same scale from train dataset. (transform)
      
# 5) Model Building using StatsModels:
  - Built 1st model using all the 37 variables, and as expected, a lot of the predictors had p-value > 0.05, which made them
insignificant.
  - Used Rrcursive Feature Elimination technique to reduce the number of predictors to 25.
  - Built 2nd model using 25 predictors and got majority but not all, predictors having p-value < 0.05 and vif < 5.
  - Step by step dropped insignificant predictors (p-value>0.05) and highly correlated predictors(vif>5) and built model after each operation.
  - Also, kept a keen eye on the signs of predictors and checked if any of the predictors are flipping signs, if they do then our model would have been highly unstable having predictors which are collinear with each others rather than target variable.
  - Reached final model number 5, with 22 predictors, having p values within 0.05, it means all the variables are significant, and all vif's within 5 which means there no multicollinearity.

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/f4f3b89e-c270-4c1e-9415-ee307d62f466)

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/210e4c13-66d4-44fd-b6d1-36246ac36fe6)


# 6) Predictions and Model evaluation: 

  6.1) Predicting target variable (train dataset):
  - The output we got as prediction, is the probability of successful conversion of lead.
  - This probability can be converted into score out of 100 and utilised as lead score but how would we know which lead is a potential hot-lead? for that, we need to decide a cut-off as per our business needs.
  - Assumed cut-off to be 0.5 and found out the evaluation metrics as follows:
    1) Sensitivity (TPR)- 71.33
    2) Specificity - 88.41
    3) False Positive Rate (FPR) - 11.58
    4) Positive Predictive Value - 79.15
    5) Negative Predictive Value - 83.32
  - From this, we can say that we were able to correctly predict,
      - 71.33% of leads which are going to convert.
      - 88.41% of leads which are not going to convert
  - We need to improve the results by tweaking the cut-off value. but there is a trade-off involved between sensitivity and specificity which can be visualised by ROC curve.

  6.2) Plotting the ROC Curve:
  - It shows the tradeoff between sensitivity and specificity (any increase in sensitivity will be accompanied by a
decrease in specificity).
  - The closer the curve follows the left-hand border and then the top border of the ROC space, the more accurate the test.
  - The closer the curve comes to the 45-degree diagonal of the ROC space, the less accurate the test.
  
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/f59473d0-088a-420a-9672-851495425b6e)

  6.3) Finding Optimal Cutoff Point:
  - Optimal cutoff probability is that prob where we get balanced sensitivity and specificity.
  - Found out Sensitivity, Specificity and Accuracy at various probability cut-offs from 0 to 0.9 so that by plotting line chart we would get an idea where all the metrics have balanced values.
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/26439a76-f6db-4f92-b62a-4dc83f2cd4ab)
  - At cut-off 0.36, found out the evaluation metrics as follows:
    1) Sensitivity (TPR)- 80.60
    2) Specificity - 82.43
    3) False Positive Rate (FPR) - 17.56
    4) Positive Predictive Value - 73.90
    5) Negative Predictive Value - 87.32
  - We can observe the improvement in the Sensitivity but Specificity is decreasing. So, as per the business requirements we can tweak the probability cut-off and attain desired results.
  - By implementing the model to our business, definitely there would be some extent of misclassification and we could encounter opportunity loss, ultimately resulting into revenue loss.
  - For our businees, we are more inclined towards finding out the extent to which our model can predict conversion ratio and the opportinity loss related to the misclassified cases.
  - And, this can be achieved by finding out Precision(Positive Predictive Values) and Recall(True Positive Rate).
  - We have already found out the above metrics, let's dive deeper into their interpretation and find out optimal probability cut-off value.

  6.4) Precision recall tradeoff
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/cabfa195-5e7a-473e-8b65-f19b192d087c)

  - From precision recall graph, we can take 0.42 as optimal final probability cutoff.  but as our target is to predict around 80% leads which are being successfully converted, we can even choose cut-off higher than 0.42 to increase the precision of our model but at the cost of decreased positive predictive values. Lets choose cutoff as 0.47.
      - At cut-off 0.47, found out the evaluation metrics as follows:
        1) Precision : 78.04
        2) Recall : 73.51
      - As we can see, we are getting around 78% precision and sensitivity is around 74%. So, if we completely depend on the predictions of our model, then we can still achieve around 74% (Recall) of current the revenue, while only catering to 2290 (Predicted as converted by our model) leads which accounts to around 36% of the leads.
      - As our aim was to increase the conversion ratio to around 80%, we are able to reach close to this figure.
      - Let's find out test results.
        
  6.5) Making predictions on the test set:
  - At cut-off 0.47, found out the evaluation metrics on test set as follows:
      1) Precision : 77.22
      2) Recall : 71.50
      
# 7) Conclusion (Based on test data): Cut-off Lead Score - 47
   
- We were able to achieve around 77% precision and around 72% recall on test data set.
- So, initially if we focus only on leads which are predicted as potential leads by our model, then we can successfully convert almost 77% from them and these 77% of the leads will give us 72% of the current revenue/business.
- Also, the model has predicted that out of total leads, 36% will get converted, so initially we do not need to focus on rest of the leads, which reduces the workload by 64%.   
- At the cost of 28% business, we can reduce the work load by almost 64%.
- Later on, as per the availability of workforce or time, we can focus on rest of the leads having lead score less than 47 and capitalize rest of the potential leads to increase the business/revenue. But it should be noted that the conversion ratio would gradually keep on decreasing with decrease in cut-off score.
   
# 8) Using Principal Component Analysis to verify the model performance:
   - By using PCA algorithm, with cut-off at 0.47, found out the evaluation metrics on test set as follows:
      1) Precision : 77
      2) Recall : 72
  - As we can see that, the logistic regression model that we built by using statsmodels and PCA have almost similar metric scores on test dataset.
  - So we can say that, we have used right predictors to build or LR model.

# 9) Building Classification Model using Support Vector Machine and Random Forest :
  - 1) Random Forest :
       - Used Grid Search to test various parameters like max_depth, min_samples_leaf, min_samples_split and built Random Forest Classifier.
       - Got the best estimator with max_depth = 6, min_samples_leaf = 20, min_samples_split = 80.
       - Results on test dataset:
         - Precision : 79.29
         - Recall : 76.10
       
  - 2) Support Vector Machine:
       -  using similart cut-off probability, 0.47, got below results on test set:
       -  Precision : 79.40
       -  Recall : 72.63
           
# 10) Ensembling Classification Models:
   - Used Stacking Classifier to emsemble Logistic Regression Model, Supprt Vector Machine Classification Model and Random Forest Classification Model.
   - Used label encoded data instead of one-hot endoded data because it was better compatible with all 3 classifiers.
   - Selected Logictic Regression as meta model.
   - Got below results on test data using probability cut-off 0.45:
      - Precision : 78.10
      - Recall : 77.22
- Although precision percentage has not increased significantly, but the recall has increased by almost 6 %, which makes number of cases predicted as converted by our model 824 which was 763 as predicted by LR model.
- This still contributes to 8% increase in converted cases over LR model, which is significant in terms of business. 
-------------------------------------------------------------------------------------------------------------------


