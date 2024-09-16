# How to Use ChatGPT for Web Scraping in 2024

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

[![](https://dcbadge.vercel.app/api/server/eWsVUJrnG5)](https://discord.gg/GbxmdGhZjq)

- [1. Create a ChatGPT Account](#1-create-a-chatgpt-account)
- [2. Locate the elements to scrape](#2-locate-the-elements-to-scrape)
- [3. Prepare the ChatGPT prompt](#3-prepare-the-chatgpt-prompt)
- [4. Review the code](#4-review-the-code)
- [5. Execute and test](#5-execute-and-test)
- [Tips and tricks for using ChatGPT](#tips-and-tricks-for-using-chatgpt)
  * [1. Get code editing assistance](#1-get-code-editing-assistance)
  * [2. Check for errors](#2-check-for-errors)
  * [3. Code Optimization Assistance](#3-code-optimization-assistance)
  * [4. Handle dynamic content](#4-handle-dynamic-content)
- [Overcome web scraping blocks with a dedicated API](#overcome-web-scraping-blocks-with-a-dedicated-api)


Follow this article to learn how to use [ChatGPT](https://chat.openai.com/) for developing fully-functional Python web scrapers. You'll also find out some important tips and tricks to improve the quality of a scraper’s code.

Before moving to the actual topic, let’s briefly introduce our demo target for this tutorial. We'll extract data from the [Oxylabs Scraping Sandbox](https://sandbox.oxylabs.io/products), a dummy e-commerce store that maintains video game listings in several categories. Here's what the landing page of the store looks like:

![](/images/sandbox.png)

Now, let’s delve into the steps required to scrape data from this webpage using ChatGPT.


## 1. Create a ChatGPT Account

Visit ChatGPT’s [login page](https://chat.openai.com/auth/login) and hit Sign-up. You also have the option to sign up using your Google account. On successful sign-up, you will be redirected to the chat window. You can initiate a chat by entering your query in the text field.

## 2. Locate the elements to scrape

Before prompting ChatGPT, let’s first locate the elements we need to extract from the target page. Assume that we need only the video game **titles** and **prices**.

- Right-click one of the game titles and select `Inspect`. This will open the HTML code for this element in the Developer Tools window.

- Right-click the element and select `Copy selector` with the game title in it. The following figure explains it all:

![](/images/sandbox_dev_tools.png)


Write down the selector and repeat the same to find the selector for the price element.


## 3. Prepare the ChatGPT prompt

The prompt should be well-explained, specifying the code’s programming language, tools and libraries to be used, element selectors, output, and any special instructions the code must comply with. Here's a sample prompt that you can use to create a web scraper using Python and & BeautifulSoup:

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


## 5. Execute and test

Copy the ChatGPT-generated code and check if it's executing fine. Here's the code that ChatGPT generated for us:

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


Here's the snippet of the output CSV file generated after executing the code:

![](/images/scraped_csv.png)

Congratulations! You've just effortlessly scraped the target website. For your convenience, we also prepared this tutorial in a [video format](https://www.youtube.com/watch?v=AUEjBzLJlE4).


## Tips and tricks for using ChatGPT

### 1. Get code editing assistance

Specify the changes you want to make, such as modifying the scraped elements, boosting the effectiveness of the code, or modifying the data extraction procedure. ChatGPT can offer you additional code options or modify suggestions to improve the web scraping process.

### 2. Check for errors

To adhere to coding standards and practices, you can ask ChatGPT to review the code and provide recommendations. You can even paste your code and ask ChatGPT to lint it. You can do so by adding the “lint the code” phrase in the additional instructions of the prompt. 

### 3. Code Optimization Assistance

When it comes to web scraping, efficiency is critical, especially when working with large datasets or challenging web scraping tasks. ChatGPT can provide tips on how to increase the performance of your code. 

You can ask for advice on how to use frameworks and packages that speed up web scraping, use caching techniques, exploit concurrency or parallel processing, and minimize pointless network calls.

### 4. Handle dynamic content

Certain websites produce dynamic content using Javascript libraries or use AJAX requests to produce the content. ChatGPT can help you navigate such complex web content. You can inquire ChatGPT for the techniques to get the dynamic content from such Javascript-rendered pages.

ChatGPT can offer suggestions on using headless browsers, parsing dynamic HTML, or even automating interactions using simulated user actions.

## Overcome web scraping blocks with a dedicated API

Be aware that there are some limitations of using ChatGPT for web scraping. Many websites have implemented strong security measures to block automated scrapers from accessing the sites. Commonly, sites use CAPTCHAs and request rate-limiting to prevent automated scraping. Thereby, simple **ChatGPT-generated scrapers may fail** at these sites. However, [Web Unblocker](https://oxylabs.io/products/web-unblocker) by Oxylabs can help in these scenarios. It's a **paid proxy solution** which you can test using a **1-week free trial** by regsitering a free account on the [dashboard](https://dashboard.oxylabs.io/).

Web Unblocker provides features such as rotating proxies, bypassing CAPTCHAs, managing requests, utilizing a built-in headless browser, etc. Such measures can help minimize the chances of triggering automated bot detection.
