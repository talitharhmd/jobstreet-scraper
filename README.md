# 📄Short Report – Scraping Data Science Job Listings from Jobstreet

## a.  🗂️Data Description
This dataset contains job listings related to data science, collected from the Jobstreet website. The data was gathered from three countries: Malaysia, Indonesia, and Singapore.
The scraping process extracted publicly available job listings using keywords such as “data scientist,” “data analyst,” “machine learning,” and related terms.

| Column        | Description                            |
| ------------- | -------------------------------------- |
| `url`         | Link to the job listing detail page    |
| `country`     | Country where the job is posted        |
| `title`       | Job title                              |
| `company`     | Company name                           |
| `location`    | Job location                           |
| `category`    | Job category or field                  |
| `work_type`   | Work type (e.g., Full-time, Part-time) |
| `description` | Full job description                   |

Total records:
The scraping collected around 475+ job listings across the three countries.

## b. 🔧Scraping Process
The data was collected using Selenium and Chrome WebDriver, which allows scraping from dynamically loaded content.
Scraping steps:
1) Define a set of keywords:
"data scientist", "data analyst", "machine learning", "data engineer", "data science"
2) Search each keyword on different country-specific Jobstreet domains:
- jobstreet.com.my (Malaysia)
- jobstreet.com.sg (Singapore)
- id.jobstreet.com (Indonesia)
3) Extract all job links from the search results.
4) Visit each job detail page to extract the following:
Job title, Company, Location, Job category, Work type, Job description.
5) Store the results in a structured table using pandas.

## c. ⚙️Preprocessing
1. We applied standard case folding and removed unnecessary patterns:
- Lowercased all text
- Removed URLs, hashtags, emojis, special characters
- Removed extra whitespace
2. Language Translation: Indonesian texts were translated to English using Deep Translator.
3. We created a dictionary of Indonesian internet slangs and replaced them with formal words.
example: 
Raw data       : "gaji segini mah ga worth it bgt"
Case folding   : "gaji segini mah ga worth it bgt"
Corrected Text : "gaji segini tidak layak banget"
4. Tokenization
5. Stopword Removal
Custom stopwords were created using with manual stopword list and Sastrawi & NLTK Indonesia stopwords.
6. Lemmatization
Using TextBlob, each word was lemmatized to its root form.
7. EDA

📦 Sample Output (Before → After)
| Step             | Example                                                                  |
| ---------------- | ------------------------------------------------------------------------ |
| Raw Text         | "kerja remote tp gaji segitu mah ogah lah"                               |
| Cleaned          | "kerja remote tp gaji segitu mah ogah lah"                               |
| After Slang Fix  | "kerja remote tetapi gaji segitu tidak mau lah"                          |
| Tokenized        | `["kerja", "remote", "tetapi", "gaji", "segitu", "tidak", "mau", "lah"]` |
| Stopword Removed | `["kerja", "remote", "gaji", "segitu", "mau"]`                           |
| Lemmatized       | `["work", "remote", "salary", "segitu", "want"]`                         |

## d. 🧠Reflections
| Challenge                                                                                  | Solution                                                                                 |
| ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| Some pages took longer to load and required additional wait time                           | Used `WebDriverWait` with timeout handling to ensure pages were fully loaded             |
| Variations in page layout between countries caused element lookup failures                 | Implemented conditional scraping logic based on country and detected layout structure    |
| Some listings returned empty content or resulted in a timeout                              | Wrapped scraping logic in `try-except` blocks to catch and handle `TimeoutException`     |
| Link structures differ by country                                                          | Built a dynamic, country-aware URL generation function to adjust for each domain         |
| Duplicate job links in the list                                                            | Used a dictionary or `set()` to automatically remove duplicate job URLs                  |
| Risk of incorrect or incomplete data being saved                                           | Previewed and validated the data before saving to ensure accuracy and completeness       |

