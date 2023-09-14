# python-candle-forecast
 Adding a simple 5-hour forecast using pmd on top of our python-template.

 We recommend the lightweight miniconda: https://docs.conda.io/projects/miniconda/en/latest/ alongside VSCode with Jupyter & Python plugins.

# Reproduce 

1. Clone this repo. I use GitHub Desktop, you can grab the URL to clone in that app, or use `git clone https://github.com/fsc-data-science/python-candle-forecast.git`
2. Create a txt file called `api_key.txt` and place your API key (you can create one via the API page when logged into Flipside: https://flipsidecrypto.xyz/account/api-keys) 
3. Open miniconda3  and move to your cloned directory `cd YourLocationHere/python-candle-forecast`
4. In Conda create a new environment using the provided environment.yml file: `conda env create -f environment.yml` 

The environment yml includes installs for flipside, numpy, pandas, pmdarima, and plotly, alongside this python-candle-forecast environment name and python 3.10.9

5. Activate the new environment: `conda activate python-candle-forecast` 
6. Run the analysis script `hello-flipside.py` using your `python-candle-forecast` Kernel. If you create an interactive HTML file (`candlestick_chart.html`) it worked! (Open in browser to see the eth volume weighted average price candle chart over the last 30 days).
7. 

8. As you work with your own repos and environments, use `conda env export > environment.yml` to manage new package installations and ensure reproducibility for those who clone your repos!

# Flipside API 

You can generate API keys to bring SQL queries from the data studio into Python here: https://flipsidecrypto.xyz/account/api-keys 
This repo includes the flipside package in the environment for you. Note: API Keys are capped at a free 5,000 query seconds/month.
 So it is recommended you write your queries in the (free) Studio first, and then bring the polished query into Python once it runs as expected.

Relatedly, keep our docs handy as you work with data from Flipside. 

https://docs.flipsidecrypto.com/flipside-api/get-started/run-your-first-query

For simplicity you may want to keep this auto-paginate function handy until it is formally added to the package. You can convert the result to a pandas data frame using `pd.DataFrame()`

```
"""This function will be added to Flipside package after testing, just copy/paste as needed for now"""
def auto_paginate_result(query_result_set, page_size=10000):
    """
    This function auto-paginates a query result to get all the data. It assumes 10,000 rows per page.
    In case of an error, reduce the page size. Uses numpy.
    """
    num_rows = query_result_set.page.totalRows
    page_count = np.ceil(num_rows / page_size).astype(int)
    all_rows = []
    current_page = 1
    while current_page <= page_count:
        results = flipside.get_query_results(
            query_result_set.query_id,
            page_number=current_page,
            page_size=page_size
        )

        if results.records:
            all_rows.extend(results.records)  # Use extend() to add list elements

        current_page += 1  # Increment the current page number

    return all_rows  # Return all_rows in JSON format
```

