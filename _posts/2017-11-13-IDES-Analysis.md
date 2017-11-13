# Knowledge Learned from IDES analysis:

## Goal: 

  1. Predict days spent in each stage of IDES process
  2. Create input for the simulation model to improve accuracy.  
   
## Challenges:

  1. Skewed data with many outlies in the response variables 
  2. most of the data is categorical
  3. bad data entry. 
  4. Large number of columns with suspected multicollinearity 
  5. Large percent of missing data.

## Solutions:

  1. Apply Lasso regression for variable selection. [Lasso Wiki](https://en.wikipedia.org/wiki/Lasso_(statistics))
  2. Use PCA to remove multicollinearity, identify key predictors, & reduce dim.[PCA Wiki]( https://en.wikipedia.org/wiki/Principal_component_analysis) 
  3. Use M-Estimation to adjust for outliers in the response variable.  [Robust Regression Wiki](https://en.wikipedia.org/wiki/Robust_regression)
  4. Peforming a log transformation to the model. [Data Transformations](https://en.wikipedia.org/wiki/Data_transformation_(statistics))

## Background and Assumptions for Current Analysis

  1. Variables used: 
   * Number of contions for each case split out on the systems level. E.g.
     * case.id:0001 - THE SKIN:2, Neurological:3, Cardiovascular: 0, Digestive: 1, ...
   * Service (e.g. Army, Navy, Air Force, or USMC)
   * VA rating per condition (using a sum of all ratings for all conditions per case).
  2. Response variables are skewed right.
  3. There are errors from poor data entry. 

## Data Insights:

  * Number of conditions and time spent in referral stage have an inverse 
  relationship.
  * Statistically significant variables: Army, VA rating, AF, Neurological, Endocrine.
  (referral stage.)
  * by itself, count of conditions is *not* proportional to stage time. 
  * It appears the more conditions one has and higher severity rating from
  the VA, the less likely the response variable is to be an outlier.

## Why Solutions where chosen

  1. Lasso regression is used to pick which pridictors variables should be used in the model. Lasso is used over a stepwise selection method because it applies the same penalty equally to all variables at once. This avoids giving a single variable a bias based on when it is added or removed from a model. 
  2. PCA produces predictor columns that are completely orthogonal or unrelated to eachother so there is no chance of one predictor explaining the same thing as another. It also uses a *linear* combination of all the original columns so no information is lost in the process. 
  3. M-estomation is used to create a linear model that is more resistant to outliers in the response. Generaly more expensive to compute and errors are interpreted differently than usual. 
  4. Taking the log of skewed data can normalize it to become more linear. However, this changes the interpritation of the model. 
