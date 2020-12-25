# Homework #03

## Overview
1. Name / Title: (Popular Baby names)
2. Link to Data: (https://data.cityofnewyork.us/Health/Popular-Baby-Names/25th-nujf )
3. Source / Origin: 
	* Author or Creator: Department of Health and Mental Hygiene (DOHMH)
	* Publication Date: 12/13/2013
	* Publisher: NYC Open Data
	* Version or Data Accessed: June 8, 2020
4. License: Open Data Commons Public Domain Dedication and License
5. Can You Use this Data Set for Your Intended Use Case? (yes)
## Table Design
   I use id as my primary key and auto-increment,
    babyname is text,
    yob (stands for year of birth) is integer,
    gender(gender of the baby) is text,
    ethnicity is text,
    namecount (number of the name count for baby) is integer,
    ranking is integer
## Import
import csv
import os
import sqlite3
import time

def get_csv_data(path):
    file = open(path)
    reader = csv.reader(file)
    return  reader

def insert_data(reader):
    conn = sqlite3.connect("D:/PycharmProjects/Test01/homework03.db")
    c = conn.cursor()
    i = 0
    for row in reader:
        if i == 0:
            i = 1
            continue
        else:
            i = i+1
        babyname = row[3].replace('\'', '')
        sql = "insert into babyname (babyname,yob,gender,ethnicity,namecount,ranking) values ('" + babyname + "'," + \
              row[0] + ",'" + row[1] + "','" + row[2] + "'," + row[4] + "," + row[5] + ")"
        c.execute(sql)
        conn.commit()
        # time.sleep(0.1)
        print("当前数据插入到第:" + str(i) + "条")
    c.close()
    conn.close()
    
if __name__ == '__main__':
    path = "D:/PycharmProjects/Test01/popular_baby_names.csv"
    reader = get_csv_data(path)
    insert_data(reader)

## Database Information
1. select * from sqlite_master
type    name             tbl_name         rootpage    sql 
table	babyname	     babyname	      2	          CREATE TABLE babyname(id  integer PRIMARY KEY AUTOINCREMENT NOT NULL,babyname text,yob integer,gender text,ethnicity text,namecount integer, ranking integer)
table   sqlite_sequence  sqlite_sequence  3           CREATE TABLE sqlite_sequence(name,seq) 

## Query Results
1.#the total number of rows in the database
select count(*) from babyname  #Query total number of records
count(*)
27492

2.#Query the first 15 records
select * from babyname order by id desc  limit 15 
id      babyname yob    gender  ethnicity                   namecount  ranking
28204	Alessia	 2015	FEMALE	WHITE NON HISPANIC       	12	        81
28203	Isaac	 2016	MALE	ASIAN AND PACIFIC ISLANDER	21       	48
28202	Mendel	 2015	MALE	WHITE NON HISPANIC	        42	        64
28201	Yaritza  2015	FEMALE	HISPANIC	                12	        79
28200	Alayna 	 2016	FEMALE	HISPANIC	                10	        74
28199	Joel	 2015	MALE	WHITE NON HISPANIC	        32	        72
28198	Joselyn	 2015	FEMALE	HISPANIC	                12	        79
28197	Arthur	 2015	MALE	WHITE NON HISPANIC       	29	        75
28196	Cheskel	 2016	MALE	WHITE NON HISPANIC	        20	        89
28195	Nathalie 2016	FEMALE	HISPANIC	                11	        73
28194	Layla	 2016	FEMALE	BLACK NON HISPANIC         	25	        28
28193	Otto	 2016	MALE	WHITE NON HISPANIC        	12	        97
28192	Antonio	 2016	MALE	HISPANIC	                10	        91
28191	Kira	 2015	FEMALE	ASIAN AND PACIFIC ISLANDER	12	        41
28190	Adrian	 2016	MALE	WHITE NON HISPANIC	        47	        64

3.#Modify a value in a column
update babyname set namecount=50 where id=28204 

4.#Query the first 15 records #Displays the first 15 rows, but only three columns
select babyname,yob,gender from babyname order by id desc  limit 15 

5.#add a new column without a default value
alter table babyname add school text default null

6.#set the value of that new column
update table set school='hafu' where id=28205

7.#show only the unique (non duplicates) of a column of your choice
SELECT DISTINCT babyname from babyname

8.#group rows together by a column value (your choice) and use an aggregate function to calculate something about that group
SELECT avg(ranking),sum(namecount) from babyname GROUP BY babyname

9.#using the same grouping query or creating another one
select * from babyname a where exists(select babyname from babyname where a.babyname = babyname group by babyname having count(babyname)>=2)



