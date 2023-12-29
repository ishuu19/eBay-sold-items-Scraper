eBay Graded Cards Scraper
This Python script is designed to scrape eBay listings for graded cards and extract relevant information, such as title, price, condition, and sold date. The script utilizes the BeautifulSoup library for HTML parsing and the requests library for making HTTP requests.

How to Use
Set the Number of Pages: Adjust the num_pages variable to specify the number of eBay pages you want to scrape. This determines the extent of your data collection.
python
Copy code
# Set the number of pages you want to scrape
num_pages = 100  # Change this to the desired number of pages
eBay Base URL: The base_url variable contains the eBay search URL. Customize the search parameters to match your criteria. In this example, it searches for "graded cards" with sold and complete listings, displaying 240 items per page.
python
Copy code
# eBay base URL
base_url = 'https://www.ebay.com/sch/i.html?_from=R40&_nkw=graded+cards&_sacat=0&rt=nc&LH_Sold=1&LH_Complete=1&_ipg=240&_pgn={}'
CSV File Setup: The script creates a CSV file named ebay_listings.csv and writes headers for the data fields: Title, Price, Condition, and Sold Date.
python
Copy code
# Create a CSV file and write headers
with open('ebay_listings.csv', 'w', newline='', encoding='utf-8') as csv_file:
    csv_writer = csv.writer(csv_file)
    csv_writer.writerow(['Title', 'Price', 'Condition', 'Sold Date'])
Loop Through Pages: The script loops through the specified number of pages, makes an HTTP request for each page, and extracts relevant information using BeautifulSoup.
python
Copy code
# Loop through pages
for page in range(1, num_pages + 1):
    # Make an HTTP request to the eBay URL for the current page
    url = base_url.format(page)
    response = requests.get(url)

    # Parse the HTML content using BeautifulSoup
    soup = BeautifulSoup(response.text, 'html.parser')
Extract and Write Data: The script extracts data such as titles, prices, conditions, and sold dates, then writes this information to the CSV file.
python
Copy code
# Extract data using BeautifulSoup
titles = [title.text.strip() for title in soup.select('.s-item__title')]
prices = [price.text.strip() for price in soup.select('.s-item__price')]
conditions = [condition.text.strip() for condition in soup.select('.SECONDARY_INFO')]
sold_dates = [sold_date.text.strip() for sold_date in soup.select('.s-item__title--tag')]

# Write data to CSV
for title, price, condition, sold_date in zip(titles, prices, conditions, sold_dates):
    csv_writer.writerow([title, price, condition, sold_date])
# Full Code
python
Copy code
import requests
from bs4 import BeautifulSoup
import csv

# Set the number of pages you want to scrape
num_pages = 100  # Change this to the desired number of pages

# eBay base URL
base_url = 'https://www.ebay.com/sch/i.html?_from=R40&_nkw=graded+cards&_sacat=0&rt=nc&LH_Sold=1&LH_Complete=1&_ipg=240&_pgn={}'

# Create a CSV file and write headers
with open('ebay_listings.csv', 'w', newline='', encoding='utf-8') as csv_file:
    csv_writer = csv.writer(csv_file)
    csv_writer.writerow(['Title', 'Price', 'Condition', 'Sold Date'])

    # Loop through pages
    for page in range(1, num_pages + 1):
        # Make an HTTP request to the eBay URL for the current page
        url = base_url.format(page)
        response = requests.get(url)

        # Parse the HTML content using BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extract data using BeautifulSoup
        titles = [title.text.strip() for title in soup.select('.s-item__title')]
        prices = [price.text.strip() for price in soup.select('.s-item__price')]
        conditions = [condition.text.strip() for condition in soup.select('.SECONDARY_INFO')]
        sold_dates = [sold_date.text.strip() for sold_date in soup.select('.s-item__title--tag')]

        # Write data to CSV
        for title, price, condition, sold_date in zip(titles, prices, conditions, sold_dates):
            csv_writer.writerow([title, price, condition, sold_date])
Feel free to adapt the script to your specific needs!
