# Project 4: Predict and Manage West Nile Virus


## Introduction

**West Nile Virus still plagues citizens in Chicago, Illinois, today.**

To make Chicago homes safer for all, our team of Data Scientists have 

- analyzed available data to predict likelihood of incidence of the disease, and 

- will present a preliminary cost-benefit analysis to inform health authorities on Zenivex spray-based interventions. (see slides)
---------

#### Data
The Authorities have provided us with 4 datasets:
1. [Weather](./datasets/weather.csv),
2. [Spray](./datasets/spray.csv),
3. [Training](./datasets/train.csv),
4. [Test](./datasets/test.csv), and
5. Mapping resources.
---
#### Data Dictionary
We have processed this data into the following fields:

|Feature|Type|Dataset|Description|
|---|---|---|---|
|date|<i>datetime</i>|final_train|Date of observation| 
|species|<i>object</i>|final_train|Mosquito species| 
|latitude|<i>float</i>|final_train|Latitude of observation| 
|longitude|<i>float</i>|final_train|Longitude of observation| 
|nummosquitos|<i>int</i>|final_train|Num of mosquitoes observed| 
|wnvpresent|<i>int</i>|final_train|Presence of west nile virus where 1 is present and 0 is not present| 
|longitude|<i>float</i>|final_train|Longitude of observation| 
|year|<i>int</i>|final_train|Year of observation|
|weekofyear|<i>int</i>|final_train|Week of the year|
|dum_species|<i>int</i>|final_train|Species where west nile virus might be present|
|tavg|<i>float</i>|final_train|Average temperature (celsius) of each day of the observation|
|preciptotal|<i>float</i>|final_train|Total precipitation recorded (inches) on each day of the observation|
|stnpressure|<i>float</i>|final_train|Average station pressure (inches of HG)|
|sealevel|<i>float</i>|final_train|Average sea level pressure (inches of HG)|
|resultspeed|<i>float</i>|final_train|Resultant wind speed (miles per hour)|
|resultdir|<i>float</i>|final_train|Resultant wind direction (To tens of whole degrees)|
|resultspeed|<i>float</i>|final_train|Resultant wind speed (miles per hour)|
|avgspeed|<i>float</i>|final_train|Average speed (miles per hour)|
|humidity|<i>float</i>|final_train|Relative humidity, calculated using tavg in celsius and dewpoint|
|daylength|<i>float</i>|final_train|Length of the day, calculated using sunrise and sunset|
|tavg_lag14|<i>float</i>|final_train|Average temperature (celsius), with 14 lag days|
|humidity_lag14|<i>float</i>|final_train|Relative humidity, with 14 lag days|
|preciptotal_lag14|<i>float</i>|final_train|Total precipitation recorded (inches), with 14 lag days|
|resultspeed_lag14|<i>float</i>|final_train|Resultant wind speed (miles per hour), with 14 lag days|

---

## Methodology

1. Examine data
1. Clean data.
3. Visualize data using plots and maps.
4. Iteratively select features, engineer new features, and model data.
5. Finalize model for evaluation.

**Metrics**: We take the AUROC score (also the Kaggle default) as the primary metric, and also examine recall.

- Recall: Avoid false negatives (save the people priority). Use our own test set from train-test-split
- AUROC (a.k.a. ROC-AUC) combines precision and recall into one metric by calculating the harmonic mean between those two. Use the score obtained from Kaggle submission as Kaggle's test set has over 100k rows (5 times more than our train set), and is therefore more reliable.

**Models**

1. Baseline
Predict 0 for every row in the test Kaggle set. It gets us accuracy of 0.95, ROC-AUC of 0.50 and recall of 0. This is what our model has to beat.

2. Modelling with base features.
We call it 'reg' models in the notebook. It consists of applying simple models to engineered data.

3. Polynomial features and PCA
Expend the number of columns by multiplying them, then used PCA to reduce them to only essential ones (98%+ explained variance)

4. Add SMOTE and SMOTETOMEK.
Adresses the class imbalance and tries to improve recall scores which, without SMOTEing, are abysmally low.


Run various models in each of the 4 categories, use GridSearchCV for hyper-parameter tuning. 
Choose the production model based on the metrics mentioned above: ROC-AUC we get from submitting the model to Kaggle, and recall from our own test set we obtained via train-test-split.

### Conclusion

**There are many reasons to suggest not spending resources on spraying:**

1. Our charts made during EDA section show no significant decrease in either the mosquito number nor in the incidence of WnV after previous spraying;

2. Under reasonable assumptions, we can only afford to spray 15% of the city area for 8 weeks during the peak mosquito season, which wouldn't make much of a difference, as mosquitoes from other parts of the city could easily migrate to replace them.

**Still, we believe and recommend that limited spraying should take place.**

1. Even assuming only an area the size of 15% of the Chicago downtown can be sprayed in any given week, we could use our model to maximize the efficiency of such a limited spray, by focusing on the areas in our model that give rise to highest 'WNV present' odds. (from model.predic_proba)

2. Scientifically speaking, the spray does kill off mosquitoes and their larvae, even if our charts/data do not show it. We choose to believe the science and use spraying as a mosquito-reduction technique. We will however use our findings of spray's lack of success, by deciding not spend too much money on it.

3. It is important for a city government to take actions that show that it cares about its people; standing idly by while the virus affects its people is not an option.

4. Lastly, even though spraying does not have substantial effects now, that does not mean that it will remain so in the future, especially if West Nile virus were to start spreading at faster rates. That is why it is important to have a well-oiled and functioning spraying program in place now, which can then easily be ramped up in the future if need were suddenly to arise.

### Beyond the cost-benefit: What's next?

The only way to substantially improve our model and make use of City's limited spraying finds, is to extend the model in such a way so that it is able to pinpoint the exact micro-location of where the WNV mosquitoes will be present. This would include more advanced use of GPS data, as well as teaming up with weather experts and entomologists to model the ways in which mosquitoes move around the city based on atmospheric data and their biological needs. 

---

### Organization

**Folder Organization**
```
|__ code
|   |__ 01_cleaning_eda.ipynb
|   |__ 02_maps.ipynb  
|   |__ 03_models.ipynb
|__ datasets
|   |__ train.csv
|   |__ test.csv   
|   |__ weather.csv
|   |__ spray.csv
|   |__ mapdata_copyright_openstreetmap_contributors.txt
|__ images
|   |__ gif_frames   # image data for GIF 1
|   |__ gif_frames2  # image data for GIF 2
|__ P04_group_presentation.pdf
|__ README.md
```

**Across the Project**
1. [Cleaning, Exploratory Visualizations, and Export](./code/01_cleaning_eda.ipynb)
2. [Visualizations via maps](./code/02_maps.ipynb)
3. [Models and Conclusions](./code/03_models.ipynb)