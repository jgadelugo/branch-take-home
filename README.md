# Branch Take Home

## Getting Started:

The goal is wrangle through some data, and test your basic knowledge of data engineering.
For this project, we will we using  the public *randomuser.me* API. 
This [API Endpoint](https://randomuser.me/api/?results=500) should be used for all the tasks

## Tasks:

1. Using the json response from the endpoint. Model, and design the database table/tables.
	-	You can use [dbdiagram](https://dbdiagram.io/d) to generate the ERD (Entity Relationship Diagram)
	-	_Deliverable_: Export of ERD

2. Build an end to end process in Python3, that generates a csv file for each of the tables you have designed
	-  The JSON results must be flatten
	-  The column names must contain only letters (a-z, A-Z), numbers (0-9), or underscores (_)
	-  _Deliverable_: The python code, that can generate the csv files. 

3. Now imagine this is a production ETL process. How would you design it? What tests would you put in place?
  - _Deliverable_: Any diagrams/text files that can show us your design. 

Please place the deliverables from all the tasks in a git repo and send us a link to review.

If you have any questions, concerns or suggestions please feel free to contact us. Happy coding!


## files provided
* run_main.sh - runs requirments.txt, screts.sh and collect_users_data.py
  * Sets up enviroment and creates csv files from data
* secrets.sh
  * sets up salts enviroment variables for password and sensitive data hashing
  * would not include this file in production
* collect_users_data.py
  * collect data from end point, creates tables and saves them as csv files
* branchTakeHomeERD.pdf
  * ERD (Entity Relationship Diagram)
* {date}_{table_name}.csv
  * files containing table data for specific day
  * naming conention might change if needs to be collected multiple times per day


## How to run

linux environment run 
```
. ./run_main.sh
```
Otherwise make sure pandas, requests, hashlib are installed and run
```
. ./secrets.sh
./collect_users_data.py
```

## Additional work to productionize ETL
* How would you design it?
  * If it is user input data when creating or updating an account
    * use cloud function that uses event listner for when data gets added then run script to add data to tables/save file
  * If its a daily pull
    * create cloud function and schedule using pub/sub and cloud scheduler
  * After initial data is loaded
    * We could create a slow changing dimensions table (t2 table) with data to track change over time. We can then use this to get most up to date data for account info, or track users with historical changes for analysis.
  * Add monitoring and alerts for ETL success/fail and checking data integrity
  * Other pipeline tools:
    * ariflow can be used for ETL as well
* What tests would you put in place?
  * I would add tests for data integrity and any anomolies, adding logging if any are detected
  * test and logging for requests fail and request status
  * test and logging for data changes. New columns or change of column names.
  * test primary keys, when new data is added doesnt duplicate primary keys
* hashed sensitive data, not sure if we would need the information in it. Safer to hash if not needed.
* secret with salt could be stored in GCP secret manager
* IAM permissions limited to sensitive data, and PII

## Other resources 
* [Google Colab - Testing environment](https://colab.research.google.com/drive/1CO0fApzNRlvXlEJgPNQRKPhii9y-GY4Q#scrollTo=80J2f_OGFVaM)
* [Google Sheet - Table Breakdown and T2 Table example](https://docs.google.com/spreadsheets/d/1zdA64y-mFwKdnYPVyF93j0a6BGOLEkV8yd_dv5eTcJ4/edit?usp=sharing)
  
## Author
* [Jose Alvarez de Lugo](https://github.com/jgadelugo)
