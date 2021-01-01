## Data Modeling

## Project: Data Modeling with Apache Cassandra

## Table of Contents

- [Introduction]
- [Description]
- [Data]
- [Part I: ETL Pipeline for Pre-Processing]
- [Part II: Apache Cassandra Coding Portion]
  - [Creating a Cluster]
  - [Create Keyspace]
  - [Set Keyspace]
- [Data Modeling]
- [Files]
- [Software Requirements]

## Introduction

Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analysis team is particularly interested in understanding what songs users are listening to. Currently, there is no easy way to query the data to generate the results, since the data reside in a directory of CSV files on user activity on the app.

They'd like a data engineer to create an **Apache Cassandra** database which can create queries on song play data to answer the questions. Our role is to create a database for this analysis. We'll be able to test the database by running queries given by the analytics team at Sparkify.

## Description

In this project, we'll apply the concepts learned in data modeling with Apache Cassandra and complete an ETL pipeline using Python. I will model the data by creating tables in Apache Cassandra to run queries. We are provided with part of the ETL pipeline that transfers data from a set of CSV files within a directory to create a streamlined CSV file to model and insert data into Apache Cassandra tables.

## Data

We'll be working on one dataset: *event_data*. The directory of CSV files partitioned by date. Here are examples of filepaths to two files in the dataset:

event_data/2018-11-08-events.csv
event_data/2018-11-09-events.csv

## Part I: ETL Pipeline for Pre-Processing

Data currently resides in a directory of JSON files. We need to create a streamlined CSV file from all these. The final file will be used to extract and insert data into Apache Cassandra tables.

The code to pre-process the CSV files was provided already. So no need to go in-depth for it.

This is what the file *event_datafile_new.csv* looks like. It has 6821 rows and contains the following columns.

- artist 
- firstName of user
- gender of user
- item number in session
- last name of user
- length of the song
- level (paid or free song)
- location of the user
- sessionId
- song title
- userId

## Part II: Apache Cassandra Coding Portion

We will model our data based on the queries provided to us by the analytics team. But first, let's setup Apache Cassandra for this. This is a three step process:

### Create a Cluster

We create a cluster and connect it to our local host. This makes a connection to a Cassandra instance on our local machine.

from cassandra.cluster import Cluster
cluster = Cluster(['127.0.0.1'])

# To establish connection and begin executing queries, need a session
session = cluster.connect()

### Create a Keyspace

session.execute(""" CREATE KEYSPACE IF NOT EXISTS udacity
WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1}""")

### Set Keyspace

session.set_keyspace("udacity")

### Data Modeling

We model our data **based on the queries we will perform. This is because we shouldn't scan the entire data because it is distributed on multiple nodes. It will slow down our system because sending all of that data from multiple nodes to a single machine will crash it.

Now we will create the tables to run the following queries.

1. Give me the artist, song title and song's length in the music app history that was heard during  sessionId = 338, and itemInSession  = 4

2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182

3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

To gain more technical detail about the coding portion please view the ETL notebook.

### Files

1) Project_1B_\ Project_Template.ipynb--------# Performs ETL
2) event_data---------------------------------# CSV files to be pre-processed
3) event_datafile_new.csv---------------------# Streamlined file to create database from

