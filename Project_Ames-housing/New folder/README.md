# Ames, Iowa housing price model, based on sale prices from 2007-2011 as well as the detailed information on all the charateristics of the houses involved.



## Problem Statement:
*10 years after the biggest housing and financial crisis of our generation, real-estate is again starting to produce noticable yearly price gains. Some Tier 1 cities such as New York and San Francisco have already seen their housing prices break new highs and become unaffordable for most locals. Even some Tier 2 cities have joined the party fueled by low interest rates and become overbought according to conventional real-estate metrics.   
A real- estate investment fund, known for its sound investing philosophy based on market fundamentals, has therefore hired us to develop a model that can accurately price houses in Tier 3 cities with which they were up-to-now unfamiliar with. According the them, the Tier 3 cities such as Ames, IA still provide good rental yields and make sense as investments from fundamental investing point of view. 
Another key deliverable in addition to a simple, interpretable model that prices houses in Ames would be a set of quantifiable factors determining those prices. As the goal of the fund is to deliver return to its investors, they would need to know which factors hold the most sway over the final determination of a house's market price.*


## Data 
### Source data used:
 - 2019 All data used in this project is from http://jse.amstat.org/v19n3/decock/DataDocumentation.txt
 
 ### Data cleaning/transformation:
 - In addition to cleaning, dropping and merging the data, one new categorical ranked column was created for neigborhood desirability. It is mostly based on https://bestneighborhood.org/best-neighborhoods-ames-ia/, together with some local realtor data.
 
 ### Data dictionary:
 ####                         CA2019q DataFrame
|Feature|Type|Range|Description|
|---------|---|---|---|
|lotarea|float|0.4 - 7.2|**log** of the lot size in sf| 
|overallqual|integer| 1- 10|quality of finishing and materials|
|totalbsmtsf|float|0 - 3200|basement area in sf| 
|grlivarea|float|5.8 - 8.2|living area sf| 
|kitchenqual|integer|0 - 3|kitchen quality| 
|paveddrive|dummy|0, 1|paved driveway| 
|zone_FV|dummy|0, 1|floating village zone| 
|zone_RL|dummy|0, 1|low density zone|
|zone_RM|dummy|0, 1|medium density zone|
|lotshap_irr|dummy|0, 1|irregular shape of the lot|
|near_busy_st|dummy|0, 1|adjecent to arterial or feeder st|m
|non_1fam|dummy|0, 1|building type not single family home|
|2plus_fl|dummy|0, 1|2 or more floors|
|hip_roof|dummy|0, 1|hip roof design|
|ext_ord|integer|0 - 2|type of exterior covering by quality |
|has_veneer|dummy|0, 1|veneer on the facade|
|cncrt_found|dummy|0, 1|concrete foundation| 
|bsmt_ord|float|0 - 5|basement quality, exposure and type - cumulative| 
|heatingqc_dum|dummy|0, 1|heating|
|cent_air_dum|dummy|0, 1|central air-conditioning| 
|garage_ord|float|0 - 3|combination column of garage size and quality, re-scaled| 
|firplace_ord|float|0 - 2|combination column of garage size and quality, re-scaled| 
|lotfrontage_imp|float|21 - 313|feet of street connected to property| 
|remodel_age|integer|1 - 61|years since last remodel| 
|tot_bath|Cat|Am Community Survey|2018 median income rank| 
|deckporcharea|float|0 - 1423|feet of street connected to property| 
|nhood_1|dummy|0, 1|low desirability neighborhood| 
|nhood_3|dummy|0, 1|high desirability neighborhood|
|saleprice|float|0.4 - 13.3|log of transacted price|


## Analysis
 - After cleaning, transforming and recombining all the data into a new dataset, exploratory analysis and visualization was used to shed light on which features should be fed into linear regression model for the most precise predictions. 4 types of sub-categories of linear models were used: a simple regression, regression with cross-validation, Ridge Regression and Lasso Regression. Each of these models was used multiple times with number and combination of features. A function was developed that, given the nunmber of features, selects the optimal combination. R-squared and RMSE were both used to assess the models
 
 ## Outcomes and recommendations:
 **Outcomes**

- The first goal of this project is to create a pricing model for homes in Ames, IA that has a combination of accuracy and interpretibility. As far as the accuracy is concerned, we entered a few of our models into a Kaggle competition to see how they stack up versus the competition. Our top model, a 20-feature one with Ridge Regularization scored in the top 15 out of a myriad of submission, with a test rmse error of only 20,090 dollars.

- another successful outcome was that all of our models performed well, with an r2 of around 0.9, which shows that the good kaggle score was not due to the luck of stumbling into a right model with the right data, but rather, it was due to careful data selection and transformation

- as far as the model that will be presented to the stakeholders, we chose a somewhat simpler model with comparable rmse (better on test, worse on kaggle data) and with fewer transformation but with much more interpretibility and intuitive sense, which can easily produce results that can be used in day-to-day operations

**Recommendations:**

- the last model in this notebook with 28 features and charts of how each of these features affect the house price has the best of both worlds and can be used as the go-to model. With r2 of close to 0.9, rmse of 25k and easily made sense of due to few transformations, it is dependable, simple, easy to modify and ready for everyday use

- another reason why we chose this model despite the fact it doesn't have the lowest kaggle rmse  is that a pure data model of housing prices, devoid of human judgement, will never be perfect. No model on the Kaggle leaderboard was able to predict house prices to an average error of less than 15k dollars. This implies that no matter how complex and cutting-edge the model is, assessing the price will always be somewhat of an art. And since that is the case and a human input will be needed at some stage anyway, we were willing to sacrifice (potentially) a bit of accuracy for the sake of interpretibility.

- still, when the Fund finally starts to invest, the recommendation would be to switch models and use our most accurate one, as maximum accuracy is needed when deciding which house to invest in or which ones are underpriced. 

- once the funds are deployed and the homes are bought, it'll be time to look at which house features can be improved/upgraded to enhance the property's value:

a) while nothing can be done about the zoning, improvements in foundation of the house and central air can increase property value substantially (10k and 12k respectively)

b) it makes sense to build-out the house and cover up as much as the lot as possible. While it might not be aesthetically pleasing to some, the market values living area much more than lotarea (even after adjusting that lotarea is many times bigger than the living area - 44 vs 1 dollar per sf2)

c) investing into overall condition of the house and the garage can pay high dividents. An imporvement of 1 point on the condition scale can be worth more than 10000 dollars. Ofcourse, the cost must be assessed as well, but with such a high premium for improvement, doing it should be well worth it.