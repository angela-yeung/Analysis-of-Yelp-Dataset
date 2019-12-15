# Analysis-of-Yelp-Dataset

Sentiment Analysis

In order to understand the popularity and traction of Mexican breakfast and brunch restaurants and what our business should avoid doing, a sentiment analysis was performed over user reviews of all Mexican breakfast and brunch restaurants. The purpose of this analysis was to reveal the most important factors that lead Yelp users to give a high, or conversely low star rating to a particular restaurant. A total of 192 businesses with unique business IDs, totalling to 45,053 reviews were used to conduct the sentiment analysis. 


1. Data Pre-processing 

A supervised machine learning based approach was used to conduct this sentiment analysis, as opposed to taking a lexical-based approach. Therefore, prior to training a model, labels must be assigned to each instance (i.e. review):
	
    - Reviews with a star rating of 4 or more were assigned ‘positive’;
    - Reviews with a star rating equal to 3 were assigned ‘neutral’;
    - Reviews with a star rating less than 3 were assigned ‘negative’;

Reviews assigned a ‘neutral’ label were excluded from the dataset in order for the machine learning model to classify terms as either having positive or negative polarity. After excluding ‘neutral’ reviews, 79.24% of the remaining reviews were labelled as ‘positive’. Thus, our baseline accuracy was 79.24%. 

Case-folding, stripping of punctuation, tokenisation and stopping was performed on the text in each review. Next, the term frequency - inverse document frequency (TF-IDF) was calculated and fed into the machine learning model as features. In essence, the idea behind the TF-IDF approach is that words that occur more in a particular review, but don’t appear often across all reviews contribute more towards classification.  

The term frequency measures how frequently a term occurs in a document. In order to normalise for reviews with differing word counts, the term frequency was divided by the document length: 

    TF(t) = (Number of times term t appears in a document) / (Total number of terms in the document).

The inverse document frequency is a measure of how important a term is. While calculating TF, all terms were considered equally important. However, terms that were too common, such as “the”, “is”, “this”, usually have little importance to the sentiment of the review. Therefore, terms that occur in a large number of documents were weighed down: 

    IDF(t) = log_e(Total number of documents / Number of documents with term t in it).
    
The TF-IDF feature matrix with each term as a column and each review as a row was calculated based only on the 2,500 most frequently occurring words due to computational resource limitations. Similarly, the algorithm forced the TF-IDF matrix to only include words that occur in at least 7 reviews. Words that occur less frequently are not very useful for classification. Furthermore, the TF-IDF matrix was constructed only with words that occur in a maximum of 80% of the documents. Words that occur in all documents are too common and are not very useful for classification. 

Once the TF-IDF has been calculated, the data was divided into training and test sets using an 80:20 split. The training set was used to train the algorithm while the test set was used to evaluate the performance of the machine learning model. 

2. Fitting the Model

A linear support vector machine (SVM) classifier was used to learn from the training set.
 
3. Model Performance Evaluation 

The SVM algorithm achieved an accuracy of 95.24% on the test set, which surpasses our baseline accuracy of 79.24%. Given the imbalance in our dataset, it is worth noting that the recall for negative reviews (ie. the percentage of actual negative reviews detected by the model) is 87%. This suggests that the model did not just assign all reviews as ‘positive’.  

4. Calculating the polarity of each term 

The linear SVM produces a hyperplane that maximises the margin between the support vectors in the ‘positive’ and ‘negative’ classes. The weights w_i  represent this hyperplane as coordinates of a vector orthogonal to the hyperplane. The direction of the weights vector gives the predicted class. Additionally, it also gives an indication of how important each feature was for the separation (i.e. how much each word in a review contributed to the overall sentiment of the review). Therefore, the weight of each term is its polarity score: 
    
    SVM as a linear binary classifier: 		
            Predict class ‘positive’ if s ≥ 0 
		Predict class ‘negative’ if s < 0 
		where s = b + ∑〖x_i * w_i 〗
                    
In selecting the top words, those that contained little information such as ‘great’, ‘amazing’, ‘awesome’, and synonyms such as ‘delicious; tasty’ were filtered from the list in order to identify what causes customers to give a high/ low star rating. The top 10 negative and positive words are shown in Figure 11 below: 
 
‘Delicious’ ranked first among all positive words and ‘bland’ ranked first among all negative words, indicating that taste weighs more than other factors like service and price when customers are judging a restaurant. In addition, the majority of negative words are related to the taste and hygiene of food items. Positive words such as ‘attentive’, ‘complaint’, ‘friendly’, ‘quickly’ all suggest that customer service is an important factor for a restaurant’s rating. The words ‘little’ and ‘hidden’ suggest that smaller, hidden, boutique restaurants earn higher review ratings than cookie-cutter chain restaurants. It was also observed that when it comes to the type of food, customers enjoy homemade and Sonoran style Mexican food. The word ‘overpriced’ ranked above the word ‘poor’, alluding that price contributed more to a low review rating rather than poor customer service.

5. Deep-dive into Competitors 

To gain more in-depth insight into why our restaurant is going to succeed, an analysis was performed on existing competition in the Mexican Breakfast and Brunch market. This will enable us to learn from our competitors, differentiate from them and create a competitive advantage. 

Competitors were selected based on their review count, with restaurants with a higher review count being a stronger competitor. The top three competitors are: Nacho Daddy, La Santisima, Border Grill. Prior to performing sentiment analysis on their reviews, how the sentiment of each competitor evolved over time was inspected by examining the three-month moving average of review ratings. Both Nacho Daddy and Border Grill exhibited upward trends over time, however, La Santisima suffered a downward trend in review ratings, which was especially obvious from 2017 onwards. 
 
Finally, sentiment analysis on review text was performed for each competitor using the same methodology as outlined above. The accuracy scores of the SVM models are 95.05%, 94.73%, 92.61% for each competitor respectively. 
