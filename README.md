# 📊 Netflix Movies & TV Shows Data Analysis using SQL

## 📌 Overview
This project provides an in-depth analysis of Netflix's movie and TV show catalog using SQL. The primary objective is to extract valuable insights, answer key business questions, and present findings in an interactive Tableau dashboard.

## 🎯 Objectives
- Analyze the distribution of content types (Movies vs. TV Shows)
- Identify the most common ratings for movies and TV shows
- Explore content distribution by release years, countries, and durations
- Categorize content based on specific criteria and keywords

## 📂 Dataset
The dataset used in this analysis is sourced from Kaggle:
🔗 [Movies Dataset](#)

## 🏛️ Schema
```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix (
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```

## 📌 Business Problems & SQL Solutions
### 1️⃣ Count the Number of Movies vs. TV Shows
```sql
SELECT type, COUNT(*) FROM netflix GROUP BY 1;
```
🎯 *Objective:* Determine the distribution of content types on Netflix.

### 2️⃣ Find the Most Common Rating for Movies and TV Shows
```sql
WITH RatingCounts AS (
    SELECT type, rating, COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT type, rating, rating_count,
           RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT type, rating AS most_frequent_rating FROM RankedRatings WHERE rank = 1;
```
🎯 *Objective:* Identify the most frequently occurring rating for each type of content.

### 3️⃣ Find All Movies Released in a Specific Year (e.g., 2020)
```sql
SELECT * FROM netflix WHERE release_year = 2020;
```
🎯 *Objective:* Retrieve all movies released in a specific year.

### 4️⃣ Identify the Top 5 Countries with the Most Content
```sql
SELECT * FROM (
    SELECT UNNEST(STRING_TO_ARRAY(country, ',')) AS country, COUNT(*) AS total_content
    FROM netflix GROUP BY 1
) AS t1 WHERE country IS NOT NULL
ORDER BY total_content DESC LIMIT 5;
```
🎯 *Objective:* Identify the top 5 countries with the highest number of content items.

### 5️⃣ Find the Longest Movie
```sql
SELECT * FROM netflix WHERE type = 'Movie' ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```
🎯 *Objective:* Identify the movie with the longest duration.

### 🔍 More Business Problems Solved:
- **Find Content Added in the Last 5 Years**
- **List All TV Shows with More Than 5 Seasons**
- **Count the Number of Content Items in Each Genre**
- **Identify the Top 10 Actors Appearing in Indian Productions**
- **Categorize Content Based on Keywords 'Kill' and 'Violence'**

## 📈 Findings & Conclusions
- **📊 Content Distribution:** Netflix offers a diverse range of movies and TV shows.
- **⭐ Common Ratings:** Understanding the most frequent ratings provides insight into the platform’s target audience.
- **🌍 Geographical Trends:** Identifying top countries producing Netflix content.
- **🎭 Content Categorization:** Keyword-based classification helps analyze content themes.

## 📊 Tableau Dashboard
An interactive **Tableau Dashboard** has been created to visualize insights derived from SQL analysis.

### 🔹 Key Visuals:
- 📊 **Content Type Distribution:** Movies vs. TV Shows
- 🌍 **Top Countries:** Content production by geography
- 📅 **Release Trends:** Yearly and country-based content patterns
- ⭐ **Rating Insights:** Most common ratings on Netflix

👉 **Explore the Tableau Dashboard here!** [Tableau Dashboard](#)

---
🚀 *This project provides a comprehensive view of Netflix's content and serves as a foundation for strategic decision-making in content curation.*
