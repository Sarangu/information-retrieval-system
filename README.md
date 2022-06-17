# CS6111 Advanced Database Systems - Information Retrieval System

## Team Members
Aishwarya Sarangu (als2389)

Rishabh Gupta (rg3334)

## Submitted Files
* **run:** script to run augmented search
* **requirements.txt:** all dependencies to run the code
* **transcript.txt:** sample runs for three queries

## Installing Dependencies

```
$ sudo apt-get update
$ sudo apt-get install -y python3-pip
$ pip3 install -r requirements.txt
$ python3 -m nltk.downloader stopwords
```

## Running the program

```
$ ./run AIzaSyC3gsw3bEoyTWHBk2akQ2uLP4IkeuyE_zg 31a218a86791fb76c <precision> "<search_query>"
```

## Project Design

The project has essentially one file called ```run``` which has all the logic of searching for a query using the google custom search api and augmenting it after the user submits the relevance of the documents returned by the api call. This continues until the user's desired precision is reached or the query no more results in any relevant documents.

The code primarily contains three procedures whose functionalities are as follows:

* **search_google:** This function takes the user query and hits the google custom search api to get top 10 results which are then presented to the user. The user then marks the results that are relevant to their query and if that gives us a precision lesser than the what the user desires, we call the code to augment this query and keep hitting the search api until we reach the desired precision.
* **sort_vocab:** This is the core of our entire implementation and returns the list of words in the vocabulary (which comprises of summary and title of the docs that were marked relevant by the user) in descending order based on the tf-idf of all the words.
* **get_new_query:** This function returns the new augmented query after finding the first 2 words (with highest tf-idf) that are not part of the last query and concatenating them with the last augmented query. The words in this new augmented query are ordered in descending order depending on their tf-idf values.

* Our code relies on a few external libraries which are ```googleapiclient, TfidfVectorizer from sklearn, numpy, stopwords from nltk.corpus```

## Query-Modification Method

* Our core logic is based on tf-idf values of each word in the documents that are marked relevant by the user. We consider the title and summary/snippet of each result returned by the search api as the corresponding document for that result.
* We compute the tf-idf of the all the words in the documents created in the previous step and return a sorted list of words in the vocabulary in decreasing order of their tf-idf.
* We find the two new words that we will augment our query with by selecting the top 2 words with the highest tf-idf that were not part of the last augmented query.
* Finally, the order of the words in the new query (old query + the two new words) are determined by which word has a higher tf-idf and that comes earlier in the query.

## Handling Non-HTML Files

We look for the field ```fileFormat``` in the results returned by the custom search api to figure out which results are non-html ones. We ignore the relevance information provided by the user for those results. 

## API Key & Engine ID

* **Google Custom Search Engine JSON API Key:** AIzaSyC3gsw3bEoyTWHBk2akQ2uLP4IkeuyE_zg
* **Engine ID:** 31a218a86791fb76c
