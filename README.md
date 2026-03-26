# 🎬 Movie Database Analysis using MySQL (ALX Project)

## 📌 Project Overview
This project focuses on analyzing a movie database using **MySQL**. The dataset contains information on movies, actors, genres, production companies, languages, and Oscar awards.

The goal of this project was to:
- Clean and prepare raw data
- Understand relational database structure (ERD)
- Perform SQL queries to answer analytical questions
- Extract meaningful insights from the data

---

## 🗂️ Step 1: Understanding the Database
The database consists of **14 tables**, including:

### Core Tables:
- movies  
- actors  
- genres  
- keywords  
- productioncompanies  
- productioncountries  
- languages  
- oscars  

### Mapping Tables:
- casts  
- genremap  
- keywordmap  
- productioncompanymap  
- productioncountrymap  
- languagemap  

These tables are connected using **primary and foreign keys**, forming a relational database.

---

## 🧹 Step 2: Data Cleaning
Before analysis, I performed data cleaning to ensure accuracy and consistency:

- Removed duplicate records  
- Handled missing values  
- Standardized inconsistent data formats  
- Fixed issues in the Oscars dataset (e.g., year format like "1932/1933" → "1933")  
- Ensured proper relationships between tables  

---

## 🛠️ Step 3: Tools Used
- MySQL  
- SQL (for querying and analysis)

---

## 🔍 Step 4: Data Analysis (SQL Queries)

I used SQL to answer real-world analytical questions, including:

### 🎯 Example Questions Solved:
- Who won the Oscar for "Actor in a Leading Role" in 2015?  
- What are the 10 oldest movies in the database?  
- How many unique awards exist in the Oscars table?  
- How many movies contain specific keywords like "Spider"?  
- How many movies belong to a specific genre and keyword combination?  
- Which production companies have the highest average popularity?  
- Which genre has the lowest average popularity?  
- How many unique roles has a specific actor played?  

---

## 💻 Sample Query

```sql
SELECT * FROM oscars
WHERE 
    award = 'Actor in a Leading Role'
    AND year = 2015
    AND winner = 1.0;

SELECT * 
FROM movies 
WHERE release_date 
IS NOT NULL ORDER BY release;

SELECT COUNT(DISTINCT award)
FROM oscars;

SELECT COUNT(title)
FROM movies
WHERE title LIKE '%spider%';

SELECT COUNT(DISTINCT movies.movie_id)
FROM movies
JOIN genremap ON movies.movie_id = genremap.movie_id
JOIN genres ON genremap.genre_id = genres.genre_id
JOIN keywordmap ON movies.movie_id = keywordmap.movie_id
JOIN keywords ON keywordmap.keyword_id = keywords.keyword_id
WHERE genres.genre_name = 'Thriller'
  AND keywords.keyword_name LIKE '%love%';

SELECT COUNT(release_date)
FROM movies
WHERE release_date 
BETWEEN '2006-08-01' AND '2009-10-01'
AND popularity > 40
AND budget < 50000000;

SELECT COUNT(DISTINCT characters)
FROM actors
JOIN casts
ON casts.actor_id=actors.actor_id
WHERE actor_name = "Vin Diesel";

SELECT genre_name
FROM movies
JOIN genremap
ON movies.movie_id=genremap.movie_id
JOIN genres
ON genremap.genre_id=genres.genre_id
WHERE title = 'The Royal Tenenbaum;

SELECT production_company_name, AVG(popularity) AS avg_popularity
FROM movies
JOIN productioncompanymap
ON movies.movie_id = productioncompanymap.movie_id
JOIN productioncompanies
ON productioncompanies.production_company_id = productioncompanymap.production_company_id
GROUP BY production_company_name
ORDER BY avg_popularity DESC
LIMIT 3;

SELECT COUNT(actor_name)
FROM actors
WHERE gender = 1
AND actor_name LIKE 'N%';

SELECT genre_name, AVG(popularity) AS avg_popularity
FROM movies
JOIN genremap
ON movies.movie_id=genremap.movie_id
JOIN genres
ON genremap.genre_id=genres.genre_id
GROUP BY genre_name
ORDER BY avg_popularity ASC;

SELECT award, COUNT(*) AS actor_nominations
FROM oscars
JOIN actors 
ON oscars.name=actors.actor_name
GROUP BY award 
ORDER BY actor_nominations DESC;

CREATE VIEW Alan_Rickman_Movies AS
SELECT title, release_date, tagline, overview 
FROM Movies
LEFT JOIN Casts ON Casts.movie_id = Movies.movie_id 
Left JOIN Actors ON Casts.actor_id = Actors.actor_id 
WHERE Actors.actor_name = 'Alan Rickman';

sql'''

