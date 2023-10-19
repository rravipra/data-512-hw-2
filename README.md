# Considering Bias in Data

# Goal:

The goal of this project is to explore the concept of bias in data using Wikipedia articles. It will consider articles about cities in different US state and I will combine a dataset of Wikipedia articles with a dataset of state populations, and use a machine learning service called ORES to estimate the quality of the articles about the cities.
 
I will also be performing an analysis of how the coverage of US cities on Wikipedia and how the quality of articles about cities varies among states. My analysis will consist of a series of tables that show:

1) The states with the greatest and least coverage of cities on Wikipedia compared to their population.
2) The states with the highest and lowest proportion of high quality articles about cities.
3) A ranking of US geographic regions by articles-per-person and proportion of high quality articles.

# Data:

Getting quality scores from a Machine Learning system [ORES](https://www.mediawiki.org/wiki/ORES) and these labelings were learned based on articles in Wikipedia that were peer-reviewed using the [Wikipedia content assessment](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment) procedures

The example notebook code is licensed under: [Creative Commons](https://creativecommons.org) and [CC-BY license](https://creativecommons.org/licenses/by/4.0/)

API info: [The API Info](https://www.mediawiki.org/wiki/API:Info)

The data can be acquired from the [MediaWiki REST API for the EN Wikipedia](https://www.mediawiki.org/wiki/API:Main_page) for the page_info from which you can extract the 'title' and the 'lastrevid' which you will need to then use it to extract the ORES scores from the LiftWing ML Service API.

Get your access token to get the data:

You will need a Wikimedia user account to get access to Lift Wing (the ML API service). You can either [create an account or login](https://api.wikimedia.org/w/index.php?title=Special:UserLogin). If you have a Wikipedia user account - you might already have an Wikimedia account. If you are not sure try your Wikipedia username and password to check it. If you do not have a Wikimedia account you will need to create an account that you can use to get an access token.

There is [a 'guide' that describes how to get authentication tokens](https://api.wikimedia.org/wiki/Authentication) - but not everything works the way it is described in that documentation. You should review that documentation and then read the rest of this comment.

The documentation talks about using a "dashboard" for managing authentication tokens. That's a rather generous description for what looks like a simple list of token things. You might have a hard time finding this "dashboard". First, on the left hand side of the page, you'll see a column of links. The bottom section is a set of links titled "Tools". In that section is a link that says [Special pages](https://api.wikimedia.org/wiki/Special:SpecialPages) which will take you to a list of ... well, special pages. At the very bottom of the "Special pages" page is a section titled "Other special pages" (scroll all the way to the bottom). The first link in that section is called [API keys](https://api.wikimedia.org/wiki/Special:AppManagement). When you get to the "API keys" page you can create a new key.

The authentication guide suggests that you should create a server-side app key. This does not seem to work correctly - as yet. It failed on multiple attempts when I attempted to create a server-side app key. BUT, there is an option to create a [Personal API token](https://api.wikimedia.org/wiki/Authentication) that should work for this course and the type of ORES page scoring that you will need to perform.

Note, when you create a Personal API token you are granted the three items - a Client ID, a Client secret, and a Access token - you shold save all three of these. When you dismiss the box they are gone. If you lose any one of the tokens you can destroy or deactivate the Personal API token from the dashboard and then create a new one.

# Code files:

The notebook files in this repository are present in the folder called Code_files and the files in that are:

get_ores_scores.ipynb : This file consists of the some code in the beginning from the [wp_page_info_example.ipynb](https://drive.google.com/drive/folders/1FtvWV31DHE8HIMdEsPGuCXPz0PMvShfl) with a few changes which I have explicitly mentioned in the notebook.

get_page_info.ipynb : This file consists of the some code in the beginning from the [wp_ores_liftwing_example.ipynb](https://drive.google.com/drive/folders/1FtvWV31DHE8HIMdEsPGuCXPz0PMvShfl) with a few changes which I have explicitly mentioned in the notebook.

Combining_and_Analysis.ipynb: This file consists of that analysis and combining all the datasets to make a single dataset.


# Data Files created from the code:

1) ores_scores.csv
2) page_info_per_city.csv
3) wp_scored_city_articles_by_state.csv

**Structure of the CSV files:**
For ores_scores.csv

**title**: The title (i.e the city, state).

**rev_id**: The last revision id.

**prediction**: The prediction retrieved from the ORES scores.

For page_info_per_city.csv

**title**: The title (i.e the city, state).

**rev_id**: The last revision id.

For wp_scored_city_articles_by_state.csv

**state**: The state in U.S.

**regional_division**: The division that the state belongs to.

**population**: The population of that particular state.

**revision_id**: The last revision id.

**article_quality**: This is the same as the prediction from the ORES scores.

Example structure of the json files for the page info and the ORES scores (this is extracted from the title and the last revision id).

```python
Getting page info data for: Northern flicker
{
    "351590": {
        "pageid": 351590,
        "ns": 0,
        "title": "Northern flicker",
        "contentmodel": "wikitext",
        "pagelanguage": "en",
        "pagelanguagehtmlcode": "en",
        "pagelanguagedir": "ltr",
        "touched": "2023-10-18T00:13:37Z",
        "lastrevid": 1179719310,
        "length": 27754,
        "watchers": 105,
        "talkid": 8324488,
        "fullurl": "https://en.wikipedia.org/wiki/Northern_flicker",
        "editurl": "https://en.wikipedia.org/w/index.php?title=Northern_flicker&action=edit",
        "canonicalurl": "https://en.wikipedia.org/wiki/Northern_flicker"
    }
}
```

```python
Getting LiftWing ORES scores for 'Bison' with revid: 1085687913
{
    "enwiki": {
        "models": {
            "articlequality": {
                "version": "0.9.2"
            }
        },
        "scores": {
            "1085687913": {
                "articlequality": {
                    "score": {
                        "prediction": "FA",
                        "probability": {
                            "B": 0.07895665991827401,
                            "C": 0.03728215742560417,
                            "FA": 0.5629436065906797,
                            "GA": 0.30547854835374505,
                            "Start": 0.011061807252218824,
                            "Stub": 0.00427722045947826
                        }
                    }
                }
            }
        }
    }
}
```


# Research Implications:

Over the span of this project I got a much better understanding and hands on experience with using APIs and how we are able to extract data by making API calls. Any data science project as you know begins with having the right data and to have the skills/knowledge to know where to get/extract the data from. This project provided me with the resource and hands on experience to exactly do that in one of the ways which is by extracting data by making API calls. Further, I also learned and noticed that the ORES predictions are pretty generous when compared to the peer reviewed journals. Something that I noticed was the fact that Wikipedia might not necessarily be the best data source if we want to produce high quality research results.

I found out that some states that have much lower population seem to produce the best quality articles and also the fact that higher the number of articles does not necessarily mean that the particular state has a higher value for the number of high quality articles per capita based on the population of that state. For example even though Pennsylvania and Michigan had way more number of articles than states like South Dakota or Vermont (ie. approximately more than 5x the number of articles) they were ranked much lower in terms of the highest quality of articles based on the population of that state. So it can be said that there is a much higher quality in states like Vermont and South Dakota since it represents the quality per population much better.

Something that surprised me about the findings was the fact that none of the top states in the US that are considered to have the best educational institutions, libraries etc. directly reflected the highest quality of articles. For example: it was surprising to see New York, California and Florida in the bottom 10 of the states that produced the highest quality articles. They are also the states in the US that fall in the top 10 most literate states whereas I noticed that states like Vermont, South Dakota which comparatively have a much lower literacy rate and population seemed to have much higher quality of articles.

Answering specific questions:

What biases did you expect to find in the data (before you started working with it), and why?

Before I started working with the data, I expected to find biases towards states with larger cities, popular landmarks, and those having a rich historical background. The reason for that was primarily because the mentioned factors often contribute to a state's prominence and representation in public discourse and thus leading to more articles and potentially higher quality content.

What (potential) sources of bias did you discover in the course of your data processing and analysis?

Upon closer examination of the data, there were several potential sources of bias that became evident. Editorial focus appeared to play a significant role, with certain states possibly having a more active Wikipedia contributor community. Additionally, there was an indication that states with better access to resources, like academic institutions or libraries, had articles of superior quality.States with better access to resources did not seem to necessariy have the top articles per capita with the highest quality.

What might your results suggest about (English) Wikipedia as a data source?

As wikipedia is crowd sourced I would say that it is not necessarily the best data source to get the best quality results. The results show that the top states in the US that have a better access to resources, like academic institutions, are not necessarily the states that provide articles of superior quality. This is not something that you would expect in the results so it can be inferred that data sources other than wikipedia also need to be taken into consideration for a better analysis to give us the best results.

