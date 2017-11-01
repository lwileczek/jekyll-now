# Knowledge Learned from IDES analysis:

## Using OLS (ordinary least squares)

  1. average inrease is blah,
  2. data skewed

## Goal: 

  1. Predict days spent in each stage of IDES process
  2. Determine how much time is added which each increase in condition. 
   
## Chanllenges:

  1. Skewed data with many outlies in the response variables 
  2. most of the data is catagorical
  3. bad data entry. 
  4. Large number of columns with suspected multicoliniarity 
  5. Large percent of missing data.

## Solutions:

  1. Apply Lasso regression for variable selection.
  2. Use PCA to remove multicolinarity, identify key predictors, & reduce dim.
  3. Use M-Estimation to adjust for outliers in the response varible.

## Fun Facts:

  * Number of conditinos and time spent in referral stage have an inverse 
  relationship.
  * variables used in OLS: Army, VA rating, AF, Neurological, Endocrine.
  (referral stage.)
