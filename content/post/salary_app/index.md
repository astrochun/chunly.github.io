---
title: Building a streamlit app to serve UArizona salary data and deploying it on Heroku

date: 2021-07-11

tags:
- Python
- Heroku
- streamlit
- pandas
- bokeh
- Salary data
- Hacking

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""

links:
- icon: chrome
  icon_pack: fab
  name: Check out the app!
  url: https://sapp4ua.herokuapp.com

- icon: github
  icon_pack: fab
  name: Code
  url: https://www.github.com/astrochun/uarizona-salary-app

- icon: heart
  icon_pack: fas
  name: Sponsor
  url: https://github.com/sponsors/astrochun
  
---

## Motivation 
The [Daily Wildcat](https://www.wildcat.arizona.edu/) of the
[University of Arizona](https://arizona.edu) (UArizona) has routinely provided
the salary of UArizona employees for nearly a decade through public requests.
While this allowed for the easy viewing of information for an individual,
the analysis from the Daily Wildcat was rather limited. As such, I wanted to
build a data visualization app that allow anyone to explore the data for
100% transparency.

## How
The salary data were available as machine-readable CSV files, so it made sense
to build a Python application that uses `pandas` to extract, transform, and
load (ETL) the data. I heard about `streamlit`, a Python web framework that
would make it faster to build such an app. Essentially `streamlit` provides a
RESTful application programming interface (API) with a front-end layer that
interacts with Python scripts that do data analysis and visualization. It
took me about a weekend to understand `streamlit` and its "widgets", which
are interactive tools that could be used to perform ETL actions with the data.
From there, I put together the software to load the data and provide different
"data views" that were different interactions with the data. The code is
available on [GitHub](https://www.github.com/astrochun/uarizona-salary-app)
under an MIT License. There are many resources and blogs about building a
`streamlit`, so I won't go in details here, but I will illustrate some
aspects:

Loading in data can be simple. 

```python

import pandas as pd
import streamlit as st

# List of fiscal years
FY_LIST = [
    'FY2019-20', 'FY2018-19', 'FY2017-18', 'FY2016-17',
    'FY2014-15', 'FY2013-14', 'FY2011-12',
]


@st.cache
def load_data():

    file_id = {
        'FY2019-20': '1d2l29_T-mOh05bglPlwAFlzeV1PIkRXd',
        'FY2018-19': '1paxrUyW1wZuK3bjSL_L7ckKEC6xslZJe',
        'FY2017-18': '1AnRaPpbRTLVyqdeqe6vkPMYgbNnw9zia',
        'FY2016-17': '1rXBuuXit5oWKtfnA05gsNtsWAyESeIs2',
        'FY2014-15': '1ZANVDr6Kw40MJYiOENWbLMTFEMWyf7f4',
        'FY2013-14': '1rQ8A2CIVhDYu0lESKVh72h6VUd8gIEFl',
        'FY2011-12': '1fQOzEHiOvc_H1NcLMlK3KVV1DJkRbRuX',
    }

    data_dict = {}
    for year in FY_LIST:
        data_dict[year] = pd.read_csv(
            f'https://drive.google.com/uc?id={file_id[year]}'
        )

    return data_dict


def main():

    # Load data
    data_dict = load_data()
```

The above code loads every previous year's of available data into a `dict`
of `pandas` DataFrames. The `st.cache` decorator ensures that the data is
loaded into memory once. This ensures a shorter response time when it's
deployed and users are accessing the app frequently. Here, I made the data
publicly available in Google Drive. I could have gone with an SQL server,
but given the  small record sizes (~10,000 records), and the reliability
of Google Drive, this worked well for an MVP.

To select from the data, I utilized `streamlit` sidebar widgets to add the
layer for selection. For example, the following selects the data for one
fiscal year:

```python
def select_fiscal_year() -> str:
    """Sidebar widget to select fiscal year"""

    st.sidebar.markdown('### Select fiscal year:')
    fy_select = st.sidebar.selectbox('', FY_LIST, index=0).split(' ')[0]

    return fy_select


def main():
    # Load data
    data_dict = load_data()

    fy_select = select_fiscal_year()

    # Select dataframe
    df = data_dict[fy_select]
    st.sidebar.text(f"{fy_select} data imported!")
```

This results in something that looks like this:

{{< figure src="screenshot1.png" lightbox="true" width="40%"
    title="Sidebar tool selection for fiscal year" >}}

As discussed earlier, interactions with the data is done with different views.
Currently, I have five of them:
1. Trends: General facts and numbers (e.g. number of employees, salary budget,
   etc.), for each fiscal year
2. Salary Summary: Statistics and percentile salary data, includes salary histogram
3. Highest Earners: Extract data above a minimum salary
4. College/Division Data: Similar to Salary Summary but extracted for each college(s)/division(s)
5. Department Data: Similar to Salary Summary but extracted for each department(s)

This can easily be done on the side and then the main page is loaded

```python
DATA_VIEWS = [
    'About', 'Trends', 'Salary Summary', 'Highest Earners',
    'College/Division Data', 'Department Data'
]

def select_data_view() -> str:
    """Sidebar widget to select your data view"""

    st.sidebar.markdown('### Select your data view:')
    view_select = st.sidebar.selectbox('', DATA_VIEWS, index=0)

    return view_select


def main():
    # Sidebar, select data view
    view_select = select_data_view()

    if view_select == 'About':
        views.about_page()

    if view_select == 'Trends':
        views.trends_page(data_dict, pay_norm)

    if view_select == 'Salary Summary':
        views.salary_summary_page(df, pay_norm)

    if view_select == 'Highest Earners':
        views.highest_earners_page(df)

    # Select by College Name
    if view_select == 'College/Division Data':
        views.subset_select_data_page(df, COLLEGE_NAME, 'college',
                                      pay_norm)

    # Select by Department Name
    if view_select == 'Department Data':
        views.subset_select_data_page(df, 'Department', 'department',
                                      pay_norm)

```

Here `views` is a module and each of its function displays different
content in the main page. Here's a screenshot for the Salary Summary page

{{< figure src="screenshot2.png" lightbox="true" width="100%"
    title="One of the data views from `sapp4ua`." >}}


If you found this __open-source__ software useful, consider sponsoring me
through GitHub.
<iframe src="https://github.com/sponsors/astrochun/button"
title="Sponsor astrochun" height="35" width="116" style="border: 0;">
</iframe>
