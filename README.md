# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once  at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)
    * There are 983,648 trips in the **bikeshare_trips table**.
```sql
        SELECT COUNT(*)
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```
    * There are 74 stations in the **bikeshare_stations** table.
```sql
SELECT COUNT(*)  FROM `bigquery-public-data.san_francisco.bikeshare_stations`
```
    * Also, there are 74 __distinct__ stations in the **bikeshare_stations** table.
        ```sql
        SELECT COUNT(DISTINCT(*))
        FROM `bigquery-public-data.san_francisco.bikeshare_stations`
        ```
    * There are 107,501,619 rows in the **bikeshare_status** table.
        ```sql
        SELECT COUNT(*)
        FROM `bigquery-public-data.san_francisco.bikeshare_status`
        ```


    I also noticed that BigQuery shows a prediction of the amount of data that will be processed if the query is run.  For the queries where I simply used count(*), BigQuery consults the table metadata for the total rowcount and therefore process 0 B of data.  For the table bikeshare_trip when running count(distinct(*)), BigQuery processed 7.5 MB of data.  This may be an important consideration in case of costs associated with large queries.


- What is the earliest start date and time and latest end date and time for a trip?

    *Earliest* start date and time of 2013-08-29 09:08:00 UTC and *latest* end date and time of 2016-08-31 23:48:00 UTC

```sql
SELECT min(start_date), max(end_date)  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

- How many bikes are there?

    Assuming that bike_number is a unique identifier and that the question is asking for the total number of all bikes that have been used for any trip, there are 700 bikes in the program.

```sql
SELECT count(distinct(bike_number))  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: What is the maximum bike capacity of the system?
  * Answer: 1344 docks are available in the system, assuming that all 74 stations and associated docks are active.
  * SQL query:
```sql
        SELECT sum(dockcount)  FROM `bigquery-public-data.san_francisco.bikeshare_stations`
```


- Question 2:  How many bikes were last available in the area of each landmark?
  * Answer:
```
                             Number of 
          Landmark            Available Bikes
        Mountain View                63
        Palo Alto                    39
        Redwood City                 55
        San Francisco               343
        San Jose                    125
```
  * SQL query:
 ```sql
        SELECT A.landmark, SUM(B.bikes_available)
        FROM `bigquery-public-data.san_francisco.bikeshare_stations` AS A
        INNER JOIN `bigquery-public-data.san_francisco.bikeshare_status` AS B 
        ON A.station_id =  B.station_id
        WHERE B.time = (
              SELECT max(C.time)
              FROM `bigquery-public-data.san_francisco.bikeshare_status` AS C 
              WHERE C.station_id = A.station_id
          )
        GROUP BY A.landmark
        ORDER BY A.landmark ASC
 ```
 

- Question 3: How many trips were taken by subscriber type?
  * Answer:
  ```
      Subscriber Type      Number of Trips
      Customer                 136,809
      Subscriber               846,839
  ```
  * SQL query:
  ```sql
      SELECT distinct(subscriber_type), count(trip_id)
      FROM `bigquery-public-data.san_francisco.bikeshare_trips`
      GROUP BY subscriber_type
```


### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
      - **bikeshare_trips**
    * Answer: 983,648 trips
    * CLI statement:
        ```code
        bq query --use_legacy_sql=false '
             SELECT count(*)
             FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```
      - **bikeshare_stations**
    * Answer: 74 stations
    * CLI statement:
        ```code
        bq query --use_legacy_sql=false '
             SELECT COUNT(*)
             FROM `bigquery-public-data.san_francisco.bikeshare_stations`'
        ```
      - **bikeshare_status**
    * Answer: 107,501,619 rows
    * CLI statement:
        ```code
        bq query --use_legacy_sql=false '
             SELECT COUNT(*)  
             FROM `bigquery-public-data.san_francisco.bikeshare_status`'
        ```    
  * What is the earliest start time and latest end time for a trip?
    * Answer: earliest start = 2013-08-29 09:08:00, latest end = 2016-08-31 23:48:00
    * CLI statement:
    ```code
        bq query --use_legacy_sql=false '
            SELECT min(start_date), max(end_date)  
            FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

  * How many bikes are there?
    * Answer: 700 bikes
    * CLI statement:
    ```code
        bq query --use_legacy_sql=false '
            SELECT count(distinct(bike_number))
            FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```


2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    * Answer:
      Defining **morning** as being before 12:00 noon and a morning trip **ending** in the morning, there were **400,144** trips in the morning and **583,504** trips in the afternoon.
    * CLI statement:
    ```code
        bq query --use_legacy_sql=false '
            SELECT COUNTIF(end_hour < "12:00:00") as morning, 
                    COUNTIF(end_hour >= "12:00:00") as afternoon
              FROM (
                WITH table AS (SELECT start_date, end_date
                FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
                SELECT start_date, EXTRACT(TIME FROM DATETIME(start_date)) as start_hour, 
                        end_date, EXTRACT(TIME FROM DATETIME(end_date)) as end_hour
                FROM table
              )'
    ```
    I originally included start_date to investigate other scenarios trips that bridged noon and midnight, but later dropped these from the analysis.



### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: Which stations are "under-utilized"?

- Question 2: Are there rider zip codes that are under-served (i.e. there few stations in the zip code compared to the number of trips)?  How many rider zip codes are served from existing stations zip codes for SUBSCRIBERS (i.e. better data quality?)?

- Question 3: Which stations are consistently over-utilized (i.e. have low availability)?  This may signal a need to add stations or bikes. 

- Question 4:  How many riders who match the subscriber type of 'Customer' use bikes in the range of 30-45 minutes?  They could be good candidates to market a new 45-minute promotion to.
   
- Question 5:  What are the top X-most ridden bikes and where are they ridden?  If this looks like a "thing", make a promotional game, for example, people who find and ride this bike, contributing to a pattern are eligible to win a prize.

- Possible Additional Questions: 

    What is the concentration of customers/subscribers per bike station?  How does this compare to the distribution of bikes and bike stations?  To the distribution and volume of trips?

    What is the concentration of trips in specific timeslots for specific routes?  If those routes are experiencing "surge", could offering promotions to encourage trips before and after the surge timeslots increase ridership?

    What is the duration of trips between stations?  Are there customers who routinely exceed their alloted time?  Are there routes where customers routinely exceed their alloted time (offer a special price?)?

    Which stations have experienced a growth in number of trips while the station capacity has not changed?

    For which trips did a customer or subscriber return a bike within a certain "excess" period that if they had been offered a promotion might have produced better utilization (ie. the bike just sat there after they returned it) and they might have kept it longer with an offer?

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: Which stations are "under-utilized"?
  * Answer: 70 stations report average hourly availability >80%, of which the top 5 had over 700 hourly occurrences each.
  * SQL query:
    ```sql
        SELECT station_id, count(distinct status_date) as avail_rate
        FROM (
          SELECT station_id,
                 status_date,
                 status_hour, 
                 AVG(availability) as avg_availability
          FROM (
            SELECT station_id, 
                    EXTRACT(DATE FROM time) as status_date,
                    EXTRACT(HOUR FROM time) as status_hour,
                    EXTRACT(MINUTE FROM time) as status_minute,
                    (bikes_available / (bikes_available + docks_available)) as availability, 
            FROM `bigquery-public-data.san_francisco.bikeshare_status`
            WHERE bikes_available > 0 and docks_available > 0
          )
          GROUP BY station_id, status_date, status_hour
          ORDER BY station_id ASC, status_date DESC, status_hour DESC
        )
        WHERE avg_availability > 0.8
        GROUP BY station_id
        ORDER BY avail_rate DESC
    ```


- Question 2:  Are there rider zip codes that are under-served (i.e. there few stations in the zip code compared to the number of trips)?
  * Answer:  The top 9 rider zip codes that were not home to an existing station had more than 10,000 trips each.
  * SQL query:

        First, look at zip codes in scope from bikeshare_trips to see how many will be excluded from the data due to invalid length.

    ```sql
        SELECT COUNT(*)
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        WHERE zip_code IN (
            SELECT DISTINCT zip_code
            FROM `bigquery-public-data.san_francisco.bikeshare_trips`
            WHERE LENGTH(zip_code) != 5
            ORDER BY zip_code ASC
        )
    ```
            NOTE:  39,251 trips out of 983,648 will be excluded due to zip code invalid length, representing 4% of data.

        To visualize "unserved" zip codes in BigQuery Geo Viz:
    ```sql
        SELECT A.zip_code, A.zip_code_geom, B.trip_count
          FROM `bigquery-public-data.geo_us_boundaries.zip_codes` AS A
          JOIN (
              SELECT zip_code, covered, count(trip_id) as trip_count
              FROM (
                SELECT C.trip_id,
                       C.zip_code,
                       C.zip_code IN (
                          SELECT B.zip_code
                          FROM `bigquery-public-data.san_francisco.bikeshare_stations` as A
                          JOIN `bigquery-public-data.geo_us_boundaries.zip_codes` AS B
                          ON ST_CONTAINS(B.zip_code_geom, ST_GeogPoint(A.longitude, A.latitude))
                       ) as covered,
                       C.subscriber_type
                FROM `bigquery-public-data.san_francisco.bikeshare_trips` as C
                ORDER BY C.zip_code ASC
              )
              WHERE covered = FALSE
              GROUP BY zip_code, covered
              ORDER BY trip_count DESC
              LIMIT 50
        ) as B
        ON A.zip_code = B.zip_code
      ```
      
         And to visualize the existing stations in BigQuery Geo Viz:
     ```sql
        SELECT
            station_id,
			ST_GeogPoint(longitude, latitude)  AS WKT,
            dockcount
        FROM `bigquery-public-data.san_francisco.bikeshare_stations`      
     ```


- Question 3:  How many riders who match the subscriber type of 'Customer' use bikes in the range of 30-45 minutes?  They could be good candidates to market a new 45-minute promotion to.
  * Answer:  10,502 Customer rides and 2,398 Subscriber rides.
  * SQL query:
    ```sql
        SELECT subscriber_type,
               count(trip_id)
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        WHERE duration_sec > 30*60
              AND duration_sec < 45*60
        GROUP BY subscriber_type
    ```
  
  
- Question 4:  What are the top X-most ridden bikes and where are they ridden?  If this looks like a "thing", make a promotional game, for example, people who find and ride this bike, contributing to a pattern are eligible to win a prize.
  * Answer:  Generated a table with data for each of the top 5 bikes.  The dataset contains the bike number, the start station id and the number of trips taken for that bike from that station.  I will use this to continue exploring.
  * SQL query:
    ```sql    
        SELECT bike_number, start_station_id, count(trip_id)
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        WHERE bike_number IN (
          SELECT DISTINCT bike_number FROM (
              SELECT bike_number, avg(num_trips) as ave
              FROM (
                SELECT bike_number, count(trip_id) as num_trips
                  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
                GROUP BY bike_number
              )
              GROUP BY bike_number
              ORDER BY ave DESC
              LIMIT 5
           )
         )
         GROUP BY bike_number, start_station_id
         ORDER BY bike_number
    ```
  
---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

