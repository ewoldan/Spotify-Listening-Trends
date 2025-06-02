# Spotify Billlboard Hot 100 ETL & Analysis

<img width="713" alt="image" src="https://github.com/user-attachments/assets/78408c28-dc01-4069-aacf-99a15c55f760" />


## Table of Contents
1. [Description](#description)  
2. [Objective](#objective)  
3. [Business Problem](#business-problem)  
4. [Scope](#scope)  
5. [Data Acquisition & Preparation](#data-acquisition--preparation)  
6. [Tableau Dashboard](#tableau-dashboard)  



## Description
This project builds a repeatable workflow **Extract → Transform → Load (ETL)** pipeline that automatically retrieves the tracks in Billlboard Hot 100 playlist from Spotify, cleans and enriches the information, and stores the results in a database. In addition, it creates summary files—such as a list of the most‐featured artists—and produces a Tableau dashboard to visualize key trends.



## Objective
- Automate data collection from Spotify’s Billlboard Hot 100 playlist so that we always have the latest snapshot of what’s trending
- Clean and standardize the raw track data into a consistent, easy‐to‐use format
- Create additional insights, including:
  * A “most featured artists” list (which artists appear most often across the Hot 100)
  * Genre breakdowns, track durations, and popularity scores
- Store everything in a MySQL database for fast querying, plus export CSV files for quick reference
- Visualize the results in an interactive Tableau dashboard for analysts and decision‐makers



## Business Problem
Music labels, streaming analysts, and marketers often need an up‐to‐date snapshot of what’s trending on Spotify’s Hot 100. By ingesting and analyzing this data in a structured ETL pipeline, stakeholders can:
- **Identify emerging hitmakers** and “most featured” collaborators.  
- **Track genre shifts** at a glance (e.g., how many rap tracks make the Top 10 vs. pop or country).  
- **Monitor track‐level metrics** (popularity score, duration, release date) to inform A&R decisions.  
- **Enable automated refreshes** so that data‐driven strategy (playlisting, promotions, advertising) always leverages the latest Hot 100 snapshot.

Without this pipeline, analysts would spend significant time manually downloading CSV exports or scraping track pages—introducing lag, inconsistencies, and missed opportunities.



## Scope
1. **Extract**  
   - Connect to the Spotify Web API to pull all tracks in the official Hot 100 playlist (playlist ID: 6UeSakyzhiEt4NB3UAd6NQ)
   - Gather essential details such as track name, artist names, album information, release dates, duration, and Spotify’s popularity score

2. **Transform**  
   - Load raw JSON into a Pandas DataFrame.  
   - Normalize multi‐artist fields: split “artist1, artist2, …” for summary counts
   - Compute a “most featured artists” summary table by exploding the artist list and counting occurrences across all Top 100 tracks
   - Convert track durations from milliseconds into minutes for easier interpretation    

3. **Load**  
   - Use SQLAlchemy with a PyMySQL connector to write the cleaned DataFrame to a MySQL table called `hot_100`  
   - On each run, delete all existing rows in `hot_100` and replace them with the fresh 100‐row snapshot
   - Create a SQL view named `most_featured_artists` that computes “artist name + total appearances in the current Hot 100”  
   - Export one CSV for reporting `spotify_billboard_100_data.csv` (the full, row-level Hot 100 dataset)

4. **Visualization**  
   - Connect the MySQL data (or use the CSV exports) in Tableau to create charts and dashboards that showcase:
     - Top tracks by chart position and popularity
     - Genre distribution among the Hot 100
     - Scatter plots relating track duration to popularity
     - Bar charts of the most featured artists
     - Quick insights such as “Top Artist,” “Top Genre,” and “Average Track Duration”



## Data Acquisition & Preparation

1. **Spotify API Credentials**  
   - Register a free Spotify Developer application to obtain:  
     - `SPOTIPY_CLIENT_ID`  
     - `SPOTIPY_CLIENT_SECRET`  
   - Create a `.env` file in the project root (do **not** commit it) with:  
     ```text
     SPOTIPY_CLIENT_ID=your_client_id_here
     SPOTIPY_CLIENT_SECRET=your_client_secret_here
     MYSQL_PASSWORD=your_mysql_password_here
     ```

2. **Python Dependencies**  
   ```bash
   pip install pandas spotipy sqlalchemy pymysql python-dotenv

## Tableau Dashboard

The Tableau dashboard consists of two pages: 
- **the first** shows high‐level trends (top artists, genre distribution, and chart position vs. duration) with a global filter at the top to slice by date, genre, artist, or chart range
- on **the second page**, you’ll find a detailed table of all 100 tracks—each with a clickable Spotify URL—and a “Top Song” spotlight link at the bottom for instant playback.

**Open the full dashboard:**  
[Spotify Hot 100 Tableau Dashboard](https://public.tableau.com/app/profile/emilia.woldan6280/viz/Spotify-Hot-100/Dash-Main-Page)


<img width="1122" alt="Hot-100-Page1" src="https://github.com/user-attachments/assets/824adf5f-059f-4bee-8e0c-d7fa743ebc89" />

<img width="1122" alt="Hot-100-Page2" src="https://github.com/user-attachments/assets/51407af6-a9c5-4977-9734-327d7ab05336" />

