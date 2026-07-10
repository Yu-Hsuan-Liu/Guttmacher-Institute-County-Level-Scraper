# Guttmacher Institute County-Level Scraper

A Selenium-based tool that downloads county-level data from the [Guttmacher Institute Data Center](https://data.guttmacher.org/) for every county in the United States, then combines the per-state downloads into a single county-level dataset.

## What the code does

1. Reads `data/FIPS.csv`, a lookup table of county FIPS codes with county names and state abbreviations.
2. For each state, joins that state's county FIPS codes into a Data Center table URL of the form `https://data.guttmacher.org/counties/table?county=<FIPS+FIPS+...>&dataset=data&state=<ST>&topics=...`.
3. Opens each URL in a Chrome browser driven by Selenium and clicks the export button to download the table as a CSV file. Sleep intervals between requests keep the request rate low.
4. After the user moves the downloaded files into a `GUTTMACHER/` folder, the final cells read every file, concatenate them, and pivot the long-format data (`county_id`, `measure_name`, `datum`) into a wide table with one row per county and one column per measure.
5. Writes the combined table to `abortion_clinics.csv`.

## Data files

- `data/FIPS.csv`: input lookup of county FIPS codes (`FIPS`, `Name`, `State`).
- `data/abortion_clinics.csv`: example output. Despite the filename, the included file holds county-level counts of publicly funded family planning services: numbers of Planned Parenthood clinics, federally qualified health centers, health department clinics, hospital-based and other clinics, and female contraceptive clients served at each clinic type. The measures returned depend on the topic codes in the URL, so other Data Center topics can be collected the same way.

## Requirements

- Python 3
- Google Chrome and a matching ChromeDriver
- Packages: `selenium`, `pandas`, `numpy`, `beautifulsoup4`, `requests`

Note: the notebook was written against the Selenium 3 API (`find_element_by_xpath`). Selenium 4 users need to replace these calls with `find_element(By.XPATH, ...)`. The ChromeDriver path in the first cell should point to your local installation.

## Usage

1. Open `GUTTMACHER_INSTITUTE_WEB_SCRAPER.ipynb` and update the ChromeDriver path in the first cell.
2. Adjust the `topics=` parameter in the URL to the Data Center topics you need, and point the `pd.read_csv` call to `data/FIPS.csv`.
3. Run the download loop. One CSV per state arrives in the browser download folder.
4. Move the downloaded files into a folder named `GUTTMACHER` next to the notebook.
5. Run the concatenation cells to produce the combined county-level CSV.

## Repository structure

```
GUTTMACHER_INSTITUTE_WEB_SCRAPER.ipynb   Main notebook: download loop and file combination
data/FIPS.csv                            County FIPS lookup (input)
data/abortion_clinics.csv                Example combined output
README.md                                This file
LICENSE                                  MIT license
```

## Caveats

The scraper depends on the export button's XPath on the Data Center table page. If the site layout changes, the XPath needs to be updated. Please respect the Guttmacher Institute's terms of use and keep the built-in delays between requests.
