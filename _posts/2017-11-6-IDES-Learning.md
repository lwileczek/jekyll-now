# Knowledge Learned from IDES analysis:

## Goal: 

  1. Predict days spent in each stage of IDES process
  2. Determine how much time is added which each increase in condition. 
   
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

## Fun Facts:

  * Number of conditions and time spent in referral stage have an inverse 
  relationship.
  * Statistically significant variables: Army, VA rating, AF, Neurological, Endocrine.
  (referral stage.)
  * by itself, count of conditions is *not* proportional to stage time. 
  * It appears the more conditions one has and higher severity rating from
  the VA, the less likely the response variable is to be an outlier.

## Next Actions: 

  1. Perform regression on dataset once outliers are removed
  2. Perform Robust Regression on dataset with outliers. 
  3. Apply PCA for model selection, dimention reduction and regression.
  4. Add more variables and regress again. 
