# How-to-access-NCBI-using-R
This tutorial will walk you through how to use R code to access NCBI and Pubmed
## Tutorial
### Step 1: Install and load the required packages
```
install.packages("rentrez")
library(rentrez)
```
You may need other packages like ```XML``` or ```httr``` for processing the retrieved data. Install them if necessary using ```install.packages()```
### Step 2: Retrive PubMed IDs (PMIDs)
To access PubMed articles, you typically start by retrieving a list of PubMed IDs (PMIDs) based on your search criteria. Here's an example of how to retrieve PMIDs using the ```entrez_search()``` function:
```
# Search for articles related to a specific query
search_query <- "your search query"
search_results <- entrez_search(db = "pubmed", term = search_query, retmax = 10)
pmids <- search_results$ids
```
Replace "your search query" with your desired search terms, and retmax determines the maximum number of results to retrieve (in this case, 10).
### Step 3: Fetch Article Summaries
Once you have the PMIDs, you can fetch the article summaries using the ```entrez_summary()``` function. Here's an example:
```
article_summaries <- entrez_summary(db = "pubmed", id = pmids)
```
This retrieves the summaries for the articles corresponding to the given PMIDs
### Step 4: Access full article data
If you want to access the full article data, you can use the ```entrez_fetch()``` function. Here's an example:
```
# Fetch full article data for the first PMID
article_data <- entrez_fetch(db = "pubmed", id = pmids[1], rettype = "xml")
```
The ```rettype``` parameter specifies the format of the retrieved data (e.g., XML, text).
### Step 5: Process the retrieved data
Once you have retrieved the data, you can process it as per your requirements. You may need to use XML parsing techniques or other methods depending on the data format you retrieved.
## Example
```
# Step 1: Install required packages
install.packages("rentrez")
install.packages("httr")  # Additional package for processing the retrieved data

# Step 2: Load required packages
library(rentrez)
library(httr)

# Step 3: Retrieve PubMed IDs (PMIDs)
search_query <- "cancer gene"  # Replace with your desired search terms
search_results <- entrez_search(db = "pubmed", term = search_query, retmax = 10)
pmids <- search_results$ids

# Step 4: Fetch Article Summaries
article_summaries <- entrez_summary(db = "pubmed", id = pmids)

# Step 5: Access Full Article Data
article_data <- entrez_fetch(db = "pubmed", id = pmids[1], rettype = "xml")

# Step 6: Process the Retrieved Data
# For example, let's print the titles of the retrieved articles
for (summary in article_summaries) {
  print(summary$title)
}
```
