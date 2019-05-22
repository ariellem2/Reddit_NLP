# Porject 3: Reddit Web Scraping and NLP


Arielle Miro 
DSI-7

### EXECUTIVE SUMMARY

---

We (reddit) have been experiencing some classification difficulties in our platform. Specifically, subreddits that are similar in nature are being misclassified. 


How can we use Data Science to solve this?  

1. Collect posts from two subreddits that are similar in nature using the Reddit API. 
2. Leverage NLP to train a binary classification model on which subreddit a given post originates from. 
 

### Introduction

**Objective**: The goal is to build a model that will accurately determine the subreddit that a post comes from. Various classification models were used to achieve an optimal accuracy score and minimize misclassification. 

---

### Overview
    
**Data**
Source: Reddit API
Subreddits: 
    1. "Life Pro Tips": Tips to improve your life in one way or another. 
        - Size: 4474 observations 
    2. "Explain Like I'm Five":  A forum and archive on the internet for layperson - friendly explanations.
        - Size: 4500 rows observations  
- Overall Size: 8,974 observations
- Classes : Fairly balanced 
    - 47.3% are LPT 
    - 52.7% are ELI5 
- For modeling purposes, the positive class (1) is the subreddit "Life Pro Tips".

**Tools**
- Python (Pandas, Scikit-Learn, Seaborn, Matplotlib )
- API (Time, Requests, Json)
- NLP (Regex, Nltk, CountVectorizer, TfidfVectorizer)

### Data Cleaning 
    
- Removed index column 
- Saved the API pull as csv
- Deleted [removed] posts 
- Combined selftext and title into one column. In NLP, text = data, so more text means more data and therefore potential for a stronger model  
- Lowercased all the text 
- Removed all punctuation with Regex 
- Lemmatized text to group root words together 


- Removed keywords - some subreddits require keywords that preceed the text of post. To some extent, the keywords already classify the posts. In this case, 'lpt' and 'eli5' were removed in order to test the model on the actual content of the post rather than the keywords. 

### Methodology 

**Regression Models **
Several regression models were built for the binary classification. Besides the baseline model, the remaining models were instantiated with a Pipeline and adjusted using Gridsearch.

1. Baseline Model 
    - Model: Logistic Regression 
    - Scale: CountVectorizer 
    - Parameters: Default 
    - Scores: Train 99.8% Test 95.9%
2. Logistic Regression II
    - Model: Logistic Regression 
    - Scale: CountVectorizer 
    - Parameters: Custom (see notebook for details)
    - Scores: Train 99.9% Test 95.8%
3. Logistic Regression III
    - Model: Logistic Regression 
    - Scale: TfidfVectorizer 
    - Parameters: Custom (see notebook for details)
    - Scores: Train 99.9% Test 96.3%
4. Naive Bayes I
    - Model: MultiNomial Naive Bayes 
    - Scale: CountVectorizer 
    - Parameters: Custom (see notebook for details)
    - Scores: Train 98.4% Test 96.0%
5. Naive Bayes II
    - Model: MultiNomial Naive Bayes 
    - Scale: TfidfVectorizer 
    - Parameters: Custom (see notebook for details)
    - Scores: Train 98.3% Test 95.8%
 

### Conclusion & Recommendations

Based on the model research, the below findings and recommendations emerged from the model. 

**Findings**
- 4 out of the 5 models that used GridSearch found the best scores while keeping stop words in the text. 
    - This could be because both subreddits are about explaining topics and require many basic english stop words.
- 6 out of the top 10 words for both subreddits overlapped. 
- The model with the smallest train-test difference is the NB II.
- The model with the highest test score is the Logistic Regression with TFIDF Vectorizer. 


**Final Model**
Logistic Regression with TFIDF
- This model had the highest test scores.
- 96.4% Accuracy with only 52 false positives/false negatives 
- Sensitivity and Specificity are fairly even at about 96%
- Best Parameters used for this model:
    - logreg__C: 10 
    - logreg__n_jobs:  -2
    - logreg__penalty: l2
    - tfidf__max_features: 15,000
    - tfidf__ngram_range: (1, 2)
    - tfidf__stop_words: None

**Next Steps**
- Try a Random Forests model.
- Try a Voting Classifier model. 
- Test the current model on smaller and more specific subreddits. 
- Test the current model on other subreddits that don’t have too many words in common. 

 