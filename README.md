# 🏠 MagicBricks Property Web Scraping using Selenium

## 📌 Overview

This project focuses on **web scraping and property data extraction using Python and Selenium**. The goal is to collect residential property information from [MagicBricks](https://www.magicbricks.com/) and save it in a structured CSV format for further analysis.

The project demonstrates browser automation, property filtering, dynamic content handling, data extraction, checkpointing, resume functionality, and Object-Oriented Programming (OOP).

The project contains three versions:

1. **Base Code** — Implements browser automation, property filtering, and property data extraction.
2. **Improved Code** — Adds checkpointing and resume functionality to reduce data loss and skip previously scraped properties.
3. **Restructured Code using OOP** — Organizes the scraper into a reusable `PropertyScraper` class and improves the reliability of infinite scrolling by allowing multiple consecutive attempts without page-height growth before stopping.

---

## 📁 Project Structure

```text
MagicBricks-Web-Scraping/
│
├── base_scraping.ipynb
├── checkpoint_resume_scraping.ipynb
├── Restructuring_the_code.ipynb
├── property_data_checkpoint.csv
├── property_data.csv
├── read this 1st notes.txt
├── README.md
└── .gitignore
```

### 📄 Important Files

- **`base_scraping.ipynb`** — Original Selenium scraper for extracting property information from MagicBricks.
- **`checkpoint_resume_scraping.ipynb`** — Improved scraper with checkpointing and resume functionality.
- **`Restructuring_the_code.ipynb`** — Restructures the scraper using OOP and includes a more reliable stopping condition for infinite scrolling.
- **`property_data_checkpoint.csv`** — Stores scraped data progressively to support resuming a previous scraping session.
- **`property_data.csv`** — Contains the final scraped property dataset.
- **`read this 1st notes.txt`** — Contains additional project notes and instructions.
- **`README.md`** — Documents the project, methodology, features, and setup.
- **`.gitignore`** — Specifies files and folders Git should ignore.

---

## 🐍 Python Environment & Jupyter Kernel Setup

Before building the project, I created a separate Python environment and configured a Jupyter kernel named `web_scraping` to manage the project dependencies.

### 1. Create and Activate the Environment

Create a virtual environment named `web_scraping`:

```bash
python -m venv web_scraping
```

Activate the environment on Windows:

```bash
web_scraping\Scripts\activate
```

### 2. Install Required Packages

Installed the necessary libraries for web scraping, HTML parsing, browser automation, and Jupyter Notebook.

```bash
pip install pandas numpy requests beautifulsoup4 lxml html5lib selenium jupyter ipykernel webdriver-manager
```

**Libraries Used:**

- `pandas` — Organizing and exporting scraped data.
- `numpy` — Handling missing values.
- `requests` — Sending HTTP requests to websites.
- `beautifulsoup4` — Parsing HTML content.
- `lxml` and `html5lib` — HTML parsing backends.
- `selenium` — Automating browser interactions.
- `jupyter` — Running Jupyter Notebooks.
- `ipykernel` — Registering the Python environment as a Jupyter kernel.
- `webdriver-manager` — Managing compatible WebDriver installations.

### 3. Create a Jupyter Kernel

Registered the environment as a Jupyter kernel named `web_scraping`:

```bash
python -m ipykernel install --user --name=web_scraping --display-name "web_scraping"
```

### 4. Select the Kernel

1. Open Jupyter Notebook.
2. Open the project notebook.
3. Select **Kernel → Change Kernel → web_scraping**.
4. Run the notebook using the configured environment.

---

## 📂 Projects & Approach

### 1. Base MagicBricks Property Scraper

**Website:** [https://www.magicbricks.com/](https://www.magicbricks.com/)

**Goal:**

To automate the process of searching for residential properties in Mumbai, applying property filters, and extracting property details from the search results.

**Approach:**

1. Used Selenium WebDriver to launch and control the Chrome browser.
2. Used `WebDriverWait` and expected conditions to locate and interact with webpage elements.
3. Created a reusable `wait_for_page_to_load()` function to check whether the initial webpage loading process had completed.
4. Opened MagicBricks and searched for properties in Mumbai.
5. Applied property filters:
   - **Property type:** Flats only.
   - **BHK:** All BHK configurations.
   - **Possession status:** Ready to Move.
   - Verified properties.
   - Properties with photos and videos.
   - RERA-registered properties.
6. Used JavaScript to scroll to webpage elements and interact with buttons when required.
7. Handled dynamically loaded property listings using infinite scrolling.
8. Extracted property details using CSS selectors, XPath, and element attributes.
9. Stored the extracted information in a Python list of dictionaries.
10. Converted the collected data into a Pandas DataFrame.
11. Exported the final dataset to `property_data.csv`.

**Output:** `property_data.csv`

**Limitation:** The base version saves the collected data after the scraping process finishes. If the program stops unexpectedly before the final save, data collected during that run may be lost.

---

### 2. Improved MagicBricks Property Scraper

**Goal:**

To improve the reliability of the base scraper by implementing checkpointing and resume functionality.

The improved version retains the `wait_for_page_to_load()` helper function and introduces two major improvements.

### 🚀 Feature Improvements

#### 1. Checkpointing

- Saves scraped property data to `property_data_checkpoint.csv` after each batch of newly identified properties is processed.
- Preserves previously collected records in the checkpoint file.
- Reduces the risk of losing all progress if the scraping process stops unexpectedly.
- Allows the saved checkpoint to be loaded during a subsequent run.

#### 2. Resume Scraping

- Checks whether `property_data_checkpoint.csv` exists before scraping begins.
- Loads previously scraped property records into memory.
- Stores existing `property_id` values in a Python set named `processed_ids`.
- Identifies new property cards by comparing their IDs against the existing set.
- Skips properties that have already been scraped.
- Adds newly processed property IDs to the set to avoid processing the same IDs repeatedly during a run.

### Improved Workflow

1. Initialize Selenium WebDriver and apply the required MagicBricks filters.
2. Check the initial webpage loading state using `wait_for_page_to_load()`.
3. Check whether a checkpoint file exists.
4. Load previously scraped data and property IDs if a checkpoint exists.
5. Otherwise, initialize an empty dataset and an empty set of processed IDs.
6. Scroll through the property listings and identify new property cards.
7. Skip previously scraped property IDs.
8. Extract the required information from new property cards.
9. Append the new records to the collected data.
10. Save the updated dataset to the checkpoint CSV after processing a batch.
11. Continue scraping until the configured stopping condition is reached.
12. Export the final dataset to `property_data.csv`.

### Output Files

| File                           | Purpose                                                              |
| ------------------------------ | -------------------------------------------------------------------- |
| `property_data_checkpoint.csv` | Stores scraped data progressively and supports resuming a later run. |
| `property_data.csv`            | Stores the final property dataset.                                   |

**Important:** Checkpointing occurs after a batch rather than necessarily after each individual property. If the program stops before the next checkpoint save, records collected since the previous save may need to be scraped again.

---

### 3. Restructuring the Code using OOP

**Notebook:** `Restructuring_the_code.ipynb`

**Goal:**

To improve code organization, reusability, and maintainability by restructuring the scraper using **Object-Oriented Programming (OOP)**, while retaining checkpointing, resume functionality, and dynamic content handling.

### 🚀 Feature Improvements

#### 1. Object-Oriented Programming (OOP)

The scraper logic is organized into a `PropertyScraper` class instead of keeping all functionality in separate procedural code blocks.

The class encapsulates browser management, webpage interactions, property extraction, checkpoint handling, and the overall scraping workflow.

**Key components:**

| Method / Attribute                   | Responsibility                                                                                                         |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `__init__()`                         | Initializes the scraper, property data, processed IDs, WebDriver, and explicit wait.                                   |
| `_initialize_driver()`               | Creates the Selenium WebDriver and maximizes the browser window.                                                       |
| `_wait_for_page_to_load()`           | Checks whether the initial document loading process has completed.                                                     |
| `access_website()`                   | Opens the MagicBricks website.                                                                                         |
| `search_properties()`                | Searches for properties in the specified location.                                                                     |
| `apply_filters()`                    | Applies the required property filters.                                                                                 |
| `checkpoint_setup()`                 | Loads existing checkpoint data and property IDs, or initializes a fresh run.                                           |
| `_extract_data_using_find_element()` | Extracts an element's text and returns `numpy.nan` when extraction fails.                                              |
| `scrape_webpage()`                   | Handles scrolling, identifies new property cards, extracts data, saves checkpoints, and checks the stopping condition. |
| `run()`                              | Coordinates the complete scraping workflow and closes the browser afterward.                                           |

#### 2. Improved Infinite Scrolling Reliability

The scraper includes a stopping condition that allows **three consecutive attempts without page-height growth** before terminating the scrolling loop.

This reduces the chance of stopping the scraping process immediately when the page height does not increase after a single scroll.

**Implementation:**

**How it works:**

1. Records the current webpage height using `document.body.scrollHeight`.
2. Scrolls to the bottom of the page.
3. Waits for additional content to load.
4. Retrieves the updated page height.
5. Resets `no_growth_attempts` to zero if the page height increases.
6. Increments `no_growth_attempts` if the page height remains unchanged or decreases.
7. Stops when three consecutive attempts fail to produce page-height growth.

**Why this improvement matters:**

- A single unsuccessful scroll does not immediately terminate scraping.
- The scraper gets additional opportunities to load content.
- The stopping condition is more tolerant of delayed content loading.
- The loop eventually stops when repeated attempts fail to increase the page height.

**Important distinction:** This condition checks page-height growth, not whether new property IDs were found. The scraper may encounter no new property cards even when the page height increases, or discover new cards without a measurable height increase. Therefore, the condition improves scrolling reliability but does not guarantee that every available property has been collected.

### Running the OOP Scraper

The scraping workflow is started using the following code:

**Output:** `property_data.csv` and `property_data_checkpoint.csv`.

---

### Data Collected

| Column                   | Description                               |
| ------------------------ | ----------------------------------------- |
| `property_id`            | Unique property listing identifier        |
| `property_title`         | Property listing title including BHK type |
| `property_developer`     | Name of the property developer            |
| `property_price`         | Listed property price                     |
| `property_cost_per_sqft` | Price per square foot                     |
| `property_super_area`    | Super built-up area, where available      |
| `property_carpet_area`   | Carpet area, where available              |
| `property_status`        | Property possession or status information |
| `property_transaction`   | Transaction type                          |
| `property_furnishing`    | Furnishing status                         |
| `property_society`       | Name of the housing society               |
| `property_car_parking`   | Car parking information                   |
| `property_floor`         | Floor information                         |
| `property_facing`        | Property facing direction                 |
| `property_overlooking`   | Property overlooking information   

---

## 🔎 General Methodology

### 1. 🌐 Browser Automation

- Used Selenium WebDriver to automate Google Chrome.
- Interacted with the search bar, property filters, and property listings.
- Used `WebDriverWait` and expected conditions to wait for elements to become available or clickable.
- Created a reusable `wait_for_page_to_load()` helper function to check the initial document loading state.

### 2. 🔍 Dynamic Content Handling

- **Page Load Check:** Used `document.readyState` to check whether the initial page load had completed.

- **Infinite Scrolling:** Used `window.scrollTo(0, document.body.scrollHeight)` to scroll to the bottom of the webpage and trigger the loading of additional property listings.

- **Page Height Monitoring:** Compared previous and current values of `document.body.scrollHeight` to check whether the webpage had grown. The OOP version allows three consecutive attempts without page-height growth before stopping.

- **Element Positioning using `getBoundingClientRect()`:** Used `element.getBoundingClientRect().top` to determine an element's position relative to the browser viewport. Combined it with `window.scrollBy()` to position filter buttons and other elements for easier interaction.

- **JavaScript Interaction:** Used JavaScript to scroll to elements, bring property cards into view, and interact with buttons when required.

- **Element Identification:** Used CSS selectors, XPath, and element IDs to locate and extract property information.

### 3. 📋 Property Filtering

Applied the required filters to narrow down the property search results before collecting data.

### 4. 🛡️ Error Handling

- Used `try-except` blocks to handle missing elements and extraction errors.
- Assigned `numpy.nan` when individual property attributes could not be extracted.
- Used explicit waits to handle element availability and interaction timing.
- Included timeout handling in the page-loading helper function.
- Used repeated attempts without page-height growth before stopping the scrolling loop in the OOP version.

### 5. 💾 Data Storage and Recovery

- Stored extracted property records in Python lists and dictionaries.
- Converted the records into Pandas DataFrames.
- Exported data to CSV files using UTF-8 encoding.
- Implemented checkpointing to preserve progress between batches.
- Used property IDs to identify and skip previously scraped listings.

### 6. 🧱 Code Organization using OOP

- Encapsulated browser automation and scraping state within a `PropertyScraper` class.
- Separated website access, searching, filtering, checkpoint setup, and extraction into individual methods.
- Used instance attributes such as `self.driver`, `self.wait`, `self.property_data`, and `self.processed_ids` to share state across methods.
- Used a `run()` method to coordinate the overall scraping workflow.
- Used `try-finally` to ensure that the browser is closed when the workflow exits.

---

## 🛠️ Technologies Used

- **Python** — Core programming language.
- **Selenium** — Browser automation and dynamic webpage interaction.
- **Pandas** — Data organization and CSV file handling.
- **NumPy** — Handling missing values.
- **Requests** — Sending HTTP requests.
- **BeautifulSoup** — Parsing HTML content.
- **lxml and html5lib** — HTML parsing backends.
- **Jupyter Notebook** — Development and execution environment.
- **ipykernel** — Jupyter kernel management.
- **webdriver-manager** — WebDriver management.
- **Google Chrome** — Browser used for scraping.

---

## 🚀 Installation and Execution

### 1. Activate the Environment

On Windows:

```bash
web_scraping\Scripts\activate
```

### 2. Start Jupyter Notebook

```bash
jupyter notebook
```

### 3. Run the Base Code

Open `base_scraping.ipynb` and execute the notebook cells in order.

This version collects property information and saves the final dataset after scraping finishes.

### 4. Run the Improved Code

Open `checkpoint_resume_scraping.ipynb` and execute the notebook cells in order.

This version loads existing checkpoint data, skips previously scraped property IDs, and saves progress after each batch.

### 5. Run the Restructured OOP Code

Open `Restructuring_the_code.ipynb` and execute the notebook cells in order.

This version organizes the scraping workflow into a `PropertyScraper` class and uses three consecutive attempts without page-height growth as the stopping condition.

**Important:** Keep the checkpoint file in the expected working directory when resuming a previous scraping session. If the checkpoint file is missing, the scraper initializes an empty dataset.

---

## 📊 Output

| Output File                    | Description                                                |
| ------------------------------ | ---------------------------------------------------------- |
| `property_data.csv`            | Final dataset containing the scraped property information. |
| `property_data_checkpoint.csv` | Progressively saved dataset used to resume scraping.       |

The checkpoint and final CSV files are stored in the project directory, provided that the notebook is executed with that directory as its working directory.

---

## 📌 Key Learning Outcomes

Through this project, I learned how to:

- Create a dedicated Python environment and Jupyter kernel.
- Install and manage web scraping dependencies.
- Automate browser interactions using Selenium.
- Create reusable functions for webpage loading checks.
- Locate webpage elements using XPath, CSS selectors, and IDs.
- Apply search filters to narrow down property listings.
- Extract structured data from dynamically loaded webpages.
- Implement infinite scrolling and monitor webpage height.
- Use `getBoundingClientRect()` to determine an element's position relative to the viewport.
- Handle missing property attributes using `try-except` and `numpy.nan`.
- Implement checkpointing to reduce data loss.
- Resume scraping from previously saved property IDs.
- Improve scrolling reliability using consecutive no-growth attempts.
- Structure a scraping workflow using Object-Oriented Programming.
- Encapsulate browser state and scraping operations in a reusable class.
- Coordinate the scraping workflow through a single `run()` method.

---

## 🔮 Future Improvements

- Add logging to record scraping progress and errors.
- Separate browser automation, data extraction, and file management into reusable modules.
- Perform data cleaning and exploratory data analysis (EDA) on the collected property dataset.
- Use the extracted data in a property price prediction or recommendation system.

---

## ⚠️ Disclaimer

This project is intended for educational and portfolio purposes. Please respect MagicBricks' terms of service, applicable laws, website access restrictions, and rate limits when collecting data.
