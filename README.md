# eBay Graded Cards Scraper

# eBay Graded Cards Scraper with Selenium

This Python script enhances the eBay Graded Cards Scraper by incorporating the Selenium library, allowing for dynamic content loading. With this script, you can easily extract information such as the title, price, condition, and sold date from eBay listings for graded cards.

## How to Use

1. **Set the Number of Pages:** Adjust the `num_pages` variable to specify the number of eBay pages you want to scrape. This determines the extent of your data collection.

    ```python
    # Set the number of pages you want to scrape
    num_pages = 100  # Change this to the desired number of pages
    ```

2. **eBay Base URL and Search Parameters:** The `base_url` variable contains the base eBay URL, and `search_params` is a dictionary with various search parameters. Customize these parameters based on your preferences.

    ```python
    # eBay base URL
    base_url = 'https://www.ebay.com/sch/i.html'
    
    # eBay search parameters
    search_params = {
        '_fsrp': 1,
        '_from': 'R40',
        'LH_Complete': 1,
        'LH_Sold': 1,
        '_nkw': 'graded+cards',
        '_sacat': 0,
        '_oaa': 1,
        '_dcat': 261328,
        '_ipg': 240,
    }
    ```

3. **CSV File Setup:** The script creates a CSV file named `ebay_listings.csv` and writes headers for the data fields: Title, Price, Condition, and Sold Date.

    ```python
    # Create a CSV file and write headers
    with open('ebay_listings.csv', 'w', newline='', encoding='utf-8') as csv_file:
        csv_writer = csv.writer(csv_file)
        csv_writer.writerow(['Title', 'Price', 'Condition', 'Sold Date'])
    ```

4. **Set up Selenium webdriver:** Ensure you have the appropriate webdriver installed (e.g., ChromeDriver) and adjust the `webdriver.Chrome()` line based on your browser and its driver.

    ```python
    # Set up Selenium webdriver
    driver = webdriver.Chrome()  # You may need to adjust this based on your browser and its driver
    ```

5. **Loop Through Pages:** The script utilizes Selenium to load pages dynamically and waits for content to load before parsing.

    ```python
    # Loop through pages
    for page in range(1, num_pages + 1):
        # Add page parameter to search_params
        search_params['_pgn'] = page

        # Construct the URL
        url = f"{base_url}?{('&'.join(f'{k}={v}' for k, v in search_params.items()))}"

        # Use Selenium to load the page and wait for content to load
        driver.get(url)
        WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.CLASS_NAME, 's-item__title')))
    ```

6. **Parse and Extract Data:** The script continues to use BeautifulSoup for HTML parsing and extracts data similarly to the previous version.

    ```python
    # Parse the HTML content using BeautifulSoup
    soup = BeautifulSoup(driver.page_source, 'html.parser')

    # Extract data using BeautifulSoup
    titles = [title.text.strip() for title in soup.select('.s-item__title')]
    prices = [price.text.strip() for price in soup.select('.s-item__price')]
    conditions = [condition.text.strip() for condition in soup.select('.SECONDARY_INFO')]
    sold_dates = [sold_date.text.strip() for sold_date in soup.select('.s-item__title--tag')]
    ```

7. **Write Data to CSV and Close the Selenium webdriver:**
    ```python
    # Write data to CSV
    for title, price, condition, sold_date in zip(titles, prices, conditions, sold_dates):
        csv_writer.writerow([title, price, condition, sold_date])

    # Close the Selenium webdriver
    driver.quit()
    ```

Feel free to customize the script to your specific needs! Happy scraping!

