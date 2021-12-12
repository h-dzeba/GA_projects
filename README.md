# 2019 SAT/ACT report analysing the relationship between income and test scores across all 50 US states, as well as across all the counties in the State of California



## Problem Statement:
*2019 College Admissions Cheating Scandal brought into focus the lengths that high-income parents will go to in order to game the system of standardized tests dominated by ACT and SAT. While not as egregious as outright cheating, economic bias has been something that both tests have often been accused of, and as a result, some universities are eliminating or making optional their requirement to have one of the tests submitted as part of admission process.
In this project we are hired by ACT, Inc to investigate some reports that ACT tests exhibit more economic bias than its rival SAT, and additionally, to isolate the top performers among low-income districts in order to learn from them. By doing that, we may be able to propose ways to eliminate or at least reduce the economic bias going forward.*


## Data 
### Source data used:
 - 2019 SAT and ACT scores by state
 - 2019 SAT and ACT California scores by county
 - 2018 Household income, USA by state, California by county (the income data was purposely chosen from the year before the test, as it affected the students while they were still in school)
 
 ### Data cleaning/transformation:
 - the source data was recombined into two files/dataframes: CA2019q and USA2019q. In addition to cleaning, dropping and merging the data, one new categorical column was created in each file. The column assigns the income rank quantile to each county/state.
 
 ### Data dictionary:
 ####                         CA2019q DataFrame
|Feature|Type|Dataset|Description|
|---------|---|---|---|
|county_name|object|SAT2019CA|county name| 
|enroll|float|SAT2019CA|SAT student enrolment| 
|testtkr_sat|float|SAT2019CA|SAT test-taker number| 
|eng_bmark_sat|float|SAT2019CA|SAT eng meeting benchmark (%)| 
|math_bmark_sat|float|SAT2019CA|SAT math meeting benchmark (%)| 
|ttl_bmark_sat|float|SAT2019CA|SAT total meeting benchmark (%)| 
|testtkr_act|float|ACT2019CA|ACT test-taker number|
|read_act|float|ACT2019CA|ACT reading score|
|eng_act|float|ACT2019CA|ACT english score|
|math_act|float|ACT2019CA|ACT  score|math
|sci_act|float|ACT2019CA|ACT science score|
|ttl_bmark_act|float|ACT2019CA|ACT total meeting benchmark (%)|
|ttl_act|float|ACT2019CA|ACT total score|
|hh_income|float|US Census|median household income (county)|
|income_third|Cat|US Census|hhold income rank (high,middle,low)|

####                      USA2019q DataFrame
|Feature|Type|Dataset|Description|
|---------|---|---|---|
|state|object|2019_SAT|state name| 
|participation_sat|float|2019_SAT|SAT participation rate| 
|english_sat|float|2019_SAT|avg english SAT score|
|math_sat|float|2019_SAT|avg math SAT score| 
|total_sat|float|2019_SAT|avg total SAT score| 
|participation_act|float|2019_SAT|ACT participation rate| 
|total_act|float|2019_SAT|avg total ACT score| 
|median_income|float|Am Community Survey|2018 median HH income| 
|med_income_q|Cat|Am Community Survey|2018 median income rank| 

## Analysis
 - After cleaning and recombining all the data into two new datasets, exploratory analysis and visualization was used to shed light on the exact impact that incomes have on the test scores. US national data contained many idiosyncrasies that challenged our presumptions. The California data on the other hand, with its uniform state policies and without the patchwork of regulations that affects the 50 states as a whole, was used as a "control" group against which the challenging questions emanating from the national data can be cross-checked.
 
 ## Conclusions and recommendations:
 **Conclusions:**

- SAT is by no means better than ACT in terms of avoiding economic bias. What looks like an advantage for SAT in that respect, was shown by this analysis to be a product of different state policies towards mandatory testing, rather than some inherent lack of economic bias in SATs vs ACTs.

- unfortunately that does not mean that there is no economic bias in ACT and SAT. Rather, the conclusion is that the bias is still here, and still strong. This analysis supports that claim by focusing on one individual state - California -and delving deep into its county-by-county data, which show a clear pattern of economic bias.

- in national SAT data, the math scores difference between states from different sides of the income spectrum is more pronounced then the english. This suggests that putting more resources into math in lower-income states would be the more cost-effective way to level the playing field.

**Recommendations:**
- rather than throwing more money at the problem of underperforming schools from poorer districts, we envision a way forward whereby outperforming schools from low-income areas are identified on a regular basis through data analysis. Following that, we endeavor to learn the programs they had put in place that helped them achieve such success. Finally, we proceed to advise and inform schools around the nation to implement those same policies which would reduce the economic bias in testing and "take some heat off" ACT Inc, politically speaking. A couple of examples of school and state policies from 2019 data that bore academic fruits are:

    - in Shasta County, the top performing low-income county in California, Central Valley High School has CollegeVine program in place which helps students transition from high-school to college and  teach its students the "college way of studying". It combines the best of both worlds - a safe and familiar high-school environment together with college preparation and coursework. Aside from sharpening their academic skills, the students also attain confidence, maturity and a realistic outlook. These are the intangible skills that can help a young person take their studies more seriously, thereby helping them score higher on the College Board exams without the benefit of money or extra tuition. More here:   https://www.redding.com/story/news/2021/11/10/central-valley-high-school-dual-enrollment-college-credit-university-courses/6358940001/
                                     
    
-     
    - similar to throwing more money at the problem, putting more academic pressure on students is unlikely to lead to any long-term improvement in test scores, as the students are under enough pressure already. That is why in 2018 the State of Indiana adopted a new program developed by University of Chicago researchers called "5 Essentials" to improve their schools. The program grades schools based on non-academic criteria such as leadership, collaboration and family support and the program has proved both successful and popular when piloted in Chicago a few years ago. Indiana performed quite well in 2019 for a state with "low" income rank. Follow up research on this in a few years will be needed. More here:
    https://in.chalkbeat.org/2018/9/4/21105695/indiana-officials-didn-t-have-to-go-far-to-find-a-new-model-for-improving-schools
 
