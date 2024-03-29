<details><summary> 01 - Topic Introduction </summary>
<p>
  
- A music streaming startup needs to scale up their user base and song database. To achieve this, they’re looking to migrate their processes and data onto the cloud. Their data  currently resides in AWS S3 bucket. This bucket contains two folders: one with JSON files recording user activity within the app, and another with JSON files containing metadata for all the songs available.
- Task is to build an ETL Pipeline that extracts their data from S3, staging it in Amazon Redshift and then transforming data into a set of Dimensional and Fact Tables for their Analytics Team to continue finding insights to what songs their users are listening to.

<img width="925" alt="image" src="https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/88d74bb6-5bbf-4ddd-8393-23268a6560e9">


</p>
</details> 

<details><summary> 02 - Dataset Overview </summary>
<p>
  <details><summary> 2.1 Data Sample - Song Data Path → s3://udacity-dend/song_data </summary>
  <p>

  {"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1",   "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

  </p>
  </details> 

  <details><summary> 2.2 Data Sample - Log Data Path → s3://udacity-dend/log_data </summary>
  <p>
{"artist":null,"auth":"Logged In","firstName":"Walter","gender":"M","itemInSession":0,"lastName":"Frye","length":null,"level":"free","location":"San Francisco-Oakland-Hayward, CA","method":"GET","page":"Home","registration":1540919166796.0,"sessionId":38,"song":null,"status":200,"ts":1541105830796,"userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"","userId":"39"}
  </p>
  </details> 

  <details><summary> 2.3 Log Data JSON Path → s3://udacity-dend/log_jason_path.json</summary>
  </details> 
</p>
</details> 

<details><summary> 03 - Main Idea Development of the solution </summary>
<p>
  <details><summary> 3.1 - Schema Design for Song Play Analysis </summary>
  <p>
  A Star Schema would be required for optimized queries on song play queries.
    <details><summary> Fact Table </summary>
    <p>
    **songplays** - records in event data associated with song plays i.e. records with page NextSong songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
    </p>
    </details> 
    
    <details><summary> Dimension Tables </summary>
    <p>
    - **users** - users in the app user_id, first_name, last_name, gender, level
    - **songs**- songs in music database song_id, title, artist_id, year, duration
    - **artists** - artists in music database artist_id, name, location, lattitude, longitude
    - **time** - timestamps of records in songplays broken down into specific units start_time, hour, day, week, month, year, weekday
    </p>
    </details> 
    
  </p>
  </details> 

  <details><summary> 3.2 - Create Table Schema </summary>
  <p>
  - Instead of reading data directly from the s3 buckets into the final database, this project will make use of a staging table to act as an intermediary between the s3 bucket and the final database.
  - There are two staging tables staging_events and the staging_songs tables. These tables are to temporally hold data from the S3 Bucket before being transformed and inserted into the primary use tables.
    ![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/2452b202-19e6-4e44-94c3-1b90999d8a84)
    
![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/a6ea0dbf-503a-42b6-9164-5b03a92a6ebb)

  </p>
  </details> 

  <details><summary> 3.3 - Build ETL Pipeline </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 3.4 - Tool Use</summary>
  <p>
  
  </p>
  </details> 
</p>
</details> 

<details><summary> 04 - Processing Steps </summary>
<p>
  <details><summary> 4.1 - Configure aws (connect aws to local machine) </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.2 - Create IAM user role and attach needed permission policies  </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.3 - Create AWS Cluster </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.4 - Authorize Security Access Group to Default TCP/IP Address - AWS VPC configuration</summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.5 - Set up the main dwhhuyen.cfg </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.6 - Run the create_table script to set up the database staging and analytical tables </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 4.7 - Run the etl.py script to extract data from the files in S3, stage it in redshift, and finally store it in the dimensional tables. </summary>
  <p>
  
  </p>
  </details> 
</p>
</details> 

<details><summary> 05 - Results & Recommendations </summary>
<p>
  <details><summary> 5.1 - Results </summary>
  <p>
  
  </p>
  </details> 

  <details><summary> 5.2 - Recommendations </summary>
  <p>
  
  </p>
  </details> 

</p>
</details> 
