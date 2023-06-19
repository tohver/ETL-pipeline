# Data Warehouse with AWS Redshift

## Summary

 - [Introduction](#introduction)
 - [Getting started](#getting-started)
 - [The ETL Process](#the-etl-process)

 - [Analyzing the results](#analyzing-the-results)
 - [The database structure](#the-database-structure)

## Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want 
to move their processes and data onto the cloud. Their data resides in S3, in a directory 
of JSON logs on user activity on the app, as well as a directory with JSON metadata 
on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts 
their data from S3, stages them in Redshift, and transforms data into a set of 
dimensional tables for their analytics team to continue finding insights in what songs 
their users are listening to. You'll be able to test your database and ETL pipeline 
by running queries given to you by the analytics team from Sparkify and compare your 
results with their expected results

Used AWS Web Services:
* AWS S3
* AWS Redshift

The ETL process transforms the raw log files of the Sparkify app to a structured database schema. The Schema enables the storage of analytical data that can be easily queried and analyzed.

The data sources are in two public S3 buckets:
1. Songs bucket (s3://udacity-dend/song_data)
2. Event bucket (s3://udacity-dend/log_data)

The data are in JSON files.

<b>Log Dataset structure:</b>
![Log Dataset](./images/log_dataset.jpg)

<b>Song dataset structure:</b>
~~~~
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null
, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", 
"title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
~~~~


## Database Structure
#### Staging tables

##### TABLE staging_songs

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|num_songs| int| |
|artist_id| varchar| |
|artist_latitude | decimal | |
|artist_longitude| decimal| |
|artist_location| varchar| |
|artist_name | varchar | |
|song_id| varchar| |
|title| varchar| |
|duration | decimal | |
|year | int | |


##### TABLE staging_events

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|artist| varchar| |
|auth| varchar| |
|firstName | varchar | |
|gender| varchar| |
|itemInSession | int| |
|lastName | varchar | |
|length| decimal| |
|level| varchar| |
|location | varchar| |
|method | varchar| |
|page | varchar | |
|registration| varchar| |
|sessionId| int| |
|song | varchar| |
|status| int| |
|ts| timestamp| |
|userAgent| varchar| |
|userId| int| |


#### Dimension tables

##### TABLE users

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|user_id| int| distkey, PRIMARY KEY |
|first_name| varchar| |
|last_name | varchar | |
|gender| varchar| |
|level| varchar| |

##### TABLE songs

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|song_id| varchar| sortkey, PRIMARY KEY |
|title| varchar| NOT NULL |
|artist_id | varchar | NOT NULL|
|duration| decimal| |

##### TABLE artists

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|artist_id| varchar| sortkey, PRIMARY KEY |
|name| varchar| NOT NULL |
|location | varchar | |
|latitude| decimal| |
|logitude| decimal| |


##### TABLE time

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|start_time| timestamp| sortkey, PRIMARY KEY |
|hour| int| |
|day| int| |
|week| int| |
|month| int| |
|year| int| |
|weekday| int| |

#### Fact table

##### TABLE songplays

| COLUMN | TYPE | FEATURES |
| ------ | ---- | ------- |
|songplay_id| int| IDENTITY (0,1), PRIMARY KEY |
|start_time| timestamp| REFERENCES  time(start_time)    sortkey|
|user_id | int | REFERENCES  users(user_id) distkey|
|level| varchar| |
|song_id| varchar| REFERENCES  songs(song_id)|
|artist_id | varchar | REFERENCES  artists(artist_id)|
|session_id| int| NOT NULL|
|location| varchar| |
|user_agent| varchar| |
