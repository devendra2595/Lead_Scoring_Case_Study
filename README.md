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
7) Conclusion
8) Using PCA to verify the model performance.


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
    2.1.1) Handling Missing values in Categorical Variables:
      - Formed a new category named 'Unspecified' in each variable having missing values less than 40% and more than 5%. e.g City, Specialization, Occupation.
      - The missing values from 'Lead Source' variable were missing at random, imputed them with 'Reference'.
    2.1.2) Handling Missing values in Numerical Variables:
        'TotalVisits'
      - The majority of missing values from 'TotalVisits' variable had Lead Origin as 'Lead Add From' and 'Lead Import', and both these categories may signify that the lead has been added manually by the sales person into the system from a different Source which could be the reason TotalVisits not being captured.
      - Almost all the records of 'TotalVisits' variable with Lead Origin as 'Lead Add Form' or 'Lead Import' were 0. so imputed missing values with with 0.
        - As 'TotalVisits' were 0, the 'Page Views Per Visit' also became 0.

  2.2) Data Transformation:
    - Dropped unnecessary variables whch were not suitable for model building, like highly skewed, conveying similar information etc.
    - Merged low records categories together like, City, Occupation etc., merged categories with similar interests like, management specialization, Business Specialization etc.
    - Dropped variables with only single category.
    - Dropped two category variables which were highly skewed.
    
# 3) Exploratory Data Analysis:
  3.1 EDA- Numerical Variables:
![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/98abfd0d-74ad-4f87-b797-2895f4f77345)

![image](https://github.com/devendra2595/Lead_Scoring_Case_Study/assets/116253033/4abe061e-bc3b-4663-9516-d9e79b5b71a6)
- No significant correlation present between numerical variables









4) Data Preprocessing for model building
5) Model Building using StatsModels
6) Predictions and Model evaluation
7) Conclusion
8) Using PCA to verify the model performance.
