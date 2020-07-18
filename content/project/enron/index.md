---
title: Enron Data Analysis
summary: Project as part of Uadcity's Intro to Machine Learning course (2016).
# tags: 
# date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by Steve Ueckert, Houston Chronicle 
  focal_point: Smart

links:
- icon: external-link-alt
  icon_pack: fas
  name: Udacity Course
  url: https://www.udacity.com/course/intro-to-machine-learning--ud120
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

## Background
In the 1990's, Enron was known as one of the most innovative and successful companies in the United States. Though it began in the natural gas industry, Enron was able to spread into other utility industries and find ways to turn a profit. Unfortunately, this success is not what most people think of today when they hear "Enron." Today, the company is most remembered for accounting and stock scandals that eventually ended with a bankruptcy declaration for the company.

While the circumstances are unfortunate, the amount of data that was made public following the investigation into Enron allows people to analyze for themselves what happened and who was likely involved. Numerous people have taken the subsequent email data and packaged it for others to look at from a data science perspective. One of the ideas behind these analyses are to try to figure out which people were likely at the center of the scandel. We will label these people as Persons of Interest (POIs) throughout the analysis. The dataset being used consists of 146 entries (18 persons of interest and 128 non-persons of interest) along with data concerning their financial involvement with Enron and their e-mail correspondence within the company. From that data, we will try to come up with a way to accurately classify a person as a POI or not.

## Data Cleaning
While exploring the data, I utilizezd pandas DataFrames to allow easier visualization and manipulation of the data I wanted to keep. This allowed the completely string-based email address feature to be dropped as well as allowed counts to be made by person and by features to see which items were well-represented and which items were empty or near empty. For instance, one person (Eugene E Lockhart) lacked any entries in the financial or email features and so was removed. Another "person", The Travel Agency in the Park, turned up. While the company could have been a front for something more shady, it held very few data points and was also removed.

On the feature side of things, a DataFrame was utilized to see how many data entries existed for each feature. Calculations were done on the dataset as a whole, on POIs only, and on non-POIs:

| Feature                   |	All        |	POIs       |	NonPOIs    |
| ------------------------- | ---------- | ----------- | ----------- |
| bonus                     |	0.6	       | 0.9         |	0.5        | 
| deferral_payments         |	0.3        | 0.3         |	0.3        |
| deferred_income           |	0.3        |	0.6        |	0.3        |
| director_fees             |	0.1        |	0.0        |	0.1        |
| exercised_stock_options   |	0.7        |	0.7        |	0.7        |
| expenses                  |	0.7        |	1.0        |	0.6        |
| from_messages             |	0.6        |	0.8        |	0.6        |
| from_poi_to_this_person   |	0.6        |	0.8        |	0.6        |
| from_this_person_to_poi   |	0.6        |	0.8        |	0.6        |
| loan_advances             |	0.0        |	0.1        |	0.0        |
| long_term_incentive       |	0.5        |	0.7        |	0.4        |
| other                     |	0.6        |	1.0        |	0.6        |
| restricted_stock          |	0.8        |	0.9        |	0.7        |
| restricted_stock_deferred |	0.1        |	0.0        |	0.1        |
| salary                    |	0.7        |	0.9        |	0.6        |
| shared_receipt_with_poi   |	0.6        |	0.8        |	0.6        |
| to_messages               |	0.6        |	0.8        |	0.6        |
| total_payments            |	0.9        |	1.0        |	0.8        |
| total_stock_value         |	0.9        |	1.0        |	0.9        |

The rows highlighted were removed due to coverage concerns. Either there was very little converage, or the coverage seemed disproportionate between POIs and non-POIs. I did not want any algorithms to notice perfect coverage for POIs and immediately assign anyone with a valid data point in that category to be assigend POI status. From there, the following columns were introduced to the dataframe to incorporate removed data as well as acting as a scaling of the data:

```percentage_bonus = bonus / total_payments```
```percentage_salary = salary / total_payments```
```percentage_exercised_stock = exercised_stock_options / total_stock_value```
```percentage_from_poi = from_poi_to_this_person / to_messages```
```percentage_to_poi = from_this_person_to_poi / from_messages```
```percentage_shared_with_poi = shared_receipt_with_poi / to_messages```

For additional exploration of outlier activity, seaborn was utilized to show boxplots of the possible features. This allowed a row corresponding to TOTAL to be found. This row is quite common on spreadsheets but unwanted when it comes to trying to predict POIs. As such, it was removed from the data. While the boxplots showed numerous other points that may statistically be categorized as outliers, they corresponded to actual employees and were kept. For instance, what appears to be an unnaturally large bonus or stock option for a particular employee may be a sure sign of being involved with the scandal.

Due to the low number of data points in the set, a Gaussian Naive Bayes classification was chosen as a first attempt to predict persons of interest. To choose features, SelectKBest and f_classif from Python's scikit-learn were utilized. This allowed all possible features to be given a score based on the significance the feature has in the model. These scores were:

| Feature                     |	Score |
| --------------------------- | ----- |
| percentage_bonus            |	20.86 |
| percentage_to_poi           |	16.64 |
| percentage_shared_with_poi  |	9.3   |
| shared_receipt_with_poi     |	8.75  |
| from_poi_to_this_person     |	5.34  |
| percentage_from_poi         |	3.21  |
| percentage_salary           |	2.74  |
| from_this_person_to_poi     |	2.43  |
| to_messages                 |	1.7   |
| deferral_payments           |	0.24  |
| exercised_stock_options     |	0.22  |
| deferred_income             |	0.21  |
| from_messages               |	0.16  |
| bonus                       |	0.07  |
| percentage_exercised_stock  |	0.05  |
| restricted_stock            |	0.03  |
| long_term_incentive         |	0.02  |
| salary                      |	0.00  |

The highlighted rows were utilized in the classifications. Note that while shared_receipt_with_poi and from_poi_to_this_person had quite high scores, they were thrown out. This was due to the new percentage features being added in. Keeping both the percentage features and the original e-mail features seemed like double-dipping. In order to ensure one feature was not getting too much weight, only the percentages were kept. While all values had been normalized using a Min/Max scaler, in the end it was unnecessary as my final features were all percentages.

## Testing
In each of the following tests, validation was done using accuracy, precision, and recall metrics from sklearn. While accuracy gives an overall idea of how well an algorithm is performing compared to others, precision and recall give more specific performance metrics. For the Enron data, high precision will mean that the people labeled as POIs are unlikely to actually be non-POIs and high recall will mean that the algorithm is not missing POIs that it should be catching.

### Gaussian Naive Bayes
As the GaussianNB algorithm has no tuning options, the initial run results are the only ones to consider. This yielded an accuracy of 84.375%, a precision of 100%, and a recall of 16.67%. This lets us know the algorithm is not classifiying any Non-POIs as POIs, but it is also missing qhite a few POIs that it should be catching.

### Decision Tree
For Decision Tree, GridSearchCV was utilized to help with parameter tuning. Criterion of gini and entropy as well as options for min_samples_split were given. Upon return, the best paramters were gini and a samples split of 15. Accuracy again was 84.375%. The precision took a hit, with a score of 60%, but the recall increased to 50%. Compared to the GaussianNB algorithm, the accuracy is the same, but we mislabeling some Non-POIs. However, we are missing fewer true POIs. Given the context, I would prefer the higher recall. While the lowered precision means a non-POI might get investigated, the higher recall means a true POI is less likely to be overlooked.

### Random Forest
As with Decision Trees, GridSearchCV was used on the parameters of criterion, min_samples_split, and number of estimators. This time, the return suggested to use gini, a split of 5, and 35 estimators. This gave an accuracy score of 84.375% as before. The precision score was 66.67%, slightly better than the Decision Tree, but the recall took a hit to 33.3%. As stated previously, the recall seems to be favorable in this context as opposed to precision.

### AdaBoost
Parameters used in the GridSearchCV this time were the rate of learning and number of estimators. While these two parameters seem to have an inverse correlation, both were included as inputs are discrete instead of intervals. The return suggested using a learning rate of 1 with 5 estimators which yielded an accuracy score of 81.25%. The precision and recall were both down from other algorithms with a precision of 50% and recall of 16.67%.

Given the small size of the data set, the expectation was for Gaussian Naive Bayes to perform as well as, if not better, than more advanced classification techniques. While this was true as far as precision and overall accuracy, the fine-tuning capabilities of the more advanced algorithms let us improve recall drastically in the Decision Tree classifier. As such, that would be the classification recommended for this particular data set.
