
## Overview
The H&M Group, operating in 53 online markets with ~4,850 stores, faces a critical challenge in helping customers navigate their extensive product catalog efficiently. With e-commerce fashion projected to reach $1.0 trillion by 2025, effective product recommendations have become essential for both business success and environmental sustainability by reducing returns and transportation emissions.

This project develops and evaluates multiple recommendation approaches using H&M's transaction data (09/2018-09/2020) along with customer and product metadata to predict customer purchases in a 7-day window. The system aims to enhance shopping efficiency, customer satisfaction, and revenue growth while contributing to sustainable retail practices. The findings contribute to both academic research in recommendation systems and practical applications in fashion e-commerce.

## Data Files
The project uses three main datasets:
- articles.csv: Product metadata including article ID, product name, color, category, description (105,542 items)
- customers.csv: Customer demographics with age, postal code, member status (1.37M users)
- transactions.csv: Purchase records from 2018-2020 containing customer ID, article ID, price, sales channel (28.8M transactions)

Due to GitHub size restrictions, data files are not included in the repository. 
Please download from https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data


## Repository Contents
`notebooks/`
  1. `Exploratory-Data-Analysis.ipynb`: Initial data profiling of articles, customers, and transactions. Analysis of product types, colors, seasonality, and customer segments
  2. `Item-Based-Collaborative-Filtering.ipynb`: Item-Item similarity based recommendations
  3. `Content-Based-Filtering using CosineSimilarity.ipynb`: TF-IDF vectorization and cosine similarity based recommendations
  4. `Content-Based-Filtering using KNN.ipynb`: TF-IDF vectorization and K Nearest Neighbors based recommendations
  5. `ALS.ipynb`: Matrix factorization with Alternating Least Squares for collaborative filtering

## Methodology

### Exploratory Analysis
The analysis leveraged multiple analytical approaches to understand customer behavior and product performance:
1. Analyzed product sales patterns through aggregation of transaction data and seasonal trends visualization
2. Examined color preferences across product categories using cross-tabulation and heat mapping
3. Investigated age-based purchasing patterns by merging customer and transaction data
4. Evaluated customer retention through cohort analysis tracking purchase patterns over time
5. Performed customer segmentation using K-means clustering based on purchase behavior metrics (frequency, spend, unique items)

### Data Processing
**Data Sampling:** Selected last 6 weeks of data due to fast fashion's seasonal nature
- Original dataset:
  - Articles: 105,542 items
  - Transactions: 28,813,419 records
  - Customers: 1,371,980 users

**Data Splitting**
  - Training: 2020-08-12 to 2020-09-15 (5 weeks)
  - Testing: 2020-09-16 to 2020-09-22 (1 week)
  - Split ratio: 5:1 (training:testing)

### Implementation Approaches
**1. Collaborative Filtering:**
- Cosine Similarity Approach:
  - Created sparse user-item interaction matrix from transaction data. Matrix values are binary (1 for purchase interactions or else 0)
  - Computed item-item similarity using cosine similarity
- ALS:
  - Created user-item matrix using all customers and articles. Full matrix scope: all customers × all articles and used transactions to mark interaction points as 1
  - Matrix factorization parameters: Factors: 800, Iterations: 30, Regularization: 0.005, Alpha: 60
    
**2. Content Based Filtering:**
- Preprocessed text descriptions (lowercase, special character removal) and Applied TF-IDF vectorization with 5000 features
- Cosine Similarity Approach:
  - Generated content similarity using cosine similarity between item vectors  
- KNN Approach:
  - Used KNN (k=7) with cosine distance for similarity computation between item vectors

### Recommendation Strategy
- Existing customers:
  - Generated recommendations based on purchase history similarity
  - Combined content features with user preference
- New customers:
  - Recommended popular items based on overall purchase frequency
  - Used global item popularity metrics

### Evaluation
All of the approaches are evaluated using Mean Average Precision @ 12 (MAP@12)

## Installation and Usage
To set up the project, clone the repository, navigate to the directory, and install dependencies.
```bash
git clone https://github.com/Shanmukhi1920/H-M-Recommendation-System
cd H-M-Recommendation-System
pip install -r requirements.txt
jupyter notebook
```

## Key Findings
### Model Performance
- ALS performed best (MAP@12: 0.0072)
- KNN content-based achieved second-best (MAP@12: 0.0065)
- Item-based CF and cosine similarity content-based showed similar performance (MAP@12: ~0.0050)

### Customer Behavior
- Trousers lead sales (4M+ units), followed by dresses (3.5M) and sweaters (3M)
- Black and white dominate color preferences
- Strong seasonal patterns: dresses peak in summer, sweaters in fall/winter

### Age-Related Insights
- Younger customers (20-25) make frequent, smaller purchases
- Older customers (60-80) make fewer but larger purchases
- Customer retention drops from 62% at day 50 to 40% by day 350

### Product Preferences
- Strong preference for basic wardrobe staples
- Product popularity varies significantly by season
- Black most popular for dresses (64.75%) and tops (57.41%)
- T-shirts show balanced preference between black (41.77%) and white (43.13%)

## Citation
Carlos García Ling, ElizabethHMGroup, FridaRim, inversion, Jaime Ferrando, Maggie, neuraloverflow, and xlsrln. H&M Personalized Fashion Recommendations. https://kaggle.com/competitions/h-and-m-personalized-fashion-recommendations, 2022. Kaggle.
