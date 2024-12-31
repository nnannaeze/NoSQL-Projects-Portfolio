# NoSQL-Projects-Portfolio
## 1. Project Overview

### Scenario  
As a Data Engineer at a data analytics consulting firm, my primary role involves:  
- Efficiently handling data across different formats, platforms, and databases.  
- Integrating external datasets into NoSQL databases to support analytical workflows.  
- Problem-solving real-world challenges by designing streamlined data ingestion and retrieval processes.  

### Dataset Overview  
The dataset used for this project is a JSON file of movie information, showcasing various fields including title, genre, release year, and additional metadata. The data file was obtained using the following command:  
```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/labs/FinalProject/movies.json
```
This dataset reflects common real-world requirements, where datasets from public or proprietary sources need to be processed and utilized effectively.
### Goals  
This portfolio aims to demonstrate key competencies in NoSQL database management, including:

- **Importing JSON data into MongoDB**.
- **Querying the imported data to retrieve insights**.
- **Exporting MongoDB data into usable formats**.
- **Loading the JSON data into Apache Cassandra with a well-designed schema**.
- **Querying Cassandra tables for analytical purposes**.
## Tools & Technologies  
This project leverages several tools and technologies to manage NoSQL databases and ensure seamless data processing:

- **MongoDB Tools**:
  - **mongosh**: The MongoDB shell used for interacting with the MongoDB database from the command line.
  - **MongoDB Compass**: A GUI for managing MongoDB databases, useful for visualizing data and running queries.

- **Apache Cassandra Tools**:
  - **cqlsh**: The Cassandra Query Language shell, used to interact with Apache Cassandra from the command line.

- **Environment**:
  - **WSL (Windows Subsystem for Linux)**: A Linux environment running on Windows, used for setting up MongoDB, Apache Cassandra, and executing scripts.

- **Programming Languages**:
  - **Python**: For writing automation scripts, querying databases, and handling data transformations.
  - **Bash**: For shell scripting to automate various database tasks such as imports and exports.

These tools ensure smooth integration, management, and querying of the databases involved in the project.

## Setting the Environment For MongoDB

### 1. Start MongoDB
Before working with MongoDB, we need to start the MongoDB service. This is done by running the following command in the terminal:

```bash
mongod --dbpath /data/db
```
This command starts the MongoDB server and sets the database path to `/data/db`, which is where MongoDB will store its data files.

![img](https://github.com/nnannaeze/NoSQL-Projects-Portfolio/blob/main/nosql%20a3.png)


#### 2. Open MongoDB Shell (mongosh)
Next, open a new terminal window and start the mongosh environment to interact with the MongoDB instance:
```bash
mongosh
```
This opens the MongoDB shell, where you can run MongoDB commands and interact with the database.

### 3. Create an Admin User
To create an admin user for MongoDB, we first enter the following command to enable us to create an admin user:
```javascript
db.createUser({
  user: "admin",
  pwd: "nnannaeze@77",
  roles: [{ role: "root", db: "admin" }]
})
```
Once the admin user is created, we can update the password of the existing `root`user if needed with the following command:
```javascript
db.updateUser("root", { pwd: "nnannaeze@77" });
```
This ensures that the `root` user has the correct password, and now we have an admin user with full access to the MongoDB instance.

## MongoDB Workflow
### 1. **Importing Data into MongoDB**  
To get started with the project, I obtained the dataset in JSON format (movie information) using the following `curl` command:

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/labs/FinalProject/movies.json
```
This command downloaded the movies.json file, which contains data on various movies, including fields such as title, genre, year, director, and more.
![img](2.png)


To import the JSON data into MongoDB, I used the `mongoimport` tool. The command below was executed from the command line:

```bash
mongoimport -u root -p nnannaeze@77 --authenticationDatabase admin --db entertainment --collection movies --file movies.json --host localhost
```

![img](https://github.com/nnannaeze/NoSQL-Projects-Portfolio/blob/main/Untitled%20design%20(2).png)

This command connects to the local MongoDB instance, authenticates the user with the specified password, and loads the `movies.json` file into the `movies` collection in the `entertainment` database.

#### Breakdown of the Command:

- `-u root`: Specifies the username for authentication.
- `-p nnannaeze@77`: The password associated with the specified username.
- `--authenticationDatabase admin`: Defines the database to authenticate against, which in this case is `admin`.
- `--db entertainment`: The target database where the data should be imported. In this case, the target database is named `entertainment`.
- `--collection movies`: Specifies the specific collection within the `training` database where the movie data will be stored. The collection is named `movies`.
- `--file movies.json`: Points to the local file containing the movie data to be imported. In this case, it refers to the `movies.json` file.
- `--host localhost`: Indicates that MongoDB is hosted on the local machine.

This command successfully imports the movie data from `movies.json` into the MongoDB instance. The data is now available in the `movies` collection of the `training` database, ready for further querying and analysis.

---
#### Output
```bash
2024-12-31T11:11:06.830+0100    connected to: mongodb://localhost/
2024-12-31T11:11:07.471+0100    100 document(s) imported successfully. 0 document(s) failed to import.
```
---
### 2. **Querying Data in MongoDB**
I'll get back into the mongosh environment using the command 
```bash
mongosh -u root -p nnannaeze@77 --authenticationDatabase admin
```
After the data was imported, I ran several queries to retrieve valuable insights. Below are some example queries and their descriptions:

**Sample movie document:**
To get a feel of the documents in the colection `movie`, I'll use the `find` and `limit` command 
  ```javascript
  db.movies.find().limit(2)
  ```
This will ouput 


  ```json
  [
  {
    "_id": "3",
    "title": "Split",
    "genre": "Horror,Thriller",
    "Description": "Three girls are kidnapped by a man with a diagnosed 23 distinct personalities. They must try to escape before the apparent emergence of a frightful new 24th.",
    "Director": "M. Night Shyamalan",
    "Actors": "James McAvoy, Anya Taylor-Joy, Haley Lu Richardson, Jessica Sula",
    "year": 2016,
    "Runtime (Minutes)": 117,
    "rating": "unrated",
    "Votes": 157606,
    "Revenue (Millions)": 138.12,
    "Metascore": 62
  },
  {
    "_id": "4",
    "title": "Sing",
    "genre": "Animation,Comedy,Family",
    "Description": "In a city of humanoid animals, a hustling theater impresario's attempt to save his theater with a singing competition becomes grander than he anticipates even as its finalists' find that their lives will never be the same.",
    "Director": "Christophe Lourdelet",
    "Actors": "Matthew McConaughey, Reese Witherspoon, Seth MacFarlane, Scarlett Johansson",
    "year": 2016,
    "Runtime (Minutes)": 108,
    "rating": "unrated",
    "Votes": 60545,
    "Revenue (Millions)": 270.32,
    "Metascore": 59
  }
]


  ```
***Query to find the year with the most number of movie releases:***
  ```javascript
  db.movies.aggregate([
  { $group: { _id: "$year", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 1 }
  ])
  ```
  This query groups the movies by `year` and counts how many movies were released in each year. The results are sorted in descending order, and the top result is returned.
  
  #### Explanation:

  -`$group`: Groups the documents by the year field and calculates the total count of movies released each year.
  
  -`$sum`: 1: Adds 1 for each document grouped by year, effectively counting the number of movies per year.
  
  -`$sort`: Sorts the results in descending order based on the count field, so the year with the most movies appears first.
  
  -`$limit: 1`: Limits the result to only the year with the highest count of movies.
 
---
  #### Output
  ```javascript
  [ { _id: 2016, count: 73 } ]
  ```
 ---   

  
***Query to find the count of movies released after the year 1999:***
```javascript
db.movies.countDocuments({ year: { $gt: 1999 } })
```
  
  
  #### Explanation:
  - This query counts all movies where the year field is greater than 1999, effectively giving the total number of movies released after that year.
  ---    
  #### Output
  ```javascript
  99
  ```
  ---

  ***Query to find the average votes for movies released in 2007:***
  ```javascript
  db.movies.aggregate([
  { $match: { year: 2007 } },
  { $group: { _id: null, averageVotes: { $avg: "$Votes" } } }
  ])
  ```
  
  This query filters the movies for the year 2007 and calculates the average number of votes.
  #### Explanation:

  - `$match`: Filters the documents to include only those where the `year` is 2007.
  
  - `$group`: Calculates the average (`$avg`) of the `Votes` field for movies released in that year. The result is returned as `averageVotes`.
  ---
  #### Output
  ```javascript
  [ { _id: null, averageVotes: 192.5 } ]
  ```
 ---   
###  3. **Exporting Data from MongoDB**
To export a subset of fields from the movies collection into a CSV file, I did the following;
-  I used the `exit` command to return to bash shell,
-  I used the `mongoexport`command with the specific fields. The following command exports the `_id`, `title`, `year`, `rating`, and `director` fields:
```bash
mongoexport -u root -p nnannaeze@77 --authenticationDatabase admin --db entertainment --collection movies --fields _id,title,year,rating,Director --type=csv --out partial_data.csv
```
#### Explanation:

- `--fields _id,title,year,rating,director`: Specifies the fields to include in the export. In this case, only the `_id`, `title`, `year`, `rating`, and `director` fields are chosen.
- `--type=csv`: Specifies the export format as CSV.
- `--out partial_data.csv`: Defines the output file as `partial_data.csv`.

This command exports the specified fields from the `movies` collection to a CSV file. The resulting CSV file can be opened in tools like Excel or used for further analysis.

---
#### Output
```bash
2024-12-31T17:09:01.362+0100    connected to: mongodb://localhost/
2024-12-31T17:09:01.427+0100    exported 100 records
```
## Setting the Environment for Cassandra in WSL
To prepare my environment for working with `Cassandra` in WSL, I performed the following:

#### 1: Started Cassandra
I began by switching to the cassandra user using the following command:
```bash
sudo -i -u cassandra
```
Next, I started the Cassandra server by running:
```bash
/opt/cassandra/bin/cassandra
```
This initialized the Cassandra process and prepared it for accepting queries.
#### 2: Opened the cqlsh Shell
In a new terminal, I launched the Cassandra Query Language Shell (cqlsh) to interact with the database:
```bash
/opt/cassandra/bin/cqlsh
```
This allowed me to execute CQL commands for creating tables, inserting data, and running queries.

---
#### Output

```shell
Connected to Test Cluster at 127.0.0.1:9042
[cqlsh 6.2.0 | Cassandra 5.0.2 | CQL spec 3.4.7 | Native protocol v5]
Use HELP for help.
cqlsh>

```
## Cassandra Schema Design and Data Workflow
This section details the steps I performed to set up a Cassandra environment, create a schema, and load and query data from partial_data.csv.
####  1: Creating the Keyspace and Table
To organize the data efficiently, I created a keyspace named `entertainment` and designed the `movies` table with the following schema:

**Creating the Keyspace**
```sql
CREATE KEYSPACE entertainment 
WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
```
I then set the keyspace for subsequent operations:
```sql
USE entertainment;
```

**Creating the Table**
The `movies` table was designed with appropriate data types for the fields in `partial_data.csv`:
```sql
CREATE TABLE movies (
    id TEXT,
    title TEXT,
    year INT,
    rating TEXT,
    director TEXT,
    PRIMARY KEY (id)
);

```
#### 2: Importing Data from partial_data.csv
The data in `partial_data.csv` was imported into the `movies` table using the following command
```sql
COPY entertainment.movies (id, title, year, rating, director) 
FROM '/root/partial_data.csv' 
WITH HEADER = TRUE;
```
This command ensures the data aligns with the table’s structure and includes the column headers from the CSV file for accurate mapping.
#### 3: Verifying Data Import
To confirm the data import, I executed a query to count the total rows in the `movies` table:
```sql
SELECT COUNT(*) FROM movies;
```

---
#### Output
```sql
count
100
```
![img](https://github.com/nnannaeze/NoSQL-Projects-Portfolio/blob/main/a1.PNG)
---
#### 4. Creating an Index for Efficient Queries
To optimize queries involving the `rating` column, I created an index:
```sql
CREATE INDEX ON movies (rating);

```
To ensure the index was created, I described the `movies` table:
```
DESCRIBE TABLE movies;
```
---
#### Output
![img](https://github.com/nnannaeze/NoSQL-Projects-Portfolio/blob/main/a3.PNG)

The output included a `CREATE INDEX` statement, confirming the index creation.

#### 5: Querying Movies with Rating 'G'
Using the newly created index, I queried the movies table to count the number of `movies` with a rating of `G`:
```sql
SELECT COUNT(*) FROM movies WHERE rating = 'G';
```

---
#### Output
```sql
count
34
```
This workflow highlights my proficiency with Cassandra, including schema design, data import, and executing optimized queries.
  

---

## Conclusion
This portfolio demonstrates my proficiency in working with NoSQL databases, particularly MongoDB and Apache Cassandra, for handling diverse datasets in various formats. By performing key tasks such as data importation, querying, exporting, and schema design, I have showcased my ability to adapt to the unique features and requirements of each NoSQL platform.

In this project, I imported a movie dataset into MongoDB, executed complex aggregation queries, and exported the data into CSV for further analysis. Additionally, in Apache Cassandra, I designed a suitable schema, imported data from a CSV file, and created indexes to enhance query performance. These tasks reflect the practical skills necessary for efficiently managing and retrieving insights from large volumes of unstructured data.

Through this hands-on experience, I have not only gained a deeper understanding of NoSQL technologies but also demonstrated my ability to design and implement workflows that support data ingestion, processing, and analytical capabilities for real-world applications. This project has equipped me with the tools and knowledge to handle diverse datasets, and I am now capable of managing data across multiple databases and environments, providing strong support for data-driven decision-making processes.
