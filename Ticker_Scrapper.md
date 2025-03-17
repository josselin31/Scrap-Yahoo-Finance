#Libraries
```bash
!pip install selenium pandas tqdm
!apt-get update
!apt-get install -y chromium-chromedriver
!pip install selenium --upgrade
```
---
#Dependencies downloaded
```bash
!apt-get update
!apt-get install -y wget unzip
!wget -q -O google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
!dpkg -i google-chrome-stable_current_amd64.deb || apt-get -fy install
!wget -q -O chromedriver.zip https://storage.googleapis.com/chrome-for-testing-public/$(google-chrome --version | awk '{print $3}')/linux64/chromedriver-linux64.zip
!unzip -o chromedriver.zip
!mv chromedriver-linux64/chromedriver /usr/bin/chromedriver
!chmod +x /usr/bin/chromedriver
```
---
#Test Dependencies
```bash
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options

# Configurer les options Chrome
chrome_options = Options()
chrome_options.add_argument("--headless")  # Mode sans interface graphique
chrome_options.add_argument("--no-sandbox")
chrome_options.add_argument("--disable-dev-shm-usage")

# Démarrer Chrome avec le bon chemin
service = Service("/usr/bin/chromedriver")
driver = webdriver.Chrome(service=service, options=chrome_options)

# Tester si Selenium fonctionne
driver.get("https://www.google.com")
print(driver.title)  # Doit afficher "Google"
driver.quit()
```
---
#No Botting detection Scrapper
##tested on 31626 ticker sample
```bash
import pandas as pd
import time
import random
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from tqdm import tqdm

# Configure Selenium for headless execution
chrome_options = Options()
chrome_options.add_argument("--headless")  # Headless mode (no UI)
chrome_options.add_argument("--no-sandbox")
chrome_options.add_argument("--disable-dev-shm-usage")
chrome_options.add_argument("--window-size=1920,1080")

# Add a User-Agent to hide Selenium
chrome_options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36")

# Disable automation detection
chrome_options.add_experimental_option("excludeSwitches", ["enable-automation"])
chrome_options.add_experimental_option("useAutomationExtension", False)

# Initialize the browser
service = Service("/usr/bin/chromedriver")
driver = webdriver.Chrome(service=service, options=chrome_options)
driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined})")

# Load the CSV file
df = pd.read_csv("a.csv")

# Add columns to store results
df["group"] = ""
df["name"] = ""

# Define XPath expressions
xpath_group = '//*[@id="nimbus-app"]/section/section/section/article/section[1]/div[1]/span/span[1]'
xpath_name = '//*[@id="nimbus-app"]/section/section/section/article/section[1]/div[1]/div/div[1]/section/h1]'

# Progress bar with remaining time estimation
start_time = time.time()

for index, row in tqdm(df.iterrows(), total=len(df), desc="Scraping Yahoo Finance"):
    ticker = row[0]  # Assuming column 0 contains the ticker
    url = f"https://fr.finance.yahoo.com/quote/{ticker}"

    try:
        driver.get(url)
        time.sleep(random.uniform(2, 5))  # Random delay to avoid detection

        # Extract the group text
        try:
            group = driver.find_element(By.XPATH, xpath_group).text
        except:
            group = "N/A"

        # Extract the name text
        try:
            name = driver.find_element(By.XPATH, xpath_name).text
        except:
            name = "N/A"

        # Update the DataFrame
        df.at[index, "group"] = group
        df.at[index, "name"] = name

        # Estimate the remaining time
        elapsed_time = time.time() - start_time
        avg_time_per_ticker = elapsed_time / (index + 1)
        remaining_time = avg_time_per_ticker * (len(df) - (index + 1))
        tqdm.write(f"Remaining time estimate: {remaining_time:.2f} seconds")

    except Exception as e:
        print(f"Error with {ticker}: {e}")

# Save the results
df.to_csv("scraping_results.csv", index=False)

# Close the browser
driver.quit()

print("Scraping completed. File saved as 'scraping_results.csv'.")
```
