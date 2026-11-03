# French Company Search API — SIRENE, SIREN/SIRET — Python, Node.js & cURL examples

Search **every company registered in France** through the official government API «Recherche d'entreprises» (INSEE Sirene + RNE): filter by **NAF / APE code, department, postal code, region, headcount, PME/ETI/GE, legal form, revenue, RGE / Qualiopi labels**, or paste **SIREN / SIRET numbers** to enrich them. You get SIREN, SIRET, legal and trade names, activity, headcount, revenue and net income, **directors**, head office and establishment addresses with GPS and the **intra-EU VAT number**. Searches over the API's 10,000-result limit are split automatically. Companies that opted out of commercial prospecting are skipped by default. **$1.50 per 1,000 rows**; not-found numbers are free.

Hosted on Apify: **[French Company Search API — SIRENE, SIREN/SIRET](https://apify.com/tidydata/french-company-search?fpr=wtg6qt)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/french-company-search").call(run_input={
    "nafCodes": [
        "62.01Z"
    ],
    "departments": [
        "69"
    ],
    "employeeRanges": [
        "11",
        "12"
    ],
    "maxResults": 100
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['siren', 'name', 'nafLabel', 'city', 'employees', 'revenue', 'directorsText', 'vatNumber']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/french-company-search').call({"sirenOrSiret": ["552032534", "403052111"]});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~french-company-search/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"sirenOrSiret": ["552032534", "403052111"]}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper?fpr=wtg6qt) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper?fpr=wtg6qt) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper?fpr=wtg6qt) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor?fpr=wtg6qt) · [Google News](https://apify.com/tidydata/google-news-scraper?fpr=wtg6qt) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper?fpr=wtg6qt) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets?fpr=wtg6qt) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.

---
<sub>Links to apify.com include our affiliate ID: if you start a paid Apify plan through them, Apify pays TidyData a commission at no extra cost to you.</sub>
