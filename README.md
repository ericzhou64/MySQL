"# MySQL data import and export" 

Installation - First install a stand alone SQL to my local PC 

After make sure I have MySQL installed and server is running, Perform the following tasks:


1.Open Command Line and connect to the MySQL server
mysql -u root -p

2.Run some SQL Commands
SHOW DATABASES;
USE sakila;

3. Make copy of actor table
CREATE TABLE actors_copy LIKE actor;
INSERT INTO actors_copy SELECT * FROM actor;

4.Check if data has been copied to new actors_copy table
SELECT * FROM actors_copy limit 10;

5.Create a new database and import 
CREATE DATABASE IF NOT EXISTS mydemo;
USE mydemo;
CREATE TABLE cve (
	id INT AUTO_INCREMENT PRIMARY KEY,
    	Exploit_DB_ID VARCHAR(50),
    	CVE_ID VARCHAR(50)
);

6. Got Error, try to add permission under [mysqld] in my.ini to allow import at server
local_infile=1

Restart server and  start client with --local-infile flag enabled
mysql -u root -p --local-infile=1


7.import data from scraped cve data

LOAD DATA LOCAL INFILE 'C:/temp/cvedata.csv'
INTO TABLE cve
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(Exploit_DB_ID, CVE_ID);

8.Confirm
SELECT * FROM cve limit 10;

9.Backup database before doing anything
mysqldump -u root -p mydemo > backup_mydemo.sql

10 Create a new database called mydemo2
CREATE DATABASE mydemo2;

10.Import from backup
mysqldump -u root -p mydemo > backup_mydemo.sql

11. Check to confirm
SELECT * FROM cve limit 10;


