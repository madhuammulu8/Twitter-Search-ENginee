# Table Of Content
 * [Media](#Media)
 * [Abstract](#Abstract)
 * [Total Architecture](#Total-Architecture)
 * [Instruction For Running Twitter Crawler and Lucene](#Instruction-For-Running-Twitter-Crawler-and-Lucene)
 * [Twitter Crawling Architecture](#Twitter-Crawling-Architecture)
   * [Config.py](#Config.py)
   * [Twittercrawler.py](#Twittercrawler.py)
   * [Location and Emoji’s Handling Folder](#Location-and-Emoji’s-Handling-Folder)
 * [LUCENE Indexing Our Twitter Data](#LUCENE-Indexing-Our-Twitter-Data)
   * [TweetParser.Java](#TweetParser.Java)
   * [TweetIndexer.Java](#TweetIndexer.Java)


# Media: 

Youtube Link: https://www.youtube.com/watch?v=nEqtQ0FIVT8

Continue Of Full stack Search Enginee using the crawled data: https://github.com/madhuammulu8/Twitter-Search-Enginee-FullStack-Search-Enginee-Website

# Abstract:

The crawler, implemented in Python using Tweepy, retrieves tweets based on specified parameters, handles rate limits, and stores data in CSV files. Lucene indexing is performed using Java, with fields including coordinates, location, username, tweet text, and date/time. The indexing strategy utilizes various text analyzers and techniques. The architecture enables efficient crawling and indexing of Twitter data for further analysis and retrieval.

# Total Architecture:

<img width="498" alt="image" src="https://github.com/madhuammulu8/Twitter-search-Enginee-Crawling-and-Indexing/assets/65707202/672d2f45-f059-438a-aab1-a6afd556ac93">


# Instruction For Running Twitter Crawler and Lucene:

* Edit the crawler.bat file with the location of your local system and run the crawler.bat file, it will automatically get executed if Python has been installed.
* We have added another folder with files that handle Location and Emoji. This Python file handles the emojis, pulls tweets with geolocation in one file, and pulls non-geolocation-tagged tweets in another file.
   * Edit the crawler.bat file with the location of your local system and run the crawler1.bat then It will generate two CSV one with location and another without location to store the tweets and images will be downloaded under the images folder.
* Lucene: 
   * We first install all the files by importing the project in Eclipse and then running it as a maven clean and then a maven install.
   * Then we navigate to the project properties (right-click) and change the compiler to release version with default compiler settings. Then run as a Java application.
   * Set the path as needed in the indexer for the data.

# Twitter Crawling Architecture: 

## Config.py:
 Config files contain the API keys/consumer key, API/Consumer Secret, Access key, and Access key secret that is generated from the app we have created on Twitter. we created the app in the Twitter developer account is the first task we did in this project.

## Twittercrawler.py: 
The maximum number of requests that are allowed is based on the time interval, some specified period, or window of time. The request limit interval is fifteen minutes. If an endpoint has a rate limit of 450 requests per rate limit window (OAuth2) /  900 requests/ 15- minutes, then up to 900 requests over a 15-minute interval are allowed(OAuth1). 
As we are crawling over 1 GB of data we need to request more than 900 within 15 minutes. Hence we have added a wait-on rate limit. It helps us to retrieve data automatically. The crawler will go to sleep more on the limit of 900 requests per 15 minutes in reach. After 15 minutes our code will again automatically starts the crawling process the 

# Twitter Crawling Strategy

Handing Duplicate Tweets:-
Using the hash set we are retrieving the data Since the set does not allow duplicates it helps in reducing the extra efforts of removing duplicate data

## Data Storage:
We are writing our tweet data in a CSV writer and writing the text file in the CSV file using UTF-8 encoding as it contains the special characters

## Tweepy Cursor:
The cursor method is the latest release of Tweepy. It handles the pagination work for us behind the screen so our code can now just focus on processing the results. We have passed our parameters like our query, count, and the language into a cursor and it will automatically pass the parameters into a request whenever we make a request. Tweet_count in the items() is the total number of tweets users need to pull so it has to be passed as an argument while executing the python file, here we have passed in the argument inside the bat file.

## Location and Emoji’s Handling Folder:
As we are planning to pinpoint the location for Hadoop and show the pinpoint in the retrieving results we are filtering the data based on whether the place is found or not in the tweet. If we found the location then write the user location and followers count. Data is stored in data tweet_pulled_with_location.csv 
If we don’t find any location we write the user name, follower count …. Etc in the tweet_pulled_without_location.csv writer.

# LUCENE Indexing Our Twitter Data

## TweetParser.Java
The parser has one purpose and that is to set the structure of the tweet to be extracted and to parse and break the JSONObject into smaller parts and then pass it to the document function to be added to the index.

## TweetIndexer.Java
We are creating the indexer using the new indexer() command and then We are parsing the JSON object in order to analyze the string that is passed 

The fields used are the coordinates given as coords, The location is given as location. username given as user. The tweet data itself is given as text and the date and time are given as such. The indexing strategy is to index the text and geospatial data that exists within the tweet. To this end, we have used a few different types of text analyzers and indexing techniques.

Above is the index creation function called in the main, it uses the StandardAnanlyser for text and strings. It writes the indexes to the new index writer,
