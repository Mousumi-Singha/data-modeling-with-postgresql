# Project 1: Data modeling with Postgres

The main objective of this **Udacity Data Engineering nanodegree** project is to create a postgres database `sparkifydb` for a music app named *Sparkify* and an ETL pipeline to transfer the data from the files to the tables in postgres database.It will be modeling the song datasets and log datasets (which originally resides in a directory with JSON metadata on the songs and directory of JSON logs on user activity on the app respectively) with a star schema optimised for queries in order to analyze the data they've been collecting on songs and user activity on their new music streaming app to know what song users are listening to.


## Schema design and ETL pipeline

The star schema has 1 *fact* table (songplays) at the centre and 4 *dimension* tables (users, songs, artists, time) surrounding the fact table as star points. `DROP`, `CREATE`, `INSERT`, and `SELECT` queries are defined in **sql_queries.py**. **create_tables.py** uses functions like `create_database`, `drop_tables`, and `create_tables` to drop and create the database sparkifydb and the required tables.

Extract, transform, load processes in **etl.py** populate the **songs** and **artists** tables with data derived from the JSON song files, present in location `data/song_data`. Processed data derived from the JSON log files in location `data/log_data`, is used to populate **time** and **users** tables. A `SELECT` query gets the song id and artist id from the **songs** and **artists** tables and combines this with log file derived data to populate the **songplays** fact table.

## Prerequisites

This project makes the following assumptions:

* Python 3 is available
* `pandas` and `psycopg2` are available
* A PosgreSQL database is available on localhost

## Running the Python Scripts

At the terminal:

1. ```python create_tables.py```
2. ```python etl.py```

In IPython:

1. ```run create_tables.py```
2. ```run etl.py```

## Files

`create_tables.py`

Python script to create postgres tables.

`etl.ipynb`

Jupyter Notebook for ETL testing.

`etl.py`

Python script to load data into postgres tables.

`README.md`

This file which discusses the specifics of the project.

`sql_queries.py`

Python script with SQL queries to create,insert and query tables.

`test.ipynb`

Jupyter Notebook to test postgres data base.

`data/`

Folder containing sample data that is a subset of the **Million Song Dataset**.

## Data verification query

This query should return only one row that has a value for song_id and artist_id altogether. This is because the dataset in this project assignment is a subset of the real-world data.

`select * from songplays where artist_id is not null and song_id is not null;`

## Song play example queries

Number of users in each membership level.

`select level,count(*) from users group by level;`

Day of the week music most frequently listened to.

`SELECT weekday,count(*) FROM time group by weekday order by count(*) desc limit 1;`

Or, hour of the day music most often listened to.

`SELECT hour,count(*) from time group by hour order by count(*) desc limit 1;`

How many users are on the app based on gender.

`select gender,count(*) from users group by gender;`

Number of users from California.

`select count(*) from songplays where location like '%CA';`

The name of the artist and song whose song id is not NONE.

`select name artist_name,title song_name from artists a,songs s where a.artist_id=s.artist_id and (s.song_id,s.artist_id)=(select song_id,artist_id from songplays where song_id!='None');`

The distinct users who accessed the Sparkify app through an Iphone.

`select count(DISTINCT user_id) from songplays where user_agent LIKE '%iPhone%';`
