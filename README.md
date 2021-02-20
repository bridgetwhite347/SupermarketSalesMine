# SupermarketSalesMine

Supermarket Sales Data Final Project Report/Presentation 
by Sumaia Akhand, Dana Beggins, and Bridget White

Abstract: 
The consumer buying behavior is the study of individuals and firms who purchase goods and services. This sub-discipline of marketing is utilized by businesses to learn about their customers’ preferences and demographics. By gathering information from consumers, firms are better able to provide the goods and services that they desire as well as focus their marketing strategies on specific consumers. Focusing marketing on certain groups of people helps the business have more effective and efficient marketing, which leads the business to its overall goal of increasing profits. 

The goal of this data mining project is to help a supermarket chain with three branches to learn more about their consumers’ preferences and their store sales data to help make their business more profitable.

Introduction: 
The growth of supermarkets in most populated cities are increasing and market competitions are high. Businesses use the study of consumer buying behavior, the study of the purchasing of products and services by individuals and firms, to learn about the interests and demographics of their customers. Companies and businesses are best able to deliver the products and services they want by obtaining customer information, as well as concentrating their marketing campaigns on individual customers. Focusing marketing on specific groups of people tends to make the organization more profitable and successful, leading the company to the ultimate target of growing profits. The aim of this data mining project is to enable a retail chain with three or more branches to learn more about the interests of their customers and their sales details for their store. 
The project aims to learn model patterns in the consumer buying behavior and in store sales data for a supermarket chain in Myanmar (Burma). The dataset being utilized contains 1000 rows of information, with each representing one transaction by a customer at one of three branches of the supermarket. The project splits the dataset into two smaller parts in order to focus on the data attributes that are relevant to the goal of each experiment.
	The first part of the experiment focuses on finding relationships between consumer demographics and their buying behavior. This is important information for the business to use when creating their marketing strategies. This part uses unsupervised learning algorithms to segment the customers based on similar attributes.
	The second part of the project focuses on information related to store sales. This includes information about the branch each transaction occurred at, the category of goods the products purchased fall into, the quantity of goods purchased, payment method, and the consumer’s satisfaction rating. Examining the data is useful for the business to understand which branch is the most profitable and which consumers spend the most money.

Dataset Information:

The dataset contains 1000 entries and it has 13 columns. Of the columns, 7 are type float, 1 type int, and 9 are object. Each entry contains information about a sale from one of three locations of a supermarket. 


The columns of the dataset are described below:

Invoice id: Unique, computer generated sales slip invoice identification number

Branch: Branch of supermarket (3 branches are available identified by A, B and C).

City: Location of supercenters (Corresponds to branch data: A = Yangon, B = Mandalay, C = Naypyitaw)

Customer type: “Member” or “Normal” depending on if the customer uses store member card for transaction

Gender: Gender type of customer (Male or Female)

Product line: 6 General item categorization groups - Electronic accessories, Fashion accessories, Food and beverages, Health and beauty, Home and lifestyle, Sports and travel

Unit price: Price of each product in $

Quantity: Number of products purchased by customer

Tax: 5% tax fee for customer buying

Total: Total price including tax

Date: Date of purchase (Record available from January 2019 to March 2019)

Time: Purchase time (10am to 9pm)

Payment: Payment used by customer for purchase (3 methods are available – Cash, Credit card and Ewallet)

COGS: Cost of goods sold

Gross margin percentage: Gross margin percentage

Gross income: Gross income

Rating: Customer stratification rating on their overall shopping experience (On a scale of 1 to 10)
Distribution of Dataset


Experiments
Preprocessing steps:

The experimental setup begins with preprocessing the data. We began by looking at the columns and examining the column data types and how they will apply to the objectives. The original dataset contained several duplicate or irrelevant columns, which were dropped. These included ID, city, tax, total. On the reduced dataset, we looked for missing values in any of the columns, but there was no missing data in any of the 1000 rows.
The next step was to search for outliers in any of the columns. Using a box plot to visualize the data in each of the numerical columns, we could see if there were any outliers. To check for outliers in concrete manner, we applied a function to the column to calculate the upper and lower bounds of the 25th and 75th percentile. If there were any data points lower than the lower bound or higher than the higher bound, that was ruled an outlier. We tried to replace the outliers with either the upper or lower bounds and then visualized the change in the data by plotting a density plot. The density plot did not change, so we dropped the rows that contained an outlier.
Now that the dataset was reduced to 991 rows, we decided to use binning to discretize the continuous features. The ‘cogs’ column, which represents the total cost of goods purchased in the transaction, is stored as a float value, with all of the values between 10 and 901 dollars. We choose to use equal depth binning because even after removing the outliers, cogs values were skewed towards lower values. This is represented by the median cogs value being 240.72 and visualized in the density plot:
 
With equal-depth binning, there are an equal number of values in each bin, but each bin covers a different range of values. The smaller-valued bins cover a smaller range than the higher-valued bins. The unit price was put into equal-width bins, which each contains a similar number of values. For feature engineering in the Date and Time column, we extracted the day of the week that the transaction occurred and the time of day it occurred. We hypothesized that there would be a relationship between the day of the week a transaction occurred and the consumer buying behavior. The time of day was put into one of three bins for Morning, Afternoon and Evening.
The last step was to split up the features between two new dataframes: one for consumer behavior and one for store sales data. The data frame for consumer behavior contains: 'Customer type', 'Gender', 'Product line', 'Quantity', 'Day of Week', 'Time of Day', 'Payment', 'smooth_price_bin_mean'.  The data frame for store sales contains: 'Branch', 'Product line', 'Quantity', 'Day of Week', 'Time of Day', 'Payment', 'Rating', 'smooth_price_bin_mean', 'smooth_cogs_bin_mean'.

Modeling Consumer Behavior:

The next part of the experiment is modeling consumer behavior using unsupervised learning, specifically the Association Rule mining and K-Means clustering. The data needs to be normalized, which we chose to do using min-max normalization because the outliers have been removed so there are definite min and max values for the numerical features. For Association Rule mining, the data needs to be in One Hot Encoding format, which assigns each row a 0 or 1 to indicate whether that attribute is present in that row or not (0 indicates false while 1 indicates true). After converting the data to one hot encoding, we use the apriori algorithm to find frequent itemsets using a min support of 0.1. After visualizing the support, confidence, and lift of these association rules, we move on to K-Means clustering.
	It is important to use multiple models on the same information, so we then use K-Means clustering for the consumer behavior data. To use this algorithm, we need to drop the categorical data columns, including 'Customer type', 'Gender', 'Product line', 'Day of Week', 'Time of Day', 'Payment'. The features that were used to perform the K-Means clustering were Quantity and Unit Price. We started with 3 clusters and then used the euclidean distance algorithm to find the final number of clusters.  
	Finally, we used the Naive Bayesian Classification supervised learning model to explore relationships between categorical data related to customer behavior.  We converted numerical values to categorical values for the first Naive Bayesian Classification model.  For the second model we only used values that were originally categorical.  For each model, we trained both a Multinomial and Bernoulli classifier and evaluated their respective accuracies.


Modeling Store Sales:

The modeling for the store sales data involves unsupervised and supervised learning. The unsupervised learning is using Association Rule Mining and K-Means Clustering, and the Supervised learning is using Naive Bayesian Classification and Decision Tree. For this data, we again use min-max normalization for the numerical values. 
The association rules mining procedure is similar to that of the consumer behavior data, with converting to one hot encoding, then creating apriori association rules. We used a minimum support threshold of 0.3 to find the rules so there would be the most meaningful results.
We used K-Means clustering to search for groupings of similar transactions. The algorithm for this method is based only on the numerical features, which are unit price, cost of goods sold, quantity of goods purchased, and the customer’s satisfaction rating. For the K-means clustering, we ran the method twice, first with three clusters and then with two.
We used supervised learning models to try to predict the customer rating based on the other store sales data features. The first model we used for this is the Naive Bayesian classifier. First, to use a Naive Bayesian classifier on our data, we converted the numerical values to three categories: high, medium, and low. Then, we trained both a Multinomial and Bernoulli classifier and evaluated the accuracy of each.
	We also used another supervised model, decision tree, to predict the rating.  We created three different decision trees.  The original tree had a depth of 13, the second tree had a depth of 3, and the last tree’s depth was 4.  We used the score of each tree to determine which tree to use for predicting the rating.  

Ensemble: 

We tried using the bagging ensemble method to find patterns in consumer behavior.  Specifically we attempted to predict whether or not a customer would buy items with a high price based on the categorical data relating to customer behavior.  We created training and testing sets for the categorical consumer behavior data frame after applying one hot encoding to the data frame.  After training the data, we test the model.  We found the accuracy score and the confusion matrix.  Then we graphed the number of estimators vs. accuracy.


Results

Modeling Consumer Behavior:

Using association rules, we searched for patterns in the behavior and demographics of customers. This was used on categorical data in the consumer behavior data, which includes categories for Gender, Payment Method, Customer type, Day and Time. We looked at the association rules that had a confidence of at least 0.5 and lift of greater than 1 to determine the rules that indicated a positive correlation. These rules are in the table below:


In words, this data can be understood as customers who paid with a credit card tended to be members, morning shoppers tended to be female, female shoppers in the afternoon tended to be members, and male customers who paid with ewallet tended to not be members.
There is also a positive relationship between lift and confidence, meaning that features with a more positive correlation have higher confidence. 


The K-means clustering algorithm for consumer behavior deals with the numerical values for this data. These features are Quantity of goods purchased and Unit price of goods purchased. We were looking for groups of customers who purchased a similar number of products and products with similar unit price. We found that using 3 clusters was more accurate than using 2 clusters.  The three clusters are shown in the following plot:

Using K-means clustering on binned data produced an odd looking scatter plot. However,  visualizing the results by creating random circles with some variation following a Gaussian distribution based on the data set using the make_circles function proves to be better for visualizing the data.  The first graph shows the predicted distribution using the K-means value for two clusters.  The second graph shows the predicted distribution using Spectral Clustering.  The third graph shows the predicted distribution using DBSCAN.
 

To perform supervised learning on the consumer behavior data to classify the average unit price of the goods purchased, we trained a Naive Bayes Classifier to predict “High Price”, “Medium Price”, and “Low price”. We used both a Bernoulli and multinomial classifier, but neither of them were accurate. The Bernoulli has accuracy 0.33 (+/- 0.05)  and the multinomial has accuracy 0.31 (+/- 0.04). 

Using only the categorical data, we created Naive Bayes models to classify the Payment method of each transaction as “Credit card”, “Cash”, or “Ewallet”. Using the categorical consumer behavior data, we trained a Bernoulli and a multinomial classifier, but similar to predicting the price, the prediction for payment method using consumer data was not accurate. The Bernoulli has accuracy 0.34 (+/- 0.05) and the multinomial has accuracy 0.33 (+/- 0.05).   The low accuracy of the model led to our conclusion that categories gender, customer type, and what the customer bought have little to do with what payment method the customer used or how much the customer spent.


Modeling Store Sales:

We use association rules to derive relationships between categorical features in the store sales data. This includes Branch, Product line, Day of Week, Time of Day, and Payment method. After converting the data to one hot encoding format and applying the apriori algorithm, we look for association rules with a minimum support of 0.1 and confidence above 0.3. The confidence on all of the association rules is not higher than 0.5, but of the rules with a higher lift above 1.0, there was one rule that showed a correlation between Branch B and evening time. There was a higher number of rules that shows a negative correlation between variables. As in the consumer behavior data, there is a positive correlation between lift and confidence.  There was also a positive correlation between support and confidence.  These correlations can be visualized in the graphs below and the positively correlated association rules can be viewed in the chart below:


The most interesting findings in this table of correlations is the correlation between times of day and a branch. We found that there is a correlation between afternoon transactions and branch A, Afternoon and Branch C, and Evening and Branch B.

	
The k-means clustering algorithm for store sales data puts the data into k clusters based on numerical data, including unit price, cost of goods sold, quantity of goods sold, and customer rating. Using 2 clusters yielded a better Calinski-Harabaz score than using 3 clusters for this data.

Similar to the Consumer Behavior data frame, the results were better visualized by generating circles with a Gaussian distribution.  The first graph uses the K-means value to predict the distribution, the second uses Spectral Clustering, and the third uses DBSCAN.
 
For the classification of customer rating, we used a Naive Bayes Classifier. After converting the numerical values to categorical types, we could put the data in one hot encoding format, we train a Bernoulli Naive Bayes classifier and use cross validation to find the accuracy. We repeat this process with Multinomial Naive Bayes and find that the Multinomial classifier is slightly more accurate at 0.49 (+/- 0.03). This is still not considered very accurate because the predictions are only correct about half of the time. This leads us to believe that the factors that went into our calculation (branch, product line, day, time, cost, quantity, and price) may not have specific patterns to cause customers to rate their experience in a certain way. 
We created decision trees to model the categorical features to predict the rating category that the transaction falls into. The first tree we made yielded an accuracy score of 0.488, which is about the same as the accuracy of the Naive Bayes Classifier. The most accurate tree we made has an accuracy of 0.540, with a max depth of 4, compared to the first tree which went to 13 depth. Both of these classifiers for customer rating were not accurate enough to use to predict future customer rating.  Below is a visualization of the confusion matrix for the decision tree of the trained model along with the predicted ratings for the first ten items in the data frame.




Ensemble Method:
	The bagging ensemble method had the highest accuracy score of all the models at 0.79. The data frame used for bagging contained categorical values relating to consumer behavior and were encoded using one hot encoding.  We tested its accuracy using a confusion matrix and by graphing the change in accuracy based on its number of estimators.  The graphs are shown below: 

We are still looking for a way to implement the ensemble method to predict whether a customer will spend a high amount at the supermarket.  Currently we have not found the correct Python method to display the bagging model’s predictions to a data frame.

Conclusion
Our findings revealed that there were not many relationships between the features in this dataset. The goal of this project was to group the consumers based on similar characteristics and to find the factors that make consumers the most satisfied with their experience. The most successful part of this data mining process was the application of association rules on consumer data. 
There were limitations to the data, especially concerning the categorical features that were included in the dataset. Specifically, the ‘Product line’ attribute was ambiguous because it does not account for a transaction that has products from multiple product lines. In the future, this business can do market basket analysis if they collect more data about each item that a customer purchases in a transaction. 
We made assumptions about the data, including its relevance to the attribute being predicted in supervised learning. We split the data into two categories, focusing on the consumer behavior or the store sales data. Although we split it up for better understanding, these two categories are interrelated as the consumer buying behavior and demographics can have an effect on the transaction cost and other store sales data.
The data included the date of each transaction, all of which were within a three-month period from January to March of the same year. Instead of focusing on the date each purchase occurred, we thought that there was relevant information within the day of the week each purchase took place. In further analysis, it would be interesting to try mining techniques using data from an entire calendar year to see trends in buying behavior during different seasons.
