# Alert Machines: Detecting Fraud Before It Happens

## Intro

We all know that Covid19 has changed the way we live, work and travel. But not often do we think of the virus as also causing the rising levels of credit card fraud.The market in credit card data has exploded since the pandemic as people are making a lot more transactions online, where it is easier to steal card details.

It was recently documented how, on the dark web, credit card data for nearly half a million people was put on sale, data that includes everything necessary to make a purchases online, including the cardholder’s name, card number, CVV number, and expiration date. Price for these sets of attributes: only $10 each, managed and guaranteed by a proper dark web marketplace for stolen data.

While the actual, plastic credit cards have recently been made more secure due to enhanced security features and compliance standards, we have seen fraud shift to online transactions which now account for roughly 75 percent of all card fraud. It is the best of both of worlds for the thieves, more transactions mean more chance to steal data, and it also means that once stolen there will no shortage of opportunities to make illicit purchases.

## TASK

As the pandemic has made the credit card fraud a hot topic again, we decided to revisit one of the biggest fraud-related datasets, the one published by Vesta Corporation in 2019 as part of the Kaggle competition. Vesta is end-to-end transaction guarantee platform for online purchases, and have provided us with two files of data, with hundereds of columns and hunderds of thousands of rows each.

## Objectives

1) Develop a machine and deep learning models able to uncover and prevent online credit card fraud before it happens. The success of the models will be judged two-fold: rather than just trying to optimize a metric such as auc-roc (used in the Kaggle competition) or recall score, we plan to develop a few different models than can be deployed in different situations - a model with a reliable all-around score such as roc-auc for old, repeat customers and another model with high sensitivity (recall) for new, unknown customers.

2) vast computing power and resources can always be unleashed to tackle machine-learning problems when budget and cost-effectiveness are disregarded. This is not the case with this project: we will try to simplify the data to such as an extent that a simple laptop can run our models in less than two hours. As credit card fraud spreads geographically we want to create a model that can be deployed in various constituencies, some of which could be underfunded


## Data Description

**Transaction Table**

TransactionDT: Timedelta from a given reference datetime (not an actual timestamp)
TransactionAMT: Transaction payment amount in USD
ProductCD: Product code, the product for each transaction
card1 - card6: Payment card information, such as card type, card category, issue bank, country, etc.
addr: Address
dist: Distance
P_ and R_emaildomain: Purchaser and recipient email domain
C1-C14: Counting, such as how many addresses are found to be associated with the payment card, etc. The actual meaning is masked.
D1-D15: Timedelta, such as days between previous transaction, etc.
M1-M9: mMatch, such as names on card and address, etc.
Vxxx: Vesta engineered rich features, including ranking, counting, and other entity relations.

Categorical Features: ProductCD, card1 - card6, addr1, addr2, P_emaildomain, R_emaildomain, M1 - M9

**Identity Table** 

Variables in this table are identity information – network connection information (IP, ISP, Proxy, etc) and digital signature (UA/browser/os/version, etc) associated with transactions. They're collected by Vesta’s fraud protection system and digital security partners. (The field names are masked and pairwise dictionary will not be provided for privacy protection and contract agreement)

Categorical Features: DeviceType, DeviceInfo, id_12 - id_38

![Test Image 2](“/images/model-table.png”)

<img src="https://github.com/h-dzeba/GA_projects/blob/main/capstone/Images/model-table.png" alt="Alt text" title="Optional title">

## Conclusions

- As we can see in the table above, many of the models we ran have a similar ROC-AUC score in the range of 0.88-0.90. Of course, when it comes to issues as important at credit card fraud, every percentage point matters, and in a dataset as unbalanced as this one, where only 3.5% of rows are fraud, every little improvement is hard to come by.

- The first 3 models (0, 1, and 2) are run on a dataset without encoded categorical features and without filled-in null values. That was done because the model used was Light GBM which handles nulls and categorical columns internally better than if it had been undertaken by us

- the results of the model 2 (LGBM3-wght) are a fluke. Firstly, it test score is higher than its train score which doesn't make any sense. Secondly, we added weights to counter the unbalanced data which should result in higher recall and lower ROC-AUC, as focusing on positive (fraud) outcome uncovers more of them at the expense of overall test metrics. Instead the ROC-AUC went up, counter-intuitively. Normally we would reshuffle the train and test data to double-check such results, but since chronological data does not get shuffled, we couldn't do that.

- After encoding (models 3 and up) we could run different classifiers (RF, NN) in addition to re-running the LGBM. The results of LGBM are generally better after encoding (dummy and hash) than before. The conclusions we can draw from that are that: splitting key features with low cardinality increased the predictive power of our model, as expected. Moreover, we can conclude that hash encoding,  by 'compacting' many high-cardinality features into only 8 columns managed to preserve predictive power of the model. That is a laudable achievement for our hash-encoder as it gives us a way to model categorical data without exploding the size of our dataset.

- model 5 - LGBM with a 32-bit hash encoding (rather than previously used 8-bit one) did not result in any improvement, despite the additional 24 columns of hashing space. Hence, the model 3 is preferred as it's exactly the same as model 5, but it takes up 24 fewer columns

- model 7, the Keras Neural Network Classifier, while not ideally suited for unbalanced classification, managed to produce a model with the highest recall score of 0.676 - catching more than 2/3 of fraudulent transaction. Industry standard is 50%, so that's quite good. The downside is that the precision is quite low, so many legitimate transactions will get delayed or declined.


**WAY FORWARD**

- these models took an average of less than an hour to run on a laptop with only 4 cores. That is amazing when you consider we started out with a dataset with 500k rows and 400+ columns. It would be interesting to compare the results of our model number 3 to a model run on the entire dataset (which would end up having more than a 1000 columns once categorical variable are encoded). Would there be a noticeable jump in performance? By how much?

-  use behavioral analytics to identify bad actors across a range of online misconducts including counterfeit products, fake reviews, malware and illegal content. By casting a wider net, we can learn more about patterns common to fraud. This would result in a diversified new set of features for machines to train on.





