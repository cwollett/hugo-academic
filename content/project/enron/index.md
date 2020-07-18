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

While the circumstances are unfortunate, the amount of data that was made public following the investigation into Enron allows people to analyze for themselves what happened and who was likely involved. Numerous people have taken the subsequent email data and packaged it for others to look at from a data science perspective. One of the ideas behind these analyses are to try to figure out which people were likely at the center of the scandal. We will label these people as Persons of Interest (POIs) throughout the analysis. The dataset being used consists of 146 entries (18 persons of interest and 128 non-persons of interest) along with data concerning their financial involvement with Enron and their e-mail correspondence within the company. From that data, we will try to come up with a way to accurately classify a person as a POI or not.

## Data Cleaning

All cleaning and analysis utilized Python inside a Jupyter notebook. This allowed for comparisons in every step of the process. To start determining which pieces of the dataset might help catch persons of interest, calculations were completed to see correlations between POIs and non-POIs. The table below shows the features removed due to coverage concerns. If the model learned that a certain feature never (or always) appeared for a POI, it would fall back on that feature to categorize future POIs.

| Feature                   | All        |  POIs       |  NonPOIs    |
| ------------------------- | ---------- | ----------- | ----------- |
| director_fees             | 0.1        |  0.0        |  0.1        |
| expenses                  | 0.7        |  1.0        |  0.6        |
| loan_advances             | 0.0        |  0.1        |  0.0        |
| other                     | 0.6        |  1.0        |  0.6        |
| restricted_stock_deferred | 0.1        |  0.0        |  0.1        |
| total_payments            | 0.9        |  1.0        |  0.8        |
| total_stock_value         | 0.9        |  1.0        |  0.9        |

For additional exploration of outlier activity, seaborn was utilized to show boxplots of the possible features. This allowed a row corresponding to TOTAL to be found. This row is quite common on spreadsheets but unwanted when it comes to trying to predict POIs. As such, it was removed from the data. While the boxplots showed numerous other points that may statistically be categorized as outliers, they corresponded to actual employees and were kept. For instance, what appears to be an unnaturally large bonus or stock option for a particular employee may be a sure sign of being involved with the scandal.

The next step became finalizing feature choice to help predict persons of interest. Using a Gaussian Naive Bayes classification, and normalizing some data points to be percentages instead of raw values yielded this final set of features:

| Feature                     | Score |
| --------------------------- | ----- |
| percentage_bonus            | 20.86 |
| percentage_to_poi           | 16.64 |
| percentage_shared_with_poi  | 9.3   |
| percentage_from_poi         | 3.21  |
| percentage_salary           | 2.74  |

## Testing

In each of the following tests, validation was done using accuracy, precision, and recall metrics. While accuracy gives an overall idea of how well an algorithm is performing compared to others, precision and recall give more specific performance metrics. For the Enron data, high precision will mean that the people labeled as POIs are unlikely to actually be non-POIs and high recall will mean that the algorithm is not missing POIs that it should be catching.

### Gaussian Naive Bayes

As the GaussianNB algorithm has no tuning options, the initial run results are the only ones to consider. This yielded an accuracy of 84.375%, a precision of 100%, and a recall of 16.67%. This lets us know the algorithm is not classifying any Non-POIs as POIs, but it is also missing quite a few POIs that it should be catching.

### Decision Tree

For Decision Tree, GridSearchCV was utilized to help with parameter tuning. Accuracy again was 84.375%. The precision took a hit, with a score of 60%, but the recall increased to 50%. Compared to the GaussianNB algorithm, the accuracy is the same, but we are mislabeling some Non-POIs. However, we are missing fewer true POIs. Given the context, I would prefer the higher recall. While the lowered precision means a non-POI might get investigated, the higher recall means a true POI is less likely to be overlooked.

### Random Forest

As with Decision Trees, GridSearchCV was used for parameters. The Random Forest gave an accuracy score of 84.375% as before. The precision score was 66.67%, slightly better than the Decision Tree, but the recall took a hit to 33.3%. As stated previously, the recall seems to be favorable in this context as opposed to precision.

### AdaBoost

Parameters used in the GridSearchCV this time were the rate of learning and number of estimators. While these two parameters seem to have an inverse correlation, both were included as inputs as discrete values instead of intervals. This yielded an accuracy score of 81.25%. The precision and recall were both down from other algorithms with a precision of 50% and recall of 16.67%.

## Final Thoughts

Given the small size of the data set, the expectation was for Gaussian Naive Bayes to perform as well as, if not better, than more advanced classification techniques. While this was true as far as precision and overall accuracy, the fine-tuning capabilities of the more advanced algorithms let us improve recall drastically in the Decision Tree classifier. When choosing the best model to use, context must be considered. When you look at the data in the real world setting, you want to make sure you do not overlook potential persons of interest while not over-monitoring those who are likely innocent. In this scenario, that means having a high recall is preferred while still maintaining the highest precision possible. Out of the above results, I would utilize the Decision Tree model.
