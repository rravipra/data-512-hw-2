# Considering Bias in Data

# Goal:

The goal of this project is to explore the concept of bias in data using Wikipedia articles. It will consider articles about cities in different US state and I will combine a dataset of Wikipedia articles with a dataset of state populations, and use a machine learning service called ORES to estimate the quality of the articles about the cities.

# Data:

API Licensed under: [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) and [GFDL](https://www.gnu.org/licenses/fdl-1.3.html)

API endpoint: [endpoint](https://wikimedia.org/api/rest_v1/#!/Pageviews_data/get_metrics_pageviews_aggregate_project_access_agent_granularity_start_end)  

API Request Pageviews Endpoint: https://wikimedia.org/api/rest_v1/metrics/pageviews/

Terms and conditions: https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions

API Documentation:  https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews

The data can be acquired from the API request endpoint, please refer to the code files (i.e notebook) in this repository for the code on how to do that.

# Code files:

The only notebook file in this repository is the 512_HW1_code.ipynb file. This file consists of the some code in the beginning from the [example notebook](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fdrive.google.com%2Ffile%2Fd%2F1XjFhd3eXx704tcdfQ4Q1OQn0LWKCRNJm%2Fview%3Fusp%3Dsharing) with a few changes which I have explicitly mentioned in the notebook. The notebook then consists of the code for acquiring the data from the Pageviews API and saving them as JSON files based on the type of access. Finally, it consists of the code for data analysis and creating time series graphs to get insights on that data based on a few specific questions which you can read through in the notebook.


# Data Files created from the code:

1) academy_monthly_desktop_20150101-20230401.json
2) academy_monthly_mobile_20150101-20230401.json
3) academy_monthly_cumulative_20150101-20230401.json

All of these can be found in the JSON_files.rar folder from where you can extract it from.

**Structure of the JSON files:**

**Key**: The movie (or article) name, e.g., "Everything Everywhere All at Once".

**Value**: A list of dictionaries. Each dictionary provides data about monthly views for the corresponding movie on Wikipedia.

**Fields in the Dictionary:**
**project**: The Wikipedia project. In this case, it's "en.wikipedia", which refers to the English version of Wikipedia.

**article**: The Wikipedia article's name, using underscores in place of spaces. This matches the key of the outer dictionary. Example: "Everything_Everywhere_All_at_Once".

**granularity**: The granularity of the view data. It's "monthly" for our dataset, which means each entry represents one month's views.

**timestamp**: A timestamp indicating the starting time of the data. The format is YYYYMMDDHH. For example, "2020010100" refers to January 1, 2020, at 00:00 hours.

**agent**: The type of agent accessing the article. In this dataset, it's always "user", referring to human readers as opposed to bots or web crawlers.

**views**: The number of views for the article in the corresponding month. This is an integer value. For example, in January 2020, the article "Everything Everywhere All at Once" was viewed 1,209 times.

Example structure:

```python
{
    'article1' : [
        {
            "project": "en.wikipedia", 
            "article": article1,
            "granularity": "monthly",
            "timestamp": month1_article1,
            "agent": "user",
            "views" : num_views_1_article1
        },
        {
            "project": "en.wikipedia", 
            "article": article1,
            "granularity": "monthly",
            "timestamp": month2_article1,
            "agent": "user",
            "views" : num_views_2_article1
        },
        ...
    ],
    'article2': [
        {
            "project": "en.wikipedia", 
            "article": article2,
            "granularity": "monthly",
            "timestamp": month1_article2,
            "agent": "user",
            "views" : num_views_1_article2
        },    
        {
            "project": "en.wikipedia", 
            "article": article2,
            "granularity": "monthly",
            "timestamp": month1_article1,
            "agent": "user",
            "views" : num_views_2_article2
        },
        ...
    ]
}
```

Here month1_article1 is the first month of article1 and month2_article2 is the second month of article2. The format is mentioned above in the structure. The same with views num_views_1_article1 is the number of views of article1 for the first month and num_views_2_article1 is the number of views of article1 for the second month and so on.

# Images of graphs acquired from the code outputs:

The code also outputs 3 Time Series graphs of which I have taken screenshots of, the three files for that in this folder would be:
1) Graph1.png
2) Graph2.png
3) Graph3.png

The naming conventions are the same as in the notebook which would make it easier to refer to and to reproduce it.

# Considerations with the Data:

- It is important to note that data for every day from the start date to the end date which you are trying to extract the data from will not necessarily be available. There are a lot of days for which the data is not present. You could refer to the outputs of the DataFrame in the notebook (512_HW1_code.ipynb) of the data that we acquire that there are many NA values.

- Another thing to keep in mind is that in the request_pageviews_per_article function from the example notebook where the urllib.parse.quote function is used I have added an extra parameter safe = '' so that there are no issues while acquiring the data. In the example notebook the code does not add that parameter and based on the articles that you are trying to extract the data for you might run into issues such as 'KeyError' so I would recommend that you add that parameter.



