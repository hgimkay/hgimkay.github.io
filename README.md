# "Scraping Twitter Data and Conducting VADER Sentiment Analysis" (Python Tutorial for Social Science Researcher)

Social media facilitates web-based communication between people, allowing users to create and share contetents with others. Twitter is one such platform that provides the general public with a venue to share their opinions in a public forum. These characteristics intrigue social science researchers because they can study how people communicate each other in an online world about diverse topics such as politics, health, and social issues. In particular, the researchers can analyze tweets to understand people's general atttiudes, emotions, and opinions toward such agenda through sentiment analysis (i.e., opinion mining). Briefly, sentiment analysis is a natural language processing technique that determines the sentiment (e.g., positive, neutral, or negative) of a given text. 

In this tutorial, I will walk you through each step of conducting sentiment analysis on Twitter data.

## 1. What Do You Need?

This tutorial is based on **Python 3**. Therefore, you must install [Python 3](https://www.python.org/downloads/) on your machine. Plus, you will also need to install Tweepy, and NLTK (Natural Language Toolkit). 

Tweepy is a Python library that provide a simple and easy interface for accessing the Twitter API. 

NLTK is a most commonely used platfrom for processing human langague data.


### 1.1 Installing 

If you use [Jupyter Notebook](https://jupyter.org/), you can easily download both Tweepy and NLTK using pip. If you are using other applications (e.g., Window Shell),  detailed instructions on how to inistall [Tweepy](https://docs.tweepy.org/en/stable/getting_started.html) and [NLTK](https://www.nltk.org/install.html) are availabe in the links.

Run the line below in your Jupyter Notebook to install through pip.

```
pip install tweepy
pip install nltk
```

## 2. Scraping Twitter

### 2.1 Twitter Developer Account
To use the Twitter API with Tweepy, you must create a Twitter Developer Account and apply for access to the Twitter API. 

Follow the steps in https://developer.twitter.com/en/support/twitter-api/developer-account. Once your account is approved by Twitter, Go to the "Keys and Tokens" tab. 


Click on the "Generate" button under "Consumer Keys" to generate your API key and secret AND **write down your keys and save them**.

Click on the "Generate" button under "Authentification Tokens" to generate your access token and secret AND **write down your keys and save them**.

After this step, you should have FOUR different keys.

### 2.2 Twitter API Authentication 

Now, we will go through the step for Twitter API authentification with your API key, API secret key, access token, and access token secret.

```
import tweepy

api_key = 'COPY AND PASTE YOUR_API_KEY'
api_key_secret = 'COPY AND PASTE YOUR_API_KEY_SECRET'
access_token = 'COPY AND PASTE YOUR_ACCESS_TOKEN'
access_token_secret = 'COPY AND PASTE YOUR_ACCESS_TOKEN_SECRET'

authenticate = tweepy.OAuthHandler(api_key, api_key_secret)
authenticate.set_access_token(access_token, access_token_secret)
api = tweepy.API(authenticate, wait_on_rate_limit=True)
```
### 2.3 Scrapping Tweets with Search Queries

Assuminig you have successfully authenticated with the Twitter API, you can use Tweepy to search for tweets that match a certain query. To demonstrate this, I will show your how to search for tweets mentioning "Putin". For this particular example, I retrieve 10 tweets mentioning "Putin" that are not retweets and have their full text available in English.

To learn more about all the available search parameters and options (e.g., how to set the timeline), please click [here](https://docs.tweepy.org/en/stable/client.html?highlight=search#search-tweets). 

```
tweets = api.search_tweets(q="Putin -filter:retweets", lang="en", result_type="recent", count=10, tweet_mode='extended')
for tweet in tweets:
    print(tweet.full_text)
```

The output:

```
@Christi93340165 @Andre__Damon Putin and Xi are committing genocides atm, Biden is not
@CitizenAnkit Considering recent attempts of assassination against aleksandr Dugina &amp; tatarsky, demolition of Crimea bridge &amp; a Ukranian banker announcing a bounty of $600,000 dollars over a drone strike in Moscow, does this attack a contribution of Kremlin?
We're talking about Putin🤔🫤
@FahQueAll The way I see it. Putin is the only world leader standing up to the globalists tho.
@EdKrassen Zelensky’s putting up too much of a fight, so it wouldn’t be surprising for Putin to come up with this potentially contrived situation as an excuse.
@wartranslated The fire department was already on the roof? false flag, Putin needs a reason to commit terror.. https://t.co/xAxOw5f8ux
When the hell are you going to stop the monstrous savage murdering of Ukrainians? After they are eradicated by war criminal Putin? @NATO @EUCouncil @POTUS https://t.co/emTZ8yirjs
@RitaMar93112090 @Spriter99880 You're wrong
https://t.co/py256qZdjf
@EdKrassen Putin is completely capable of that and would risk anyone’s life except his own to make it look real.  Wouldn’t surprise me if he did.
@DefMon3 Even if it was Ukrainians (and the drone seems to far too small for that), it's laughable to claim that was an attempt to kill Putin.
@chosen_zero_ftw Oh c'mon, dictators rarely survive losing a war. And when Putin's gone, they will free Navalny. You think he won't run for an office? 
I have zero empathy for him. I don't care what they do to him. Also, look at Saakashwili now. That's how a tortured person looks.
```
### 2.4 Preprocessing Tweets
As we see from the output, the tweets are quite noisy. To improve the performance of Natural Language Process mdoel, I pre-process these tweets.  

***Disclimer: Depending on the purpose of your analysis, you may want to keep certain patterns (e.g., hashtags). If you need these patterns for your own analysis, do not delete them. Preprocessing should be tailored to your specific needs and objectives.***

```
import re
# Define regular expressions to remove unwanted patterns
url_pattern = r" ?(f|ht)(tp)(s?)(://)(.*)[.|/](.*)"
mention_pattern = r"@[a-zA-Z0-9_]+"
hashtag_pattern = r"#[a-zA-Z0-9_]+"

# Extract the text of each tweet, preprocess it, and store in a list
tweet_texts = []
for tweet in tweets:
    # Preprocess the tweet text
    text = tweet.full_text
    text = re.sub(url_pattern, "", text)
    text = re.sub(mention_pattern, "", text)
    text = re.sub(hashtag_pattern, "", text)
    text = text.replace('&amp;', '')
    text = text.strip()
    # Append preprocessed text to list
    tweet_texts.append(text)
    
for text in tweet_texts:
    print(text)
```
The output:

```
Putin and Xi are committing genocides atm, Biden is not
Considering recent attempts of assassination against aleksandr Dugina  tatarsky, demolition of Crimea bridge  a Ukranian banker announcing a bounty of $600,000 dollars over a drone strike in Moscow, does this attack a contribution of Kremlin?
We're talking about Putin🤔🫤
The way I see it. Putin is the only world leader standing up to the globalists tho.
Zelensky’s putting up too much of a fight, so it wouldn’t be surprising for Putin to come up with this potentially contrived situation as an excuse.
The fire department was already on the roof? false flag, Putin needs a reason to commit terror..
When the hell are you going to stop the monstrous savage murdering of Ukrainians? After they are eradicated by war criminal Putin?
You're wrong
Putin is completely capable of that and would risk anyone’s life except his own to make it look real.  Wouldn’t surprise me if he did.
Even if it was Ukrainians (and the drone seems to far too small for that), it's laughable to claim that was an attempt to kill Putin.
Oh c'mon, dictators rarely survive losing a war. And when Putin's gone, they will free Navalny. You think he won't run for an office? 
I have zero empathy for him. I don't care what they do to him. Also, look at Saakashwili now. That's how a tortured person looks.
```


## 3. Conducting VADER (Valence Aware Dictionary for Sentiment Reasoning) Sentiment Analysis
Now we will conduct sentiment analysis on the pre-processed tweets using NLTK VADER sentiment analysis.

### 3.1 Download Vader Lexicon and Import Sentiment Analyzer
First, we will download vader lexicon dictionary and import SentimentIntensityAnalyzer. We will use polarity_scores() method to obtain overall sentiment scores.

```
nltk.download('vader_lexicon')
from nltk.sentiment.vader import SentimentIntensityAnalyzer
sa = SentimentIntensityAnalyzer()
```

### 3.2 Explanation of Scores: What does polarity_scores() provide?
I will briefly explain what each score entity means with a simple example sentence: "This was a good movie."

```
a = 'This was a good movie.'
sa.polarity_scores(a)
```

The output:
```
{'neg': 0.0, 'neu': 0.508, 'pos': 0.492, 'compound': 0.4404}
```

As you see in the above output, four different scores are given: neg (negative sentiment score), neu (neutral sentiment score), pos (positive sentiment score), compound (normalized sentiment score). 

The output indicates 50.8% of the words in the sentence are neutral and 49.2% of the words are positive. None of the words are negative (0%). Finally, the compound score is 0.4404 (a normalized sentiment score that ranges from -1: negative to +1: positive). Baseed on the compund score, we can interpret this sentence implies a slightly positive sentiment.

The detailed discussion of how the compound score is calculated/normalized is avaialbe [here](https://medium.com/@mystery0116/nlp-how-does-nltk-vader-calculate-sentiment-6c32d0f5046b).

### 3.3 Conducting Sentiment Analyis on the Preprocessed Tweets
Now, it is time to conduct sentiment analysis on the tweets that we preprocessed.

```
for text in tweet_texts:
    print(text)
    print()
    sentiment = sa.polarity_scores(text)
    print(sentiment)
    print('-' * 50)
```

The output:
```
Putin and Xi are committing genocides atm, Biden is not

{'neg': 0.0, 'neu': 0.874, 'pos': 0.126, 'compound': 0.0772}
--------------------------------------------------
Considering recent attempts of assassination against aleksandr Dugina  tatarsky, demolition of Crimea bridge  a Ukranian banker announcing a bounty of $600,000 dollars over a drone strike in Moscow, does this attack a contribution of Kremlin?
We're talking about Putin🤔🫤

{'neg': 0.22, 'neu': 0.78, 'pos': 0.0, 'compound': -0.8412}
--------------------------------------------------
The way I see it. Putin is the only world leader standing up to the globalists tho.

{'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
--------------------------------------------------
Zelensky’s putting up too much of a fight, so it wouldn’t be surprising for Putin to come up with this potentially contrived situation as an excuse.

{'neg': 0.093, 'neu': 0.786, 'pos': 0.121, 'compound': -0.0516}
--------------------------------------------------
The fire department was already on the roof? false flag, Putin needs a reason to commit terror..

{'neg': 0.129, 'neu': 0.753, 'pos': 0.118, 'compound': -0.0516}
--------------------------------------------------
When the hell are you going to stop the monstrous savage murdering of Ukrainians? After they are eradicated by war criminal Putin?

{'neg': 0.576, 'neu': 0.424, 'pos': 0.0, 'compound': -0.9711}
--------------------------------------------------
You're wrong

{'neg': 0.756, 'neu': 0.244, 'pos': 0.0, 'compound': -0.4767}
--------------------------------------------------
Putin is completely capable of that and would risk anyone’s life except his own to make it look real.  Wouldn’t surprise me if he did.

{'neg': 0.072, 'neu': 0.756, 'pos': 0.172, 'compound': 0.4391}
--------------------------------------------------
Even if it was Ukrainians (and the drone seems to far too small for that), it's laughable to claim that was an attempt to kill Putin.

{'neg': 0.157, 'neu': 0.803, 'pos': 0.04, 'compound': -0.6705}
--------------------------------------------------
Oh c'mon, dictators rarely survive losing a war. And when Putin's gone, they will free Navalny. You think he won't run for an office? 

I have zero empathy for him. I don't care what they do to him. Also, look at Saakashwili now. That's how a tortured person looks.

{'neg': 0.114, 'neu': 0.729, 'pos': 0.157, 'compound': 0.3404}
--------------------------------------------------
```

### 3.4 Evaluation 
NLTK's VADER sentiment analysis model is a pre-trained model that has been demonstrated to be highly accurate (see [Hutto & Gilbert, 2014](https://ojs.aaai.org/index.php/ICWSM/article/view/14550); achieving an F1 score of 0.96 with the Big Tweet Dataset). It has been widely used in various disciplines such as Political Science, Linguistics, Communication, and Psychology as a popular choice for sentiment analysis. 

That said, although I only have 10 tweets, I conducted a simple evaluation of how well VADER performs on my sample.

```
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

pre_determined_labels = ["positive", "negative", "positive", "negative", "negative", "negative", "negative", "negative", "negative", "negative"]

vader_labels = []
for text in tweet_texts:
    score = sa.polarity_scores(text)
    if score['compound'] > 0:
        vader_labels.append("positive")
    else:
        vader_labels.append("negative")

# Calculate the evaluation metrics
accuracy = accuracy_score(pre_determined_labels, vader_labels)
precision = precision_score(pre_determined_labels, vader_labels, average='weighted')
recall = recall_score(pre_determined_labels, vader_labels, average='weighted')
f1 = f1_score(pre_determined_labels, vader_labels, average='weighted')

# Print the evaluation metrics
print(f"Accuracy: {accuracy:.2f}")
print(f"Precision: {precision:.2f}")
print(f"Recall: {recall:.2f}")
print(f"F1 score: {f1:.2f}")
```

The output:
```
Accuracy: 0.70
Precision: 0.75
Recall: 0.70
F1 score: 0.72
```

**Please keep in mind that this evaluation is based on a small sample of only 10 tweets. However, the VADER did a good job analyzing the sentiment of each tweet.**

## Concluding Remarks

While this approach may not be the perfect method for analyzing Twitter data, I believe that using VADER to understand Twitter users' attitudes towards certain topics is an effective technique. It's worth noting that there are more sophisticated ways of conducting sentiment analysis at a higher level. However, this tutorial is designed to assist early scholars interested in analyzing text data from microblogging sites like Twitter or Reddit.

If you have any questions or comments, please feel free to contact me via [email](hgimkay@arizona.edu).

## Authors
* **Dr. Hyeonchang Gim (Kay)** - PhD in Communication: [Google Scholar](https://scholar.google.com/citations?user=29oM56gAAAAJ&hl=en), [ORCID](https://orcid.org/0000-0003-4891-6491)

## License/Version
Tweepy: [MIT License](https://github.com/tweepy/tweepy/blob/master/LICENSE)
NLTK: [Apache License 2.0](https://github.com/nltk/nltk/blob/develop/LICENSE.txt)

## References
Hutto, C., & Gilbert, E. (2014). VADER: A Parsimonious Rule-Based Model for Sentiment Analysis of Social Media Text. Proceedings of the International AAAI Conference on Web and Social Media, 8(1), 216-225. https://doi.org/10.1609/icwsm.v8i1.14550
