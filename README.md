# ETL-pipeline

<br />

<p align="center">
 <h2 align="left"><b>Data Modeling ETL with PostgreSQL</b></h2>
 <p align="left">
  Project in Data Engineer Nanodegree Course by Udacity
  <br />
 </p>


## Project Introduction

<!-- Describing the project in brief -->

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming application. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the application, as well as a directory with JSON meta-data on the songs in their app.

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis. The role of this project is to create a database schema with fact and dimension tables and ETL pipeline for this analysis. 

The goal is to model the data with Postgres and build an ETL pipeline using Python.

### Datasets
#### Song Dataset
The dataset is a subset of [Million Song Dataset](http://millionsongdataset.com/).  Each file in the dataset is in JSON format and contains meta-data about a song and the artist of that song. 

Sample :
```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

#### Log Dataset
Logs dataset containes log files, it was generated using [Event Simulator](https://github.com/Interana/eventsim).  The files are in JSON format and simulate activity logs from a music streaming application based on specified configurations.

Sample:
```
{"artist": null, "auth": "Logged In", "firstName": "Walter", "gender": "M", "itemInSession": 0, "lastName": "Frye", "length": null, "level": "free", "location": "San Francisco-Oakland-Hayward, CA", "method": "GET","page": "Home", "registration": 1540919166796.0, "sessionId": 38, "song": null, "status": 200, "ts": 1541105830796, "userAgent": "\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"", "userId": "39"}
```


### Prerequisites

The prerequisites to run the program are:

* python with installed `psycopg2` and `pandas` libraries
* PostgreSQL 

### How to run

1. Create the tables by

   ```python
   python3 create_tables.py
   ```

2. Run ETL process by 

   ```python
   python3 etl.py
   ```

3. Executing `test.ipynb` you can check whether the data has been loaded into database properly 



## Database Schema Design

### Database Entity Relationship Diagram 

The scheme used in this project was the Star Schema where we have Song Plays as a fact table that contains all the metrics (facts) associated with each event (user actions) and four other dimension tables that contain data related to the user, artist, songs and time data such as day, hour, etc.

This approach is mostly used for relational data modeling, it allows us to have a search in a database using the minimum number of "Joins" in our queries which consequently generates quick reads.

In this project, the amount of data used is not large enough for us to use a big data or NoSQL database solution.

The Entity Relationship Diagram (ERD) of the data model is represented by the image
below:

![database](images/erd_p1.png)

## Project Structure

Repository:

| Files / Folders  |                                     Description                                              |
| :--------------: | :------------------------------------------------------------------------------------------: |
|    test.ipynb    | Displays the first few rows of each table to let you check your database.                    |
| create_tables.py | Drops, creates and also reseting.                                                            |
|    etl.ipynb     | Reads and processes a single file from song_data and log_data and loads the data into tables.|
|      etl.py      | Reads and processes files from song_data and log_data and loads them into your tables.       |
|  sql_queries.py  | Contains all your sql queries, and is imported into the last three files above.              |
|       data       | Folder at the root of the project, with all songs and logs data JSONS.                       |
|      images      | Folder with images used on the project.                                                      |
|    README.md     | File with all instructions and descriptions of the project.                                                                              


### Project Requirements

#### Query to evaluate the requeriment :
  - It’s okay if there are some null values for song titles and artist names in the songplays table. There is only 1 actual row that will have a songid and an artistid.

```
    SELECT * FROM songplays WHERE artist_id <> 'None';
```

<!-- LICENSE -->

## License

Distributed under the MIT License. See `LICENSE` for more information.


<!-- CONTACT -->

## Contact

Djan Magno - djan.magno@gmail.com

Project Link - [https://github.com/djanmagno/Udacity-Data-Engineer-Nanodegree/tree/master/Project-1-Data-Modeling-with-Postgres](https://github.com/djanmagno/Udacity-Data-Engineer-Nanodegree/tree/master/Project-1-Data-Modeling-with-Postgres)