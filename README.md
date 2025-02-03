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
