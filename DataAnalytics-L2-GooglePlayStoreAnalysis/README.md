# Unveiling the Android App Market: Google Play Store Analysis

An end-to-end data analysis of the Google Play Store: cleaning messy real-world data, exploring categories, ratings, size and pricing, running sentiment analysis on user reviews, and turning the findings into advice for a developer planning to launch a new app.

> Built as part of my Data Analytics internship at Oasis Infobyte (Level 2, Task 4).

---

## Project Objective

Answer one practical question: **if you were launching a new Android app, what does the data say about where to compete, how to price, and what users care about?**

## Tech Stack

- **Python**: pandas, NumPy
- **Visualization**: Matplotlib, Seaborn, Plotly (interactive chart)
- **NLP / Sentiment**: TextBlob
- **Environment**: Jupyter Notebook

## Datasets

| File | Description | Raw size |
|---|---|---|
| `googleplaystore.csv` | App details (category, rating, reviews, size, installs, price, etc.) | 10,841 rows, 13 columns |
| `googleplaystore_user_reviews.csv` | User review text with sentiment fields | 64,295 rows, 5 columns |

*Source: Google Play Store Apps dataset (Kaggle).*

## Data Cleaning

- Removed one **malformed row** (values shifted into the wrong columns, which put `1.9` in the Category column).
- Removed **483 exact duplicates** and kept one row per app, leaving **9,658 unique apps**.
- Converted text to numbers: `Installs` ("10,000+"), `Price` ("$4.99"), `Size` ("14M", "512k"), `Reviews`, and converted `Last Updated` to a date.
- Kept null `Rating` and `Size` values instead of inventing replacements.
- Reviews: dropped rows with no review text and duplicates, leaving **29,692** reviews.

## Key Findings

### 1. Category saturation
- 33 categories. **FAMILY (19.0%), GAME (9.9%) and TOOLS (8.6%)** hold **37.5%** of all apps.
- None of these three are in the top five for average installs per app (COMMUNICATION, VIDEO_PLAYERS, SOCIAL, ENTERTAINMENT, PHOTOGRAPHY). A crowded category means more competition, not necessarily more demand.

### 2. Ratings
- Mean rating **4.17**, median **4.3**; the distribution is left-skewed.
- Highest-rated categories: **EVENTS (4.44), EDUCATION (4.36), ART_AND_DESIGN (4.36)**. Lowest: **DATING (3.97)**. The gap between best and worst is under 0.5 points.

### 3. Size vs. installs
- Weak relationship: **Pearson 0.134, Spearman 0.31**. App size is not a growth lever.
- GAME apps are the largest (median 38 MB).

### 4. Pricing
- **92.2% free**, 7.8% paid. Median paid price **$2.99**.
- About 20 novelty "I Am Rich" apps priced near $400 added roughly **$86.5M** to the first revenue estimate. They were excluded.
- Estimated revenue (price x installs, a floor estimate): **FAMILY ~$103.7M**, **GAME ~$40.7M**, with every other category below $9M.

### 5. Sentiment
- TextBlob classification: **60.2% positive, 21.7% neutral, 18.1% negative**.
- Matches the dataset's original labels for **93.0%** of reviews. All differences are the neutral threshold; there are no positive/negative reversals.
- Most positive: AUTO_AND_VEHICLES, EVENTS, HEALTH_AND_FITNESS, EDUCATION (73-79% positive).
- Most negative: GAME (28.8% negative), SOCIAL, FAMILY, VIDEO_PLAYERS, ENTERTAINMENT.

## Insights for a Developer

1. **The biggest categories are the hardest to win.** FAMILY, GAME and TOOLS are crowded, and GAME and FAMILY also draw the most negative reviews.
2. **Less crowded, well-liked categories are a safer entry point**, such as EDUCATION, EVENTS and HEALTH_AND_FITNESS.
3. **Launch free, price low, and don't chase app size.** Most paid apps cost under $5, and size has little link to installs.

## Limitations

- One-time snapshot of the data.
- Installs are stored as ranges, so revenue is a floor estimate (before store commission and refunds).
- Sentiment comes from TextBlob on translated text; it can miss sarcasm and context.
- All findings are correlations, not proof of cause.

## Visuals

![Size vs. Installs](images/app_size_vs_num_installs.png.png)


![Rating distribution](images/avg_rating_cat.png.png)

![Sentiment by category](images/review_sentiment_cat.png.png)

![Free vs. Paid Apps](image/free_vs_paid.png)

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn plotly textblob jupyter

# 3. Place the two CSV files in the project folder, then launch the notebook
jupyter notebook "Google Play Store Analysis.ipynb"
```

Run all cells from top to bottom (cells depend on earlier ones).

## Repository Structure

```
├── Google Play Store Analysis.ipynb
├── googleplaystore.csv
├── googleplaystore_user_reviews.csv
├── images/            # screenshots of key charts
└── README.md
```

## Author

**Faith Aipoh**: Data Analyst

---

*Feedback and suggestions are welcome.*
