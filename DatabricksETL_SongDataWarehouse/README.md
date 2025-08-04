# Delta Lake Project

## Benjamin Acorda, Jaden Astle, Aamnah Malik

### ERD Diagram

![Updated Process](ERD.png)

### Diagram Update

![Updated Process](diagram2.png)

### Purpose and Process

#### Why Delta Lake?

The purpose of a delta lake table is to keep records of the changes made to a dataset. These changes are saved, allowing users to go "back in time" and view previous versions of data. Delta lakes enable ACID transactions - atomicity, consistency, isolation, and durability. They also enforce schema on any write, meaning that data additions adhere to the predefined structure of a database. While large amounts of data can be handle by these units, querying is still efficient due to optimized techniques such as data skipping. This prevents the entirety of the data to be scanned when not necessary, saving computational power during queries. In order to create the delta tables, the data had to be imported first. Because the data was stored in JSON files (key-value pairs), each row in the resulting table represents one JSON file of information, with the keys corresponding to column placements and the values corresponding to the row's information. The JSON files were split into a total of five different tables, resembling the setup of a star schema. 

#### Database Design

As stated previously, the database design follows a star schema. A star schema is a data warehousing model that contains a centralized fact table with connecting dimension tables. This setup optimizes query performance and simplifies the organizational structure of data. The centralized table contains IDs for song plays, datetime values, users, levels, songs, artists, sessions, locations, and user agents. These IDs are used to connect to dimension tables that give more information - for example, the ```artist_id``` column can connect to the artist dimension table, which contains information such as ```artist_name``` and and ```location```. This architecture makes it easy to differentiate different aspects of the company and its data. This easy separation allows for quick analysis and interpretable structure. The overall process involves extracting the JSON files from S3, loading the data from these files into a collection of data lake tables, and finally constructing the set of dimensional tables (star schema) for further anaylsis and Snowflake connection.

### Snowflake vs. Databricks

While Snowflake is a powerful tool, Databricks is far better suited for the needs of Sparkify. The ETL process we followed keeps the transformation and loading processes contained to one product and maintains an ACID-compliant storage format throughout this. Databricks & PySpark allow simple and efficient querying, reading, and writing of data in a way that eliminates the need for Snowflake. If Sparkify were looking for BI integration or more advanced analytics rather than batch analytics, it may be worth looking into getting Snowflake. However, at this stage, it would become a redundant cost and would not provide the company with increased analytical capabilities. It may even result in data duplication and thus greater overhead costs.

### Summary

We first began by importing several JSON files from S3 buckets. These JSON files were then parsed and converted into a tabular format, specifically as delta lake tables. Data manipulation and transformation were then conducted on these tables to prepare the data for analysis. Once these tables were transformed, the star schema was set up, with the fact table being the centralized table and the four dimension tables being users, artists, time, and songs. This star schema was then used for analytical insights, where the structure of the schema was taken advantage of to better understand the behavior of listeners.

### Example Queries
- Song title length statistics
 
```sql
select min(length(title)), avg(length(title)), max(length(title)) from songs_dim
```
 
- Percentage of paid users
 
```sql
select count(case when level = 'paid' then 1 end) / count(*) as paid_user_ratio from users_dim;
```
 
- Most popular listening days
 
![Updated Process](graph.png)

```sql
select dayofmonth, count(\*) from time_dim group by dayofmonth order by count(\*) desc
```
 
- Most popular first name
 
```sql
select firstName, count(*) as count from users_dim group by firstName order by count desc;
```
 
- Most popular last name
 
```sql
select lastName, count(*) as count from users_dim group by lastName order by count desc;
```
 
- Gender distribution of users
 
```sql
select gender, count(*) as count from users_dim group by gender;
```