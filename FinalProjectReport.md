# Netflix Content Popularity

**CMSE 202 Final Project Report**

**Group Members**: Jaehan Kim, Akshita Mylavarapu, Taliah Blom, Ahmed Chishti

**GitHub Repository**: https://github.com/Jaehan-MSU/cmse202-netflix-popularity-analysis.git

---

## Introduction and Research Question

Netflix popularity reflects both audience engagement and critical reception, making it complex to explain. In this project, we answer the following question: **Can Netflix content features such as release year, rating, duration, genre, country, content type, and cast/director characteristics help explain or predict popularity?** We used IMDb score, IMDb votes, TMDB popularity, and TMDB score as popularity-related metrics.

## Methods and Computational Techniques

### Data Acquisition and Cleaning

We used the [Netflix TV Shows and Movies dataset from Kaggle](https://www.kaggle.com/datasets/victorsoeiro/netflix-tv-shows-and-movies), consisting of two files: `titles.csv` (content metadata) and `credits.csv` (information on actors and directors). After cleaning, our dataset contained 5,131 unique titles (3,271 movies and 1,860 TV shows).

We cleaned the data by selecting relevant columns, removing duplicates, converting numeric types, and handling missing values.

### Feature Engineering

We engineered several features to capture talent influence:

1. **Aggregations**: Created `num_actors` and `num_directors` columns.
2. **Leave-One-Out Averages**: Computed average IMDb score, TMDB score, and TMDB popularity for actors/directors across all other titles they worked on, avoiding target leakage.
3. **Encoding**: One-hot encoded `genres` and `production_countries`, retaining only the top 50 countries.

### Modeling Approach

We built regression and classification models using `scikit-learn`:

- **Regression**: Linear Regression (LR) and Support Vector Regression (SVR), on both original and log-transformed targets.
- **Classification**: Support Vector Classification (SVC) and Perceptron classifier, predicting top-quartile popularity. Features were standardized with `StandardScaler`.

Libraries used: Pandas, NumPy, Matplotlib, Seaborn, and scikit-learn.

## Results

### Exploratory Data Analysis

Our EDA revealed several interesting patterns:

- IMDb and TMDB scores are strongly correlated (r=0.69), suggesting they capture similar aspects.
- TV shows have higher average ratings than movies (IMDb: ~6.70 vs ~6.45; TMDB: ~6.96 vs ~6.75).
- Movies receive more IMDb votes on average (~32,000 vs ~11,000).
- IMDb votes and TMDB popularity are heavily right-skewed, with a few titles driving the mean upward.
- Release year and runtime show weak correlations with popularity metrics.


### Modeling Results

| Metric                      | IMDb Score | IMDb Votes | TMDB Popularity | TMDB Score |
| --------------------------- | ---------- | ---------- | --------------- | ---------- |
| LR R²<br>(original)         | 0.290      | 0.246      | 0.028           | 0.264      |
| LR R²<br>(log-transformed)  | 0.275      | 0.256      | 0.021           | 0.252      |
| SVR R²<br>(original)        | 0.308      | -0.047     | -0.012          | 0.294      |
| SVR R²<br>(log-transformed) | 0.298      | 0.214      | 0.017           | 0.247      |
| SVC Accuracy                | 74.0%      | 83.0%      | 79.9%           | 75.6%      |
| Perceptron Accuracy         | 63.0%      | 78.6%      | 74.2%           | 62.0%      |

**Table 1: Model Performance**

Regression models achieved R² values below 0.31 for all targets, indicating selected features explained only limited variation in the popularity metrics.

Classification models achieved higher accuracy. Confusion matrices revealed models had difficulty identifying truly popular titles (low recall) while being good at identifying non-popular titles (high true negatives).

### Feature Importance

For TMDB popularity, important features included: `runtime` (positive), `is_movie` (negative), `num_actors` (positive), `country_US` (positive), and `avg_actor_tmdb` (positive), `country_IN` (negative), and `genre_reality` (negative).

### Discussion

Our results suggest Netflix content popularity is difficult to predict using only content metadata and basic cast/director features. Popularity is influenced by many factors not captured in our dataset, which might include things like:

- Marketing spend and promotional campaigns
- Release timing and cultural context
- Cast and director star power beyond historical averages
- Netflix's internal recommendation algorithms
- Viewer word-of-mouth and social media engagement

The leave-one-out averages for actors and directors showed some predictive signal, but not enough to substantially improve model performance. This suggests while creative talent matters, their influence on popularity may be mediated by other factors.

### Challenges and Limitations

We encountered several challenges:

- High skew in TMDB popularity was difficult to model even after log-transformation.
- One-hot encoding created high-dimensional features.
- The dataset lacks crucial information on marketing and promotional strategies.

## Conclusion

Overall, our results show Netflix popularity is difficult to predict using only content metadata and basic cast/director features. Some features were useful for explaining patterns in the data. For example, **IMDb score** and **TMDB score** were strongly related, **IMDb votes** and **TMDB popularity** were moderately related, and content type showed differences between movies and TV shows. TV shows had higher average ratings, while movies received more IMDb votes.

However, the modeling results showed these features were not strong enough for accurate prediction. Regression models had low R² values, meaning they explained only limited variation in popularity metrics. Classification models achieved moderate accuracy, but still struggled to identify truly popular titles. This suggests popularity depends on many outside factors not included in the dataset.

In conclusion, content features can help explain some popularity patterns, but they cannot fully predict what becomes popular on Netflix. Future work could improve the analysis by including marketing data, release strategies, internal algorithms, social media attention, and audience trends.
