# Predict the Presence of West-Nile Virus Mosquitos

### Executive Summary:
#### The Opening
   This collection of notebooks details our process in submitting to the expired kaggle created by the Chicago Department of Health (found [here](https://www.kaggle.com/c/predict-west-nile-virus)).  **Note that the logistic regression model is discussed first and contains most of the discussion**, though both notebooks are fairly self-contained aside from the small discussion of preprocessing the data.
   
#### The Need   
   The goal is to create a model that predicts the presence of West Nile Virus in traps located throughout Chicago for the even years between 2008 and 2014, using the odd years 2007 and 2013 as training data.  As with the competition, the metric of interest will be the ROC AUC.  A model that can catch a large proportion of actual cases of West Nile can assist in predicting locations for preemptive spraying or even implementing larvacide solutions.
   
   These notebooks serve primarily as a sketch in the modelling process; ultimately, we are oncerned with the problem of overfitting to data, since we have the opportunity to show our model all of the odd years, though we must finally test on years which the model has never seen.  Because of the format of this competition, the hope is that our model will detect features independent of yearly variation; however, it was the full intention of this project to seed the idea in our models that there is information to be gained by looking at the year.  In this naive train of thought, we circumvent the idea that we should be terribly worried about the time dependence, since our model takes it into account by definition (or at least this is the argument of the logistic regression, which attempts to express the probability of finding West Nile as some function $f = f(month, year, ...)).  Obviously, this could only capture variations on those given orders of time, and other methods are attempted in fitting the decision tree based models.
   
   In a more practical setting, more consideration should be given to when and where the model predicts West Nile given some probability threshold, correctly and incorrectly.  This is a small extension, and will be completed in the near future.
   
   
## Contents

| Notebook | Description |
| --- | --- |
| 01 | [Exploratory Data Analysis](./Project4/Final/EDA/Project-4-EDA.ipynb)|
| 02A | [Modeling and Submission (Logistic Regression)](./Project4/Final/Modeling/LogReg.ipynb)|
| 02B | [Modeling (Decision Tree Based Models)](./Project4/Final/Modeling/CARTs.ipynb)|



# Summary of Methodology and Results

#### Cleaning and Exploration of Data

   Light cleaning was necessary for the weather data.  When values were missing from either weather station, the value was taken from the other station.  While this decision defeats the purpose of differentiating between the weather stations, the hope was that this might account for outlier days with abnormal weather, which would be ignored if we simply tried to fill values forward, for instance.  There were only a few of these missing values to begin with, so the accuracy of our model could not have changed by much, no matter what our decision here.

#### Pre-processing of Data in advance of Modeling

   Each row of the training data was matched to the weather data nearer the local weather station, in an attempt to be slightly more accurate than simply taking the weather for station 1 (nearer the airport).  Rolling averages were found to hurt scores in general; this should be explored further, but at this time, any rolling measurements over the weather have been avoided.  Data is scaled according to column means and variances in the logistic regression model for regularization.

#### Models Fit and Compared

| Model | ROC AUC(internal train) |ROC AUC(internal test) | ROC AUC(kaggle test) |
| --- | --- | --- |
| Random Forests | 0.81 | 0.99 | 0.75 |
| Extra Trees | 0.78 | 0.80 | 0.74 |
| Gradient Boosting(best score from CV) | 0.99 | 0.71 | 0.72 |
| Logistic Regression | 0.82 | 0.81 | 0.81 |

Year feature usually exhibits no feature importance in trees, or carries a small weight in logistic regression.

#### Conclusion

   In the end, while none of the models were unable to definitively pick up on large scale trends in West Nile Presence on the order of years, the models were able to find other prominent features.  How well these models do is a bit qualitative at the moment - we can only cite whether scores agree between an internal holdout set and the kaggle test years, and how well these do relative to the leaderboard.  Whether the logistic regression could make a difference in decision making cannot be said without seriously considering the options available - it certainly would not help if the solution was to make minnow ponds across Chicago, since we would not be interested in the time of year nor weather conditions to make this decision, but perhaps the yearly movement of mosquitos, the very feature we cannot hope to predict with this data for every other year.  We have made some headway into making a competitive model, though it is not entirely ruled out that the score being the result of the given shifts is the result of a massive coincidence.  We would like to acknowledge the research efforts of our friends at [vanessa_carlton](https://git.generalassemb.ly/dstrodtman/project-4/tree/master/vanessa_carlton)(if you can see this), to whom we refer you for citation of information related to the shifted weather data.