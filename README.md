# "Scraping Twitter Data and Conducting VADER Sentiment Analysis" (Python Tutorial for Social Science Researcher)

Social media facilitates web-based communication between people, allowing users to create and share contetents with others. Twitter is one such platform that provides the general public with a venue to share their opinions in a public forum. These characteristics intrigue social science researchers because they can study how people communicate each other in an online world about diverse topics such as politics, health, and social issues. In this tutorial, I will walk you through each step of conducting sentiment analysis on Twitter data.

## 1. What Do You Need?

This tutorial is based on **Python 3**. Therefore, you must install [Python 3](https://www.python.org/downloads/) on your machine. Plus, you will also need to install Tweepy, NLTK (Natural Language Toolkit), Tweepy.


### 1.1 Installing 

If you use [Jupyter Notebook](https://jupyter.org/), you can easily download both Tweepy and NLTK using pip. If you are using other applications (e.g., Window Shell),  detailed instructions on how to inistall [Tweepy](https://docs.tweepy.org/en/stable/getting_started.html) and [NLTK](https://www.nltk.org/install.html) are availabe in the links.

Run the line below in your Jupyter Notebook to install through pip.

```
pip install tweepy
pip install nltk
```

## 2. Scraping Twitter

### 2.1 Code



A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
tweets = api.search_tweets(q="Putin -filter:retweets", lang="en", result_type="recent", count=10, tweet_mode='extended')
for tweet in tweets:
    print(tweet.full_text)
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
