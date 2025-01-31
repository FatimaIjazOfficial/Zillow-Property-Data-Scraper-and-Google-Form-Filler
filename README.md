## Zillow Property Data Scraper and Google Form Filler

### Overview
This project involves scraping property data from a Zillow-like website and automatically filling a Google Form with the scraped data using Selenium.

### Prerequisites
Ensure you have the following installed on your system:
- Python
- BeautifulSoup (`pip install beautifulsoup4`)
- Requests (`pip install requests`)
- Selenium (`pip install selenium`)
- WebDriver for your browser (e.g., `chromedriver` for Chrome)

### Tested Package Versions
- `beautifulsoup4==4.12.2`
- `requests==2.31.0`
- `selenium==4.15.1`

### Steps

#### 1. Scrape the Links, Addresses, and Prices of Rental Properties

This script scrapes the property data from a Zillow clone website. The data includes property links, addresses, and prices.

**Code:**
```python
from bs4 import BeautifulSoup
import requests

# Headers to mimic a browser visit
header = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.125 Safari/537.36",
    "Accept-Language": "en-GB,en-US;q=0.9,en;q=0.8"
}

# Use our Zillow-Clone website (instead of Zillow.com)
response = requests.get("https://appbrewery.github.io/Zillow-Clone/", headers=header)
data = response.text
soup = BeautifulSoup(data, "html.parser")

# Create a list of all the links on the page using a CSS Selector
all_link_elements = soup.select(".StyledPropertyCardDataWrapper a")
all_links = [link["href"] for link in all_link_elements]
print(f"There are {len(all_links)} links to individual listings in total: \n")
print(all_links)

# Create a list of all the addresses on the page using a CSS Selector
all_address_elements = soup.select(".StyledPropertyCardDataWrapper address")
all_addresses = [address.get_text().replace(" | ", " ").strip() for address in all_address_elements]
print(f"\n After having been cleaned up, the {len(all_addresses)} addresses now look like this: \n")
print(all_addresses)

# Create a list of all the prices on the page using a CSS Selector
all_price_elements = soup.select(".PropertyCardWrapper span")
all_prices = [price.get_text().replace("/mo", "").split("+")[0] for price in all_price_elements if "$" in price.text]
print(f"\n After having been cleaned up, the {len(all_prices)} prices now look like this: \n")
print(all_prices)
```

#### 2. Fill in the Google Form using Selenium

This part of the script automates the process of filling in a Google Form with the scraped data.

**Code:**
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

# Optional - Keep the browser open (helps diagnose issues if the script crashes)
chrome_options = webdriver.ChromeOptions()
chrome_options.add_experimental_option("detach", True)
driver = webdriver.Chrome(options=chrome_options)

for n in range(len(all_links)):
    # TODO: Add the link to your own Google Form
    driver.get("your own google form link")
    time.sleep(2)

    # Use the xpath to select the "short answer" fields in your Google Form.
    address = driver.find_element(By.XPATH, value='your address input box xpath')
    price = driver.find_element(By.XPATH, value='your price input box xpath')
    link = driver.find_element(By.XPATH, value='your link input box xpath')
    submit_button = driver.find_element(By.XPATH, value='your submit button xpath')

    address.send_keys(all_addresses[n])
    price.send_keys(all_prices[n])
    link.send_keys(all_links[n])
    submit_button.click()
```

### Note
- Replace `"your own google form link"` with the actual link to your Google Form.
- Replace `your address input box xpath`, `your price input box xpath`, `your link input box xpath`, and `your submit button xpath` with the actual XPath values of the input fields and submit button in your Google Form.

### Conclusion
This script automates the process of scraping property data and filling out a Google Form with that data. Customize the script as needed for your specific use case and ensure you have the necessary permissions to scrape or automate interactions on the websites you target.
