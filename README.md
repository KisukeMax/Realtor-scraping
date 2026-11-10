# Realtor.com Listings Scraper

A Python script that collects property listings from Realtor.com for any ZIP code or location and saves them to a CSV file.

## How it works

Realtor.com is a Next.js site, so every search page ships its results as JSON inside a `<script id="__NEXT_DATA__">` tag. Rather than scraping fragile HTML elements, the script:

1. Requests the search page for the location you enter (`/realestateandhomes-search/<location>`).
2. Parses the `__NEXT_DATA__` JSON with BeautifulSoup and reads `props → pageProps → searchResults → home_search → results`.
3. Finds how many result pages there are from the pagination links, then fetches `/pg-2`, `/pg-3`, and so on.
4. Pulls the address, price, beds, baths, square footage and listing URL out of each property.
5. Writes everything to `output <location>.csv` with pandas.

## Installation

```bash
pip install -r requirements.txt
```

## Usage

```bash
python realtor.py
```

The script asks for a location:

```
Please input zipcode or location - 78701
Found <N> Pages
Page 1 of <N>
...
Data is saved to output 78701.csv and <count> Results Found
```

Locations with spaces work too (for example `Austin, TX`); spaces are converted to the format Realtor.com uses in its URLs.

## Output columns

| Column | Description |
|---|---|
| `address` | Street line, state and ZIP |
| `city` | City |
| `state` | State code |
| `postal_code` | ZIP code |
| `price` | List price (USD) |
| `facts and features` | Beds, baths and square footage, e.g. `3 bds, 2 ba ,1850 sqft` |
| `url` | Link to the listing on Realtor.com |

## Limitations

- Written in 2022 against Realtor.com's page structure at that time. If the site changes its `__NEXT_DATA__` layout or blocks the request, the JSON path in `get_page_status()` or the request headers will need updating.
- No proxies or rate limiting are built in, so keep runs small.

## Disclaimer

For personal and educational use. Respect Realtor.com's terms of use and robots.txt, and don't use the data commercially without permission.
