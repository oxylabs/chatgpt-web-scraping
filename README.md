# How to Use ChatGPT for Web Scraping in 2024

Follow this article to learn how to use ChatGPT for developing fully-functional Python web scrapers. You'll also find out some important tips and tricks to improve the quality of a scraper’s code.


Before moving to the actual topic, let’s briefly introduce our demo target for this tutorial. We will extract data from the Oxylabs Sandbox, a dummy e-commerce store that maintains video game listings in several categories. Here is what the landing page of the store looks like:



Now, let’s delve into the steps required to scrape data from this webpage using ChatGPT.


## 1. Create a ChatGPT Account

Visit ChatGPT’s [login page](https://chat.openai.com/auth/login) and hit Sign-up. You also have the option to sign up using your Google account. On successful sign-up, you will be redirected to the chat window. You can initiate a chat by entering your query in the text field.

## 2. Locate the elements to scrape

Before prompting ChatGPT, let’s first locate the elements we need to extract from the target page. Assume that we need only the video game **titles** and **prices**.

- Right-click one of the game titles and select `Inspect`. This will open the HTML code for this element in the Developer Tools window.

- Right-click the element and select `Copy selector` with the game title in it. The following figure explains it all:




Write down the selector and repeat the same to find the selector for the price element.


## 3. Prepare the ChatGPT prompt

The prompt should be well-explained, specifying the code’s programming language, tools and libraries to be used, element selectors, output, and any special instructions the code must comply with. Here is a sample prompt that you can use to create a web scraper using Python and & BeautifulSoup:

```markdown
Write a web scraper using Python and BeautifulSoup.


Sample Target: https://sandbox.oxylabs.io/products 

Rationale: Scrape the video game titles and prices of all the games on the target page.


CSS selectors are as follows:

1. Title: #__next > main > div > div > div > div:nth-child(2) > div > div:nth-child(1) > a.card-header.css-o171kl.eag3qlw2 > h4

2. Price: #__next > main > div > div > div > div:nth-child(2) > div > div:nth-child(1) > div.price-wrapper.css-li4v8k.eag3qlw4


Output: Save all the Titles and Prices for all the video games in a CSV file


Additional Instructions: Handle character encoding and remove undesirable symbols in the output CSV.
```
Notice we have provided CSS selectors for prices and titles that we copied in the earlier step. 

> [!TIP]
> The scraped data might contain a few odd characters if not handled properly due to encoding issues. It happens when the web page and the Python script interpret the character encoding differently. Therefore, we can add relevant Additional Instructions to avoid these encoding issues.


## 4. Review the code

Once ChatGPT replies with a code, it’s always a good idea to review it first. Just overview the code if it seems to do what you want it to do. Double-check if it uses some libraries or packages you may wish to exclude. If you find any inherent glitches, write back to ChatGPT and ask to generate the code again.


## 5. Execute and Test

Copy the ChatGPT-generated code and check if it is executing fine. Here is the code that ChatGPT generated for us:

```python
import requests
from bs4 import BeautifulSoup
import csv

# URL of the target page
url = "https://sandbox.oxylabs.io/products"

# Send a GET request to the URL
response = requests.get(url)

# Parse the HTML content using BeautifulSoup
soup = BeautifulSoup(response.content, "html.parser")

# CSS selectors for title and price
title_selector = "a.card-header h4"
price_selector = "div.price-wrapper"

# Find all game titles and prices
titles = soup.select(title_selector)
prices = soup.select(price_selector)

# Create a list to store the data
data = []

# Iterate through the titles and prices
for title, price in zip(titles, prices):
    game_title = title.get_text(strip=True)
    game_price = price.get_text(strip=True)
    data.append((game_title, game_price))

# Define the CSV filename
filename = "game_data.csv"

# Write the data to a CSV file
with open(filename, "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(["Title", "Price"])  # Write header
    for game in data:
        writer.writerow(game)

print(f"Data scraped successfully and saved to '{filename}'.")
```

> [!NOTE]
> Remember to install the `BeautifulSoup` library and  `requests` library before running the code. This can be done by opening the terminal and running:

```bash
pip install requests beautifulsoup4
```

Here is the snippet of the output CSV file generated after executing the code:



