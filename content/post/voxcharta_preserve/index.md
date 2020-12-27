---
title: Preserving your voting records on Vox Charta with Python

date: 2020-12-26

tags:
- Vox Charta
- arXiv
- Astronomy
- Digital Preservation
- Python
- Hacking
- BeautifulSoup
- pandas
- GitHub
- Web Scraping
- Benty-Fields

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
- icon: github
  icon_pack: fab
  name: GitHub
  url: https://www.github.com/astrochun/voxcharta-my-voting-record

---

It's been a few months since I have done a tech/hacking post. I figured it might
be worthwhile to do a quick one on the recent web scraping work that I did.

[Vox Charta](https://voxcharta.org), a web application that allowed astronomers
to vote and discuss about the latest manuscripts made available on
[arXiv](https://arxiv.org), was conceived in 2009 by
[James Guillochon](https://github.com/guillochon).
It was used by thousands of astronomers internationally as there was no
other options available for several years. Unfortunately, it was
[announced](https://twitter.com/astrocrash/status/1287053013589385217)
that the service would be sunsetted on December 31, 2020.

Personally I used Vox Charta since 2011 and have utilized its voting method to
record astronomy papers of interest to me. When I heard the news of the sunset
I immediately knew that I needed to #preserve my voting records. There were
over 2,000 papers of interest to me!

So I went forth and built a simple Python software to extract that information.
The "My Voting Records" content was stored in HTML, so it made sense to use
[`BeautifulSoup`](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
for extraction! It was quite fun to solve this problem; this was my first
time building a web scraping tool. Loading the content was pretty straightforward:

```python
import re
from bs4 import BeautifulSoup
import json
import pandas as pd

class Extract:
    def __init__(self, filename, json_outfile, csv_outfile):
    
            self.log = log
            self.filename = filename
            self.json_outfile = json_outfile
            self.csv_outfile = csv_outfile
    
            self.content = self.import_data()

    def import_data(self):
        """Import data"""

        with open(self.filename, 'r') as f:
            content = f.read()
        f.close()

        return content

    def soup_it(self):
        """Construct BeautifulSoup data structure"""

        page_content = BeautifulSoup(self.content, "html.parser")

        return page_content
```

Because the exported HTML was not as well structured, some trial and error
was needed to figure out how to extract specific content. In the end, I was
able to scrape the arXiv ID, title, author list, author affiliation, abstract,
arXiv categories, links, comments, and many other information. The method
to retrieve the voting records is too long to put it here, but you can see
it [here](https://github.com/astrochun/voxcharta-my-voting-record/blob/main/voxcharta_my_voting_record/extract.py#L77-L163).

Then, I added the ability to export the content to two machine-readable forms.

```python
    def export_data(self, records_dict):
        """Write JSON and csv files"""

        with open(self.json_outfile, 'w') as outfile:
            json.dump(records_dict, outfile, indent=4)

        df = pd.DataFrame.from_dict(records_dict, orient='index')
        df.to_csv(self.csv_outfile, index=False)

        # Write arxiv_id list
        arxiv_outfile = self.csv_outfile.replace('.csv', '_arxiv.txt')
        clean_df = df.loc[(df['arxiv_id'] != '') & (df['arxiv_id'].notna())]
        clean_df.to_csv(arxiv_outfile, index=False, header=False,
                        columns=['arxiv_id'])
``` 

As you can see, two types of files are provided: JSON and
CSV. The latter is done with [`pandas`](https://pandas.pydata.org/).
An example of the exported JSON output is available below.

{{< figure src="voxcharta_json.jpg" lightbox="true" width="100%"
    title="" >}}

I then provided my voting records to the maintainers of the 
[Benty-Fields](https://www.benty-fields.com/) web application. It
has many features that are similar to Vox Charta by allowing for
discussions and voting. In the end, those records were transferred
over!

{{< figure src="benty_fields.png" lightbox="true" width="100%"
    title="" >}}

If you're interested in using this code, it is available
[here](https://github.com/astrochun/voxcharta-my-voting-record).
The code is made available under a
[GNU GPL-3.0](https://github.com/astrochun/voxcharta-my-voting-record/blob/main/LICENSE)
License so other astronomers can preserve their precious Vox Charta records.

The README.md provides more details, but here are some quick
installation and execution steps:

1. Download the complete webpage view from the "My Voting Record" page before
   the site is permanently down

2. Clone the repo:
   ```
   $ git clone https://github.com/astrochun/voxcharta-my-voting-record
   ```

3. Build it (preferably in a contained environment)
   ```
   $ cd voxcharta-my-voting-record
   $ (sudo) python setup.py install
   ```

4. You can then easily execute the main script:
   ```
   $ vox_run -f myvotingrecords.htm
   ```

Many thanks to James Guillochon for being the selfless maintainer of Vox
Charta for over a decade. I'm also thankful to the Benty-Fields maintainers
for their help.

If you found this __open-source__ software useful, consider sponsoring me
through GitHub.
<iframe src="https://github.com/sponsors/astrochun/button"
title="Sponsor astrochun" height="35" width="116" style="border: 0;">
</iframe>
