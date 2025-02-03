# Analyzing Music Trends on Spotify using SQL

![spotify](https://github.com/ShivanisharmaF128/spotify_sql_project/blob/main/spotify_logo.jpg)

## Overview

This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using SQL. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance.

## Objectives

- Analyze the distribution of tracks based on album types (singles, albums, etc.).

- Identify the most streamed tracks and artists.

- Examine correlations between track attributes such as energy, danceability, and loudness.

- Analyze content based on streaming platforms, views, likes, and comments.

- Use SQL queries to answer business-related questions and generate insights on music consumption trends.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [spotify_dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

## Schema

```sql
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```

### Exploratory data analysis

```sql
SELECT COUNT(*) FROM spotify;

```

```sql
SELECT COUNT(DISTINCT artist) FROM spotify;
```

```sql
SELECT COUNT(DISTINCT album) FROM spotify;
```

```sql
SELECT DISTINCT album_type FROM spotify;
```

```sql
SELECT MAX(duration_min) FROM spotify;
SELECT MIN(duration_min) FROM spotify;
SELECT * FROM spotify
WHERE duration_min=0;

```
```sql

DELETE FROM spotify
WHERE duration_min = 0;
SELECT * FROM spotify
WHERE duration_min = 0;

```

```sql
SELECT DISTINCT channel FROM spotify;

```

```sql
SELECT DISTINCT most_played_on FROM spotify;

```

## Business Problems and Solutions

### 1.Retrieve the names of all tracks that have more than 1 billion streams.

```sql

SELECT * FROM  spotify
WHERE stream > 1000000000;

```

### 2.List all albums along with their respective artists.


```sql

SELECT
    DISTINCT album,artist
FROM spotify
ORDER BY 1;

```

### 3.Get the total number of comments for tracks where licensed = TRUE.

```sql

SELECT 
   SUM(comments) AS Total_comments
   FROM spotify 
WHERE licensed ='TRUE';

```

### 4. Find all tracks that belong to the album type single.

```sql

SELECT * FROM spotify
WHERE album_type = 'single' ;

```


### 5. Count the total number of tracks by each artist.

```sql

SELECT 
     artist,
	 count(*) AS total_no_of_songs
	 FROM spotify
	 GROUP BY artist
	 ORDER BY 2;

```

### 6..Calculate the average danceability of tracks in each album.

```sql
SELECT 
    album,
	AVG(danceability) AS avg_danceability
	FROM spotify
	GROUP BY 1
	ORDER BY 2 DESC;

```

### 7. Find the top 5 tracks with the highest energy values.

```sql

SELECT 
      track,
	 MAX(energy)
FROM spotify
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

```

### 8. List all tracks along with their views and likes where official_video = TRUE.

```sql

SELECT 
      track,
     SUM(views) AS total_views,
	 SUM(likes) AS total_likes
FROM spotify 
WHERE official_video = 'true'
GROUP BY 1
ORDER BY 2 DESC;


```

### 9. For each album, calculate the total views of all associated tracks.

```sql

SELECT
     album,
	 track,
	 SUM(views)
FROM spotify 
GROUP BY 1,2
ORDER BY 3 DESC;

```

### 10.Retrieve the track names that have been streamed on Spotify more than YouTube.

```sql

SELECT * FROM 
(SELECT 
      track,
	  COALESCE(SUM(CASE WHEN most_played_on ='youtube' THEN stream END),0)AS streamed_on_youtube,
	  COALESCE(SUM(CASE WHEN most_played_on ='spotify' THEN stream END),0)AS streamed_on_spotify
FROM spotify
GROUP BY 1
) AS t1
WHERE 
    streamed_on_spotify>streamed_on_youtube
	AND 
	streamed_on_youtube<>0;


```


### 11. Find the top 3 most-viewed tracks for each artist using window functions.
 

```sql

 WITH ranking_artist
 AS(
SELECT 
     artist,
	 track,
	 SUM(views) AS total_view,
	 DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views) DESC) AS rank
FROM spotify
GROUP BY 1,2
ORDER BY 1,3 DESC)
SELECT * FROM ranking_artist
WHERE rank <=3;

```

### 12. Write a query to find tracks where the liveness score is above the average.


```sql

SELECT 
       track,
	   artist,
	   liveness
FROM spotify
WHERE liveness>(SELECT AVG(liveness) FROM spotify);

```


### 13. Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

```sql

WITH cte
AS (
SELECT 
     album,
	 MAX(energy)AS  highest_energy,
	 MIN(energy)AS  lowest_energy

FROM spotify
GROUP BY 1)

SELECT 
      album,
	  highest_energy -lowest_energy AS energy_diffrence
FROM cte
ORDER BY 2 DESC;
	  

```



