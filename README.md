# How to Scrape Google Search Results: Python Tutorial

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

In this tutorial, we showcase how to scrape public Google data with Python and Oxylabs [SERP Scraper API](https://oxylabs.io/products/scraper-api/serp) (a part of Web Scraper API), which requires a subscription or **a free trial**.

  * [What is a Google SERP?](#what-is-a-google-serp)
  * [Is it legal to scrape Google results?](#is-it-legal-to-scrape-google-results)
  * [Scraping public Google data with Python and Oxylabs Scraper API](#scraping-public-google-data-with-python-and-oxylabs-scraper-api)
  * [Set up a payload and send a POST request](#set-up-a-payload-and-send-a-post-request)
    + [Customizing query parameters](#customizing-query-parameters)
    + [Basic parameters](#basic-parameters)
  * [Location query parameters](#location-query-parameters)
    + [Controlling the number of results](#controlling-the-number-of-results)
    + [Python code for scraping Google search data](#python-code-for-scraping-google-search-data)
    + [Export scraped data to a CSV](#export-scraped-data-to-a-csv)
    + [Handling errors and exceptions](#handling-errors-and-exceptions)

## What is a Google SERP? 

Upon any discussion of scraping Google search results, you’ll likely run into the “SERP” abbreviation. SERP stands for Search Engine Results Page; it’s the page you get after entering a query into the search bar. SERPs contain various features and elements, such as:

1. Featured snippets
2. Paid ads
3. Video carousel
4. People also ask
5. Local pack
6. Related searches

## Is it legal to scrape Google results? 
The legality of scraping Google search data is largely discussed in the scraping field. As a matter of fact, scraping publicly available data on the internet – including Google SERP data – is legal. However, it may vary from one situation to another, so it’s best to seek legal advice about your specific case. 

## Scraping public Google data with Python and Oxylabs Scraper API 

1. Install required Python libraries
To follow this guide on scraping Google search results, you’ll need the following: 

- Credentials for Oxylabs' [SERP Scraper API](https://oxylabs.io/products/scraper-api/serp) – you can get a 7-day free trial by registering on the [dashboard](https://dashboard.oxylabs.io/);
- Python;
- Requests library.

First, sign up for Oxylabs' Google Search Results API and save your `username` and `password`. 

Then, download and install Python 3.8 or above from the [python.org](https://www.python.org/) website. Finally, install the [Request library](https://pypi.org/project/requests/) by using the following command: 

`$python3 -m pip install requests`

If you’re using Windows, choose Python instead of Python3. The rest of the command remains the same: 

`d:\amazon>python -m pip install requests`

## Set up a payload and send a POST request

Create a new file and enter the following code:

```
import requests
from pprint import pprint

payload = {
    'source': 'google',
    'url': 'https://www.google.com/search?hl=en&q=newton'  # search for newton
}

response = requests.request(
    'POST',
    'https://realtime.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload,
)

pprint(response.json())
```

Here’s what the result should look like:

```
{
    "results": [
        {
            "content": "<!doctype html><html>...</html>",
            "created_at": "YYYY-DD-MM HH:MM:SS",
            "updated_at": "YYYY-DD-MM HH:MM:SS",
            "page": 1,
            "url": "https://www.google.com/search?hl=en&q=newton",
            "job_id": "1234567890123456789",
            "status_code": 200
        }
    ]
}
```

Notice how the `url` in the payload dictionary is a Google search results page. In this example, the keyword is `newton`.

As you can see, the query is executed and the page result in HTML is returned in the content key of the response.

### Customizing query parameters
Let's review the payload dictionary from the above example for scraping Google search data.

```
payload = {
    'source': 'google',
    'url': 'https://www.google.com/search?hl=en&q=newton'
}
```

The dictionary keys are parameters used to inform Google Scraper API about required customization. 

The first parameter is the `source`, which is really important because it sets the scraper we’re going to use. 

The default value is `Google` – when you use it, you can set the url as any Google search page, and all the other parameters will be extracted from the URL. 

Although in this guide we’ll be using the `google_search` parameter, there's many others: `google_ads`, `google_hotels`, `google_images`, `google_suggest`, and more (full list [here](https://developers.oxylabs.io/scraper-apis/web-scraper-api/google).
 
Keep in mind that if you set the source as `google_search`, you cannot use the `url` parameter. Luckily, you can use several different parameters for acquiring public Google SERP data without having to create multiple URLs (more on that in the next paragraph.) 

### Basic parameters
We’ll build the payload by adding the parameters one by one. First, begin with setting the source as `google_search`.

```
payload = {
    'source': 'google_search',
}
```

Now, let’s add `query` – a crucial parameter that determines what search results you’ll be retrieving. In our example, we’ll use `newton` as our search query. At this stage, the payload dictionary looks like this:

```
payload = {
    'source': 'google_search',
    'query': 'newton',
}
```

That said, `google_search` and query are the two essential parameters for scraping public Google search data. If you want the API to return Google search results at this stage, you can use `payload`. Now, let’s move to the next parameter. 

## Location query parameters
You can work with a domain parameter if you want to use a localized domain – for example, `'domain':'de'` will fetch results from google.de. If you want to see the results from Germany, use the `geo_location` parameter— `'geo_location':'Germany'`. See the [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/geo-location#google) for the `geo_location` parameter to learn more about the correct values.

Also, here’s what changing the locale parameter looks like: 

```
payload = {
'source':'google_search',
'query':'newton',
'domain':'de' ,
'geo_location': 'Germany',
'locale' : 'en-us'
}
```

To learn more about the potential values of the locale parameter, check the [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/domain-locale-results-language#locale-1), as well.  

If you send the above payload, you’ll receive search results in American English from google.de, just like anyone physically located in Germany would.

### Controlling the number of results

By default, you’ll see the first ten results from the first page. If you want to customize this, you can use these parameters: `start_page`, `pages`, and `limit`.

The `start_page` parameter determines which page of search results to return. The `pages` parameter specifies the number of pages. Finally, the `limit parameter` sets the number of results on each page.

For example, the following set of parameters fetch results from pages 11 and 12 of the search engine results, with 20 results on each page:

```
payload = {
    'start_page': 11,
    'pages': 2,
    'limit': 20,
    ...  # other parameters
}
```

Apart from the search parameters we’ve covered so far, there are a few more you can use to fine-tune your results – see our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/google/search#request-parameter-values) on collecting public Google Search data. 

### Python code for scraping Google search data

Now, let’s put together everything we’ve learned so far – here’s what the final script with the shoes keyword looks like:

```
import requests
from pprint import pprint
payload = {
    'source': 'google_search',
    'query': 'shoes',
    'domain': 'de',
    'geo_location': 'Germany',
    'locale': 'en-us',
    'parse': True,
    'start_page': 1,
    'pages': 5,
    'limit': 10,
}


# Get response.
response = requests.request(
    'POST',
    'https://realtime.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload,
)


if response.status_code != 200:
    print("Error - ", response.json())
    exit(-1)


pprint(response.json())
```

### Export scraped data to a CSV
One of the best Google Scraper API features is the ability to parse an HTML page into JSON. For that, you don't need to use BeautifulSoup or any other library – just send the parse parameter as True. 

Here is a sample payload:

```
payload = {
    'source': 'google_search',
    'query': 'adidas',
    'parse': True,
}
```

When sent to the Google Scraper API, this payload will return the results in JSON. To see a detailed JSON data structure, see our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/google/search#structured-data). 

The key highlights: 

- The results are in the dedicated results list. Here, each page gets a new entry.
- Each result contains the content in a dictionary key named content. 
- The actual results are in the results key.

Note that there’s a `job_id` in the results.

The easiest way to save the data is by using the Pandas library, since it can normalize JSON quite effectively.

```
import pandas as pd
...
data = response.json()
df = pd.json_normalize(data['results'])
df.to_csv('export.csv', index=False)
```

Alternatively, you can also take note of the `job_id` and send a GET request to the following URL, along with your credentials.

```
http://data.oxylabs.io/v1/queries/{job_id}/results/normalized?format=csv
```

### Handling errors and exceptions
When scraping Google, you can run into several challenges: network issues,  invalid query parameters, or API quota limitations.

To handle these, you can use try-except blocks in your code. For example, if an error occurs when sending the API request, you can catch the exception and print an error message:

```
try:
    response = requests.request(
        'POST',
        'https://realtime.oxylabs.io/v1/queries',
        auth=('USERNAME', 'PASSWORD'),
        json=payload,
    )
except requests.exceptions.RequestException as e:
    print("Error:", e)
```

If you send an invalid parameter, Google Scraper API will return the 400 response code.

To catch these errors, check the status code: 

```
if response.status_code != 200:
    print("Error - ", response.json())
```

Looking to scrape data from other Google sources? [Google Sheets for Basic Web Scraping](https://github.com/oxylabs/web-scraping-google-sheets), [How to Scrape Google Shopping Results](https://github.com/oxylabs/scrape-google-shopping), [Google Play Scraper](https://github.com/oxylabs/google-play-scraper), [How To Scrape Google Jobs](https://github.com/oxylabs/how-to-scrape-google-jobs), [Google News Scrpaer](https://github.com/oxylabs/google-news-scraper), [How to Scrape Google Scholar](https://github.com/oxylabs/how-to-scrape-google-scholar), [How to Scrape Google Flights with Python](https://github.com/oxylabs/how-to-scrape-google-flights),  [Scrape Google Search Results](https://github.com/oxylabs/scrape-google-python), [Scrape Google Trends](https://github.com/oxylabs/how-to-scrape-google-trends)
