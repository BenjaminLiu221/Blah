# Relevant Content Identifier in r/FrugalMaleFashion

### Problem Statement

It is the time of ecommerce and online shopping with companies or brands building their online presence to cater to customers to shop online. Shopping online for clothing and footwear was becoming more common. 

Reddit's [Frugal Male Fashion subreddit](https://www.reddit.com/r/frugalmalefashion) is where users share information about coupons, discounts and sales on a variety of brands and stores. Popular posts are upvoted and headlined at the top, but users can also filter for most recent or most relevant searches. The continuous postings can streamline viewers to the forum increasing advertisement revenue. 

In order to maintain a clean environment and better retain constant users online, only content that is relevant to the community will be allowed to be posted and irrelevant content will be removed. An example of irrelevant content is a user posting present or future acquisition of hype sneakers. We will be retrieving posts from Reddit's [Sneakers subreddit](https://www.reddit.com/r/sneakers) where users share their present or future acquisitions of hype sneakerwear.

Posts from `r/frugalmalefashion` will be Class 1 and posts from `r/sneakers` will be Class 0. We will be analyzing the documents to build a model that accurately classifies a post as from Class 1 but also outputs interpretable results for downstream data visualization and businesss applications.

### Contents

 - [Description of Data](#Description-of-Data)
 - [EDA Cleaning and Preprocessing](#EDA-Cleaning-and-Preprocessing)
 - [Production Modeling](#Production-Modeling) 
 - [Conclusion and Recommendations](#Conclusion-and-Recommendations)
 - [Technologies Used](#Technologies-Used)

### Description of Data

The data is text data from posts scraped from the subreddits `r/frugalmalefashion` and `r/sneakers` starting from March 6, 2019. The data was collected using Reddit's pushshiftAPI and a function with the APIs incorporated inside to continuously send requests and save the data as json files into a SQL database. We collected 50,000 posts from [Frugal Male Fashion subreddit](https://www.reddit.com/r/frugalmalefashion/) subreddit with 993k users and 50,0000 posts from [Sneakers subreddit](https://www.reddit.com/r/sneakers/) subreddit with 678k users. `Class 1` is `r/frugalmalefashion` and `Class 0` is `r/sneakers`.

### Target

The aim was to build the best model that best explains the most important words in identifying relevant and irrelevant posts and their odds in classifying the post. From the model built, I will be evaluate my problem statement in which I am looking for the terms that best classify relevant and irrelevant posts.

### EDA Cleaning and Preprocessing

A dataframes was created for each subreddit to contain `title`, `selftext` and `year` of the posts. Duplicate posts were removed resulting. `title` and `selftext` of posts were combined into column `text`. The two dataframes were combined. There was a class slight imbalance with the majority class being `r/sneakers`. To clean the text, we tokenized the text by splitting it into a list of words. The mentions of the respective subreddit were removed. Links, special characters and redundant whitespaces were removed. Words were lemmatized and converted to their base forms. Stop words were removed from the text. I split the dataset into train and test datasets before using TF-IDF vectorizer to re-evaluate how weights of terms are calculated based on frequency across all documents.

### Production Modeling
- Naive Baseline
    - Accuracy score = 53.92%
    - `r/sneakers` is our majority class. If we classify all posts as belonging in the `r/sneakers` subreddit, we will be predicting correctly 53.92% of the time.
- Logistic Regression
    - Accuracy score = 78.77%
    - The model correctly predicted 10731 posts to be in `r/sneakers` (true negative).
    - The model correctly predicted 5850 posts to be in `r/frugalmalefashion` (true positive).
    - Able to extract features from model to see which words increase the likelihood of a post being classified in `r/frugalmalefashion`

### Conclusion and Recommendations

The model we will put into production will the Logistic Regression model with an accuracy score of 78.77% on unseen data and with human interpretable results for downstream business applications. The model is able to provide us with the terms that best classify a post as relevant in the community of `r/frugalmalefashion` and ultimate maintain the clean environment to retain users and increase advertisement revenue.

The terms used to best classify relevant posts are: `sale`, `code`, `item`, `free`, `off`, `shipping`. These terms relate to items as being on sale such as in stores or for brands.

The terms used to be classify irrelevant posts are: `nike`, `sneaker`, `pair`. These terms are related to hype sneakerwear culture and relate to users posting about present or future acquisitions of such sneakers.

The next step of this project would be to re-evaluate the cleaning and preprocessing of the data to account for word frequency overlap between the two subreddit as well as including sentiment analysis on how words are being used in conjunction with each other in the text. After that, we could continuously collect new data to search for new signal to contribute to our model.

### Technologies Used
- Rest API: Reddit PushShiftAPI
- Data Management: Postgres SQL, pandas, numpy, regex, json
- Feature Extraction: CountVectorizer, TfidfVectorizer
- Modeling: Logistic Regression
- Model Selection: train/test split, GridSearchCV