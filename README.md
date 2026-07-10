# Guttmacher Institute County-Level Scraper

Downloads county-level tables from the [Guttmacher Data Center](https://data.guttmacher.org/) for every U.S. county, one state at a time, and combines them into a single wide table with one row per county and one column per measure.

The notebook builds each state's table URL from the county FIPS codes in `data/FIPS.csv`, opens it in Chrome through Selenium, and clicks the export button. After the downloads are moved into a `GUTTMACHER` folder, the last cells concatenate and pivot everything into the combined CSV.

`data/abortion_clinics.csv` is an example output. Despite the filename, it holds county counts of publicly funded family planning clinics (Planned Parenthood, federally qualified health centers, health department clinics) and contraceptive clients served; which measures you get depends on the `topics=` parameter in the URL.

Needs Chrome with a matching ChromeDriver. Written against the Selenium 3 API, so update the `find_element_by_xpath` calls if you are on Selenium 4.
