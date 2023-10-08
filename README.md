# Bank Scenario - Apache NiFi Project
## Overview
This Apache NiFi project simulates a bank scenario where customer transactions are tracked and joined with user login information. It includes data generation scripts, Kafka integration, SQL database retrieval, stream processing, and data insertion into MongoDB.

# Table of contents
- [Prerequisites](#Prerequisites)
- [Usage](#usage)
- [Project Structure](#Project-Structure)
- [Pipeline Structure](#Pipeline-Structure)


# Table of contents

- [Usage](#usage)
  - [Flags](#flags)
    - `-1`
    - `-a`   (or) `--all`
    - `-A`   (or) `--almost-all`
    - `-d`   (or) `--dirs`
    - `-f`   (or) `--files`
    - `-h`   (or) `--help`
    - `-l`   (or) `--long`
    - `-r`   (or) `--report`
    - `--tree` (or) `--tree=[DEPTH]`
    - `--gs` (or) `--git-status`
    - `--sd` (or) `--sort-dirs` or `--group-directories-first`
    - `--sf` (or) `--sort-files`
    - `-t`
  - [Combination of flags](#combination-of-flags)
- [Installation](#installation)
- [Recommended configurations](#recommended-configurations)
- [Custom configurations](#custom-configurations)
- [Updating](#updating)
- [Uninstallation](#uninstallation)
- [Contributing](#contributing)
- [License](#license)

# Prerequisites
[(Back to top)](#table-of-contents)

Before using this project, ensure you have the following prerequisites installed:

- Apache NiFi
- Kafka
- MySQL
- MongoDB

# Usage
[(Back to top)](#table-of-contents)

This setup ensures that the data from both streams is joined based on the user_id attribute, and the final output follows the specified schema. 
Th goal is to adjust the NiFi processors and configurations accordingly to achieve this join operation and schema mapping.

1. Clone this repository to your local machine:
   ```sh
    git clone https://github.com/saraAhangari/Apache-nifi.git
    ```
2. Navigate to the project directory:
   ```sh
    cd Apache-nifi
    ```
3. Start Apache NiFi and configure the NiFi flows by importing the provided template.

4. Configure the Kafka producer to produce data to the appropriate topics (transactions and user_app_login).

5. In this project,there's a data processing flow that joins the two streams based on the user_id attribute. There for the defined rule for joining is to match records with the same user_id.
6. Final Schema will be like this:
   ```sh
   {
    "transaction_id": 1000007, 
    "login_id": 1407, 
    "amount": 20809, 
    "registered_at": "2018-11-10 11:00:14.0", 
    "created_at": "2021-10-31 15:20:32", 
    "user_id": 7, 
    "login_at": "2021-10-31 15:10:32", 
    "username": "jennifer_clark"}
   ```
7.Insert the joined and formatted data into MongoDB. 

# Project Structure
[(Back to top)](#table-of-contents)
The project directory is organized as follows:

- producer.py: Python script to generate data as Kafka Producer into our 2 topics: transactions and users_app_login. 
- mysql_fake_generator.py: Python script to generate fake user data and populate the MySQL database.
- clean_json_two_stream.py: Python script that is used in a processor of nifi pipeline to join two different schemas from the topics. 
- KafkaToMongoDB.xml: The nifi pipeline template. 
- mysql-connector-java-8.0.29.jar: The jar file that is used in a processor to fetch data from MySQL. 
- nifi-logs/: The folder to keep the unmatched records. 
- README.md: This README file.

# Pipeline Structure
[(Back to top)](#table-of-contents)
The Nifi processors that are used:
   - Kafka Consumer Processor: This processor consumes data from Kafka topics.
   - ExecuteSQL Processor: This processor executes SQL queries against a database and retrieves the results as FlowFiles. It is often used for database interactions and data retrieval.
   - PutFile Processor: The PutFile processor writes FlowFiles to the local file system or a network location. It is useful for exporting data to files for archival or further processing.
   - PutMongo Processor: This processor inserts or updates documents in a MongoDB database. It is commonly used for integrating NiFi flows with MongoDB databases.
   - RouteOnAttribute Processor: The RouteOnAttribute processor routes FlowFiles based on the values of specified attributes. It allows conditional routing of data based on attribute values.
   - SplitJson Processor: SplitJson splits a FlowFile containing a JSON array into individual FlowFiles for each element in the array. This is useful for processing individual JSON elements separately.
   - ConvertAvroToJson Processor: This processor converts Avro data format to JSON format. It is handy when working with Avro data and needing it in JSON format for further processing.
   - LogMessage Processor: LogMessage is used for generating log messages in the NiFi log file. It's often used for debugging and monitoring purposes.
   - EvaluateJsonPath Processor: EvaluateJsonPath extracts data from JSON-formatted content using JSONPath expressions. It helps retrieve specific values from JSON documents.
   - MergeContent Processor: MergeContent combines multiple FlowFiles into a single FlowFile. It's commonly used for aggregating data before further processing.
   - ExecuteScript Processor: ExecuteScript allows you to execute custom scripts (e.g., Groovy, Python) on FlowFiles. It provides flexibility for custom data transformations and processing logic.

