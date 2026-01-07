## I used Google Restaurants dataset available on https://cseweb.ucsd.edu/~jmcauley/datasets.html#google_restaurants
## Description
*This is a mutli-modal dataset of restaurants from Google Local (Google Maps). Data includes images and reviews posted by users, as well as other metadata for each restaurant.*

I thought this was an interesting dataset to explore, because everyone likes food including me.


## Model Performance Comparison (Confusion Matrix and Key Metrics) ##
|                                         | TN   | FP   | FN   | TP   | Accuracy   | F1-Score (Pos)   | F1-Score (Neg)   |
|:----------------------------------------|:-----|:-----|:-----|:-----|:-----------|:-----------------|:-----------------|
| Naive Baseline (Avg Rating Only)        | 353  | 841  | 2018 | 7803 | 0.7404     | 0.8452           | 0.198            |
| TF-IDF (Base Text Only)                 | 305  | 889  | 106  | 9715 | 0.9097     | 0.9513           | 0.3801           |
| TF-IDF + Historical Reviews             | 7    | 1187 | 8    | 9813 | 0.8915     | 0.9426           | 0.0116           |
| TF-IDF + Avg Restaurant Rating (Hybrid) | 272  | 922  | 350  | 9471 | 0.8845     | 0.9371           | 0.2996           |
| TF-IDF + Improved Negative class        | 699  | 495  | 1578 | 8243 | 0.8118     | 0.8883           | 0.4028           |
| TF-IDF + Random Forest                  | 649  | 545  | 1314 | 8507 | 0.8312     | 0.9015           | 0.4111           |

As discussed before, the dataset contains unbalanced rating distribution. Negative ratings (under 4 stars) comprise only 11.5% of the training set. This means if we're not careful, we can have very good accuracy with predicting only the positive class. To account for this I included F1-Score metrics for both the positive and negative class to visualize model performance in both GOOD and BAD rating predictions. F1-Score formula:<br>

![image.png](attachment:2fdfd1aa-622b-447b-b778-98b6ea1c60a0.png) <br>

We set a pretty good baseline by predicting future customer sentiment for a particular restaurant by simply using the average rating of that restaurant. If the average rating was below 4 stars, we would predict the customer would leave a negative review in the future. And vice-versa. Surprisingly this was a pretty good guess with 74% accuracy! Intuaitively it makes sense, if a place has great food and service, then it would most likely stay that way. <br>

Using a simple Term Frequency-Inverse Document Frequency (TF-IDF) and Logistic regression provided impressive results of over 90% accuracy. However if we look closely 889 out of 1194 negative reviews were incorrectly classified as positive (False Positive). F1-Score(Neg) has improved compared to the baseline, however we try to improve it further.<br>

Surprisingly including historical reviews as a feature made the model always predict positive. Which if we think more about - it makes sense. Just because a customer left a positive review in the past, doesn't mean that they will never leave a negative one in the future. I decided to exclude this feature in the next iterations.<br>

Including avg. rating in the feature degraded the performance slightly, but I decided to leave it in the features since I felt like it's a decent predictor of customer sentiment. <br>

Next we included review length and unique word count to help the model further understand the sentiment in the review, at the same time we change logistic regression to 'balanced' which helps the model to weigh the negative class more. This resulted in a lot more False Negatives, however in relative terms, prediction of negative class improved 2x compared to baseline (from 353 to 699 True Negatives). <br>

To get positive predictions back to around 90% mark we tried Random Forest model which improved both F1-Scores. This is the final model that we felt could not get any better with the methods that we used. <br>

For further improvements - potentially Neural Networks and LLMs can be used.

## Conclusion
In summary due to class imbalance in the data, it was a challange to predict negative class effectively using standard ML methods. Best performing model for predicting negative reviews was Random Forest with 0.41 F1-Score and 83% overall accuracy. I learned that it is crucial to keep in mind data imbalances and biases even if evaluation numbers look really good on paper. Another key takeaway is that historical behavior of a customer doesn't always transalate to good predictions (in this case) as shown by model performance degradation after including historical reviews in the feature set.<br>
Author: Kostya Kravchenko - UCSD - Master of Data Science program
