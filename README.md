# Scrap-Yahoo-Finance
Yahoo Finance Scraper using Selenium only for ticker additional informations


Here’s an attractive and clear README for your GitHub repository:

---

# Yahoo Finance Ticker Scraping Tool

This Python-based tool leverages Selenium to scrape essential data from Yahoo Finance, including asset ticker symbols, full company names, and asset groups. It is designed for users who need to collect and manage basic financial data efficiently.

## Features

- **Scraping Essential Data:** Extracts ticker symbols, company names, and asset groups (stocks, ETFs, etc.).
- **Headless Browser Execution:** Uses Selenium in headless mode for fast, silent execution without the need for a GUI.
- **Progress Tracking:** Displays a progress bar with estimated time remaining during the scraping process.
- **Error Handling:** Includes robust error handling to ensure smooth execution and avoid interruptions due to network issues or missing data.
- **Flexible and Customizable:** Easily configurable to extend functionality or target additional data points.

## Installation

### Requirements

- Python 3.6+
- Selenium
- ChromeDriver (compatible with your version of Chrome)
- TQDM for progress tracking
- Pandas for CSV handling

Install dependencies with pip:

```bash
pip install selenium tqdm pandas
```

### ChromeDriver

You will need **ChromeDriver** to run Selenium. Download the version compatible with your Chrome browser from [here](https://sites.google.com/a/chromium.org/chromedriver/downloads). Ensure the path to `chromedriver` is correctly set in the script.

## Usage

### 1. Prepare your CSV File

The script expects a CSV file (`a.csv`) where the first column contains the tickers of the assets you want to scrape. For example:

```csv
AAPL
GOOG
MSFT
...
```

### 2. Run the Script

Once the CSV is prepared and dependencies are installed, run the script:

```bash
python scraper.py
```

The script will visit the Yahoo Finance page for each ticker, extract the asset group and company name, and save the results in a new CSV file (`resultats_scraping.csv`).

### 3. Output

The script outputs a CSV file (`resultats_scraping.csv`) with the following columns:
- **Ticker**: The asset's ticker symbol.
- **Nom**: The full company name.
- **Groupe**: The asset's group (e.g., stock, ETF, cryptocurrency).

Example output:

```csv
Ticker,Nom,Groupe
AAPL,Apple Inc.,Technology
GOOG,Alphabet Inc.,Technology
MSFT,Microsoft Corporation,Technology
...
```

## How It Works

1. **Headless Browser:** The script runs a headless instance of Chrome via Selenium, which fetches data from Yahoo Finance without displaying the browser.
2. **Xpath Selection:** The script uses XPath to locate and extract the asset group and company name for each ticker.
3. **Data Collection:** For each ticker, the script scrapes the data and updates the original CSV with the asset group and company name.
4. **Progress Bar:** The `tqdm` library provides a progress bar that updates with estimated time remaining for the scraping task.
5. **Error Handling:** If the data for a particular ticker cannot be found or an error occurs, the script continues and logs the error for review.

## Customization

You can easily modify this script to scrape additional data from Yahoo Finance or extend it to work with different data sources. For example, you could modify the XPath to target other financial metrics or add more columns to the CSV for other asset attributes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to modify any sections as needed!
