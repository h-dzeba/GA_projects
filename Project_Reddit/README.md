# <center>           Reddit: A Window Into Ancient Worlds     </center>

## Background

For centuries, historians have done the bulk of their work in the field. Be it an archaeological excavation on a remote location or arduous sifting through old documents in city archives, historians have seldom made a revolutionary new discovery just by sitting in their offices and pondering the times past.

That has all changed over the past few decades as more and more research has been published online - from a detailed list of pottery found during an archaeological dig, to a groundbreaking new research articles shedding light on previously unknown aspects of life and times long ago, and even to those dusty old archival texts, they have all, slowly but surely, found their way on to the World Wide Web.

With such a vast array of historical information at their fingertips, historians are no longer able to rely on simple browser search techniques to find the relevant content. As a result, a top university has hired us to build a machine learning model which can perform that search for them, both for history, as well as, down the road, for other academic departments.

Why did we choose to start with history? There are several reason, but the main one is that...it's in the past. As such, the topics, discussions and words used are not changing as much as in other fields such as technology and medicine where disciplines seem to find an entirely knew paradigm from one year to the next. To be sure, progress in historical research still takes place, but instead of reinventing the entire field, it takes place at the margins. That is why it is perfectly suitable for a machine learning model - once a model learns the language used in a historical topic, that language will not change much from year to year, making the model efficient to use and simple to maintain for longer periods of time.


## Task

The first stage of teaching the machine to recognize topics is to train them to distinguish between only a pair of them. This is where this project comes in.

We have chosen to train and test the model on Reddit website, namely, its two subreddits dedicated to Ancient Greece and Ancient Rome. Reddit, as an online public forum, has a wide variety of participants - from university professors to teenagers, and as such, represents a microcosm of online experience. Another benefit is that the two ancient civilizations are far removed from current events, pop culture and vitriolic debates that pervade online spaces, and yet, because of their importance in the development of our civilization, they still manage to have active subreddits with close to 50k followers. Such a diverse, but relatively serious set of followers should produce less spam and more useful vocabulary for the machine to train on than most other subreddits.

In this first step of the project we will develop a model that can learn to distinguish between texts topics on ancient Rome and Greece based on 6000 posts taken from Reddit. 

Further on down the line, the model will have to be trained to distinguish between many different topics all at once, as well as to recognize when a topic does not belong to any of the academic fields, but that is beyond the scope of this notebook.

## Metrics and their names

For the purposes of the project, our model evaluation will mostly be based on total accuracy - basically, the percentage of correct predictions. Since there is no real difference between mistaking topic number one for number two vs vice versa, there is no real point to have classification metrics of 1 for positive outcome and 0 for negative. Granted, we will still use 1 and 0, but only in purely nominal terms, with 1 representing greece and 0 rome.

Concepts of false positive and false negative are likewise not used in this study, and also, precision will not be split in sensitivity (for false positives) and specificity (for negatives). As neither greece nor rome are inherently positive or negative, such concepts have no place in our project (unless a build-in function automatically displays them).

As far as right/wrong predictions are concerned we will use adapted precision metrics we will call "rome recall" and "greece recall". As the name implies, they describe to total rome( greece) outcomes correctly predicted out of the entire number of rome(greece) outcomes in the dataset.

## Models developed

| No |          MODEL (vector)         | train<br>accuracy | test<br>accuracy |                                                                         parameters |                                     other features |                                                                                                  comments                                                                                                  |
|----|:-------------------------------:|------------------:|------------------|-----------------------------------------------------------------------------------:|---------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| 1  |    Naive Bayes<br>(countvec)    |              0.88 | 0.81             |                                             max_feat:4000<br>min_df:3<br>alpha:0.5 |                    nothing extra<br>- simple model |                                                                        first model after baseline, already<br>shown good improvement                                                                       |
| 2  |    Naive Bayes<br>(countvec)    |              0.90 | 0.84             |                                             max_feat:2500<br>min_df:4<br>alpha:0.1 |                       only posts <br>with 8+ words |                                                after testing the wrong predictions<br>from model 1, only longer posts were<br>taken. immediate improvement.                                                |
| 3  |      Naive Bayes<br>(tfidf)     |              0.91 | 0.81             |                                             max_feat:4000<br>min_df:3<br>alpha:0.5 |                                               none |                                                                         tfidf vectorization didn't bring<br>about much improvement                                                                         |
| 4  |      Naive Bayes<br>(tfidf)     |              0.93 | 0.85             |                                             max_feat:4000<br>min_df:3<br>alpha:0.5 |                       only posts <br>with 8+ words |                                                             - best score so far<br>- minimal tuning required, so very<br>easy and quick to run                                                             |
| 5  |    Naive Bayes<br>(countvec)    |              0.87 | 0.80             |                                             max_feat:4000<br>min_df:3<br>alpha:0.5 |                                    extra stopwords |                                                          - similar results to model 1 but with<br>more stopwords, therefore considered<br>a succes                                                         |
| <span style="color:red">6</span> |  <span style="color:red">    Naive Bayes<br>(tfidf)   </span>  |    <span style="color:red">          0.93</span> | <span style="color:red">0.84</span>             |                                             <span style="color:red">max_feat:4000<br>min_df:3<br>alpha:0.5 |<span style="color:red"> only post with<br>8+ words, and<br>extra stopwords</span> |        <span style="color:red">                                                       - simple model with best scores<br>- production model<br>- simple to tune</span>                                                              |
| 7  |   Random Forest<br>(countvec)   |              0.81 | 0.78             |  max_feat:4000<br>criterion:entropy<br>max_depth:5<br>min_leaf:2<br>estimators:300 |                                    extra stopwords |                                                         -random forest did not show as high<br>accuracy as NB<br>- predicted Greece 60% of the time                                                        |
| 8  |     Random Forest<br>(tfidf)    |              0.81 | 0.76             | max_feat:4000<br>criterion:entropy<br>max_depth:12<br>min_leaf:1<br>estimators:300 | only post with<br>8+ words, and<br>extra stopwords | -hard to tune. started with params for<br>model 7, but it wasn't even close<br>- accuracy was initially awful, but <br>with very high max_depth got close to<br>model 7<br>-predicted Rome 67% of the time |
| 9  | 2nd deg Poly SVM,<br>(countvec) |              0.94 | 0.80             |                                                    max_feat:7500<br>C:1<br>coef0:2 |                                    extra stopwords |                   -big difference between train and test<br>- indicative of overfitting<br>- no amount of parameter tuning could <br>reduce overfitting<br>-predicted greece 57% outcomes                  |
| 10 |   2nd deg Poly SVM,<br>(tfidf)  |              0.99 | 0.83             |                                                                              0.901 | only post with<br>8+ words, and<br>extra stopwords |                                            -even worse overfitting than model 9<br>- adding greek stopwords made the model<br>balanced in its prediction (50:50)                                           |


    
    
    
    
    
    
## Conclusions
- our production model, among others, proves that distinguishing among historical topics is possible for a machine, and that reddit can be harnessed for that purpose. we got high accuracy scores despite the fact that, as described in the beginning of the project, around 60% of top words in both subreddits are the same
- rather than using reddit, it might be possible to train the model on proper nouns only (countries, kings, named war) from the relevant historical period. we noticed that some 75% of the top 30 predictors are proper nouns
- while the topics were simililar, we were lucky that the political orientation of ancient rome and greece was different - one was an empire and the other was not. words 'empire' and 'emperor' score very high on our predictor strenght lists. in the future, when comparing two empires (or two kindomns), the model might face a more uphill battle
- just because a word is a strong predictor, does not mean however that we should not be careful in how we treat it. We saw the word 'emperor', the most important word for predicting the roman subreddit, was also one of the top words among misclassified posts. every time that word ended up in a greek reddit, the post automatically got wrongly classified as roman
- choice of models and stopwords is the key to success. models can be left to data scientists, but the choice of stopwords can be tricky and we should seek input from the experts in the academic field
- in a long run, having a model that is simple, reliable, interpretable and low-maintenance is paramount to ensure University's commitment to the project. That is why we chose NB model from section 2.7. It required very little paramenter tuning (changing the parameters didn't affect the scores much), it excluded many posts and words making it less susceptible to spam, it had great accuracy scores and it wasn't biased towards choosing either subreddit.
- with more resources, building a language library would provide most benefits when it comes to international history. Basing decision on foreign alpahbets rather than excluding them through stopwords would definitely benefit the scores
- as University's goal is not to make money, but rather, to promote academic research, our projects wasn't as focused on uncovering the inclinations and propensities of the people who write the posts in order to target them with adds. Still, while we don't have to worry about who writes the posts that our model reads, we do have to worry about academics who will use our model to get their weekly or monthly rundown of what was published in their field. Testing the wrong predictions and lists of important words regularly can therefore serve as an early warning system of intrusion of spam into our dataset, and thus ensure quality product for professors, researches and students for years to come    

## Some issue with our model
- ancient rome/greece is the first model developed as part of our effort to develop a machine-learning product that can search the internet for content from various academic disciplines. future models to distinguish different pairs of topics will be much faster and easier to implement
- there will however be some stumbling blocks. one that sticks out in particular is the issue of stopwords. we relied a lot on them to train the model properly and it took us a few iterations until we settled on the set of stopwords that works well. Each new topic would require a completely new set of stopwords added to the default ones.
- as individual models are concerned, we chose a particular Naive Bayes one for its simplicity, accuracy and interpretability. SVM shows a lot of promise by being able to predict training data with almost 100% accuracy. if we can invest more time and training into it, maybe it could increase its test scores as well, and become our go-to model 
- another advantage of our chosen model is that it didn't use any greek words to aid it in prediction. wowever, if more foreign alphabets get scraped into our future data (chinese, arabic), removing them might not be the proper course of action. Using only latin letters will prove to be a huge drawback, especially in the history deparment that deals in many different cultures, letters and languages, some of them extinct
- one of the unusual aspects of our model is how random forest and SVM vacillated between overpredicting greece to overpredicting rome based upon the stopwords given,  while Naive Bayes did not. We would love to subject this oddity to greater scrutiny when time allows.
    
## The Way Forward

As stated in the background section of this project, we envision this entire project to be just the first step in unfettered internet search for any material that relates to any of the University's academic departments. No person can keep up with the amount of materials that get published daily, and intelligent machines doing the search work for us is the only way to go. The next steps will be, in order:
- get feedback from the University on the current model, make improvements based on it.
- expand the model to be able to recognize more historical topics, and not just two, but 3, 4,...n at a time
- configure the model to be able to categorize text as 'other' when it does not meet the threshold required to be put in any of the proposed categories
- once historical topics are exhausted, repeat the process in other academic fields

