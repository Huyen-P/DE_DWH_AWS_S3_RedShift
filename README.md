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
    
  ```
  {
    "num_songs": 1, 
    "artist_id": "ARJIE2Y1187B994AB7", 
    "artist_latitude": null, 
    "artist_longitude": null, 
    "artist_location": "", 
    "artist_name": "Line Renaud", 
    "song_id": "SOUPIRU12A6D4FA1E1",   
    "title": "Der Kleine Dompfaff", 
    "duration": 152.92036, 
    "year": 0
  }
  ```

  </p>
  </details> 

  <details><summary> 2.2 Data Sample - Log Data Path → s3://udacity-dend/log_data </summary>
  <p>
    
  ```
  {
    "artist":null,
    "auth":"LoggedIn",
    "firstName":"Walter",
    "gender":"M",
    "itemInSession":0,
    "lastName":"Frye",
    "length":null,
    "level":"free",
    "location":"San Francisco-Oakland-Hayward,CA",
    "method":"GET",
    "page":"Home",
    "registration":1540919166796.0,
    "sessionId":38,
    "song":null,
    "status":200,
    "ts":1541105830796,
    "userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143   Safari\/537.36\"",
    "userId":"39"
  }
  ```

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
    
  - A Star Schema would be required for optimized queries on song play queries.
    
![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/a6ea0dbf-503a-42b6-9164-5b03a92a6ebb)

<details><summary> Fact Table </summary>
    <p>
      
  - **songplays** - records in event data associated with song plays i.e. records with page NextSong songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

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

<details><summary> Steps </summary>
  <p>
    
  1. Write a SQL CREATE statement for each of these tables in sql_queries2.py
  2. Complete the logic in create_tables.py to connect to the database and create these tables
  3. Write SQL DROP statements to drop tables in the beginning of create_tables.py if the tables already exist. This way, you can run create_tables.py   whenever you want to reset your database and test your ETL pipeline.
  4. Launch a redshift cluster and create an IAM role that has read access to S3.
  5. Add redshift database and IAM role info to dwhhuyen.cfg.
  6. Test by running create_tables.py and checking the table schemas in your redshift database.
     
  </p>
  </details> 
  
  </p>
  </details> 

  <details><summary> 3.3 - Build ETL Pipeline </summary>
  <p>
    
  1. Implement the logic in etl.py to load data from S3 to staging tables on Redshift.
  2. Implement the logic in etl.py to load data from staging tables to analytics tables on Redshift.
  3. Test by running etl.py after running create_tables.py and running the analytic queries on your Redshift database to compare your results with the expected results.
  4. Delete your redshift cluster when finished.
     
  </p>
  </details> 

  <details><summary> 3.4 - Tool Use</summary>
  <p>
    
  - AWS Redshift
  - AWS VPC
  - SQL 
  - Python
  - Anaconda Prompt
  - Visual Studio Code
    
  </p>
  </details> 
</p>
</details> 

<details><summary> 04 - Processing Steps </summary>
<p>
  <details><summary> 4.1 - Configure aws (connect aws to local machine) </summary>
  <p>
    
![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/d567d250-9fe1-40ca-b726-ff9fa6d77780)


  </p>
  </details> 

  <details><summary> 4.2 - Create IAM user role and attach needed permission policies  </summary>
  <p>
    
  ![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/e80ff46c-0580-4b2c-ad10-b9b8cb817cb3)

  </p>
  </details> 

  <details><summary> 4.3 - Create AWS Cluster </summary>
  <p>
    
  - **Using Cloud Shell**
  ```
  aws redshift create-cluster --node-type ra3.xplus --number-of-nodes 2 --master-username adminuser --master-user-password TopSecret1 --cluster-identifier mycluster
  ```
  </p>
  </details> 

  <details><summary> 4.4 - Authorize Security Access Group to Default TCP/IP Address - AWS VPC configuration</summary>
  <p>

  <details><summary> VPC Review </summary>
  <p>

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/f050f22c-309a-4033-a6ca-ce25df236214)

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/81183099-9ad5-4a96-a3db-90264f23d020)

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/ddcd9dce-5e27-4e13-b5f7-0572282e71e1)


  </p>
  </details> 
    
  <details><summary> Internet Gateway - Being attached to VPC </summary>
  <p>

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/d0d02704-dbc0-4744-8c8a-9f3c1097f18c)

  </p>
  </details> 

  <details><summary> Route Tables </summary>
  <p>

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/6f141629-8fd8-40a6-931a-cf3f16df91b4)

  </p>
  </details> 

  <details><summary> Security Group </summary>
  <p>
  <details><summary> outbound rules </summary>
  <p>

  ![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/70505a51-d6ab-4cc7-9794-4abef1115c2f)

  </p>
  </details> 

  <details><summary> inbound rules </summary>
  <p>

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/5e6f1f20-52ae-4f0a-87d2-5439ed150a90)

  </p>
  </details>
  
</p>
</details> 

</p>
</details> 


  <details><summary> 4.5 - Set up the main dwhhuyen.cfg </summary>
  <p>

```
[CLUSTER]
HOST=
DB_NAME=
DB_USER=
DB_PASSWORD=
DB_PORT=

[IAM_ROLE]
ARN='IAM Role arn'

[S3]
LOG_DATA='s3://udacity-dend/log_data'
LOG_JSONPATH='s3://udacity-dend/log_json_path.json'
SONG_DATA='s3://udacity-dend/song_data'

[AWS]
KEY=
SECRET=
REGION_NAME=
```

  </p>
  </details> 

  <details><summary> 4.6 - Run the create_table script to set up the database staging and analytical tables </summary>
  <p>
    
  ![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/3de7dedb-4d48-44b4-af67-355003ea99e4)

  </p>
  </details> 

  <details><summary> 4.7 - Run the etl.py script to extract data from the files in S3, stage it in redshift, and finally store it in the dimensional tables. </summary>
  <p>
  
  </p>
  </details> 
  
</p>
</details> 

</p>
</details> 

<details><summary> 05 - Results & Recommendations </summary>
<p>
  <details><summary> 5.1 - Results </summary>
  <p>
- Number of rows in each table 

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/eca252ef-dac0-4086-ba3d-11ffc075b568)

![image](https://github.com/Huyen-P/DE_DWH_AWS_S3_RedShift/assets/72473316/b73147e6-2093-438f-8890-111f34afff9e)


  </p>
  </details> 

  <details><summary> 5.2 - Recommendations </summary>
  <p>
  
  </p>
  </details> 

</p>
</details> 
