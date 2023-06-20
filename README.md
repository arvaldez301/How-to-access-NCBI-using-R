# How-to-access-NCBI-using-R
This tutorial will walk you through how to use R code to access NCBI and Pubmed
## Tutorial
### Step 1: Install and load the required packages
```
install.packages("rentrez")
install.packages("dplyr")

library(rentrez)
library(dplyr)
```

### Step 2: Retrive PubMed IDs (PMIDs)
To access PubMed articles, you typically start by retrieving a list of PubMed IDs (PMIDs) based on your search criteria. Here's an example of how to retrieve PMIDs using the ```entrez_search()``` function:
```
# Search for articles related to a specific query
search_query <- "your search query"
search_results <- entrez_search(db = "pubmed", term = search_query, retmax = 10)
pmids <- search_results$ids
print(pmids)
```
Replace "your search query" with your desired search terms, and retmax determines the maximum number of results to retrieve (in this case, 10). Print your results to view the PubMed IDs that were generated
### Step 3: Fetch Article Summaries
Once you have the PMIDs, you can fetch the article summaries using the ```entrez_summary()``` function. Here's an example:
```
article_summaries <- entrez_summary(db = "pubmed", id = pmids)
```
This retrieves the summaries for the articles corresponding to the given PMIDs
### Step 4: Extract the desired information from the article summaries
```
article_info <- data.frame(
  Title = sapply(article_summaries, function(x) x$Title),
  Journal = sapply(article_summaries, function(x) x$Source),
  Year = sapply(article_summaries, function(x) substr(x$PubDate, start = 1, stop = 4))
)

print(article_info)
```
### Step 6: Fetch the sequences associated with the articles
```
# Fetch the sequences using the article IDs
sequences <- entrez_fetch(db, id = id_list, rettype = "fasta", retmode = "text")

print(sequences)
```
## Example
```
# Install and load the required packages
install.packages("rentrez")
install.packages("dplyr")

library(rentrez)
library(dplyr)

# Set the query term and database to search
query <- "BRCA1"
db <- "pubmed"

# Perform the search
search_results <- entrez_search(db, term = query)

# Extract the IDs from the search results
id_list <- search_results$ids

# Fetch the article summaries using the IDs
article_summaries <- entrez_summary(db, id = id_list)

# Extract the desired information from the article summaries
article_info <- data.frame(
  Title = sapply(article_summaries, function(x) x$Title),
  Journal = sapply(article_summaries, function(x) x$Source),
  Year = sapply(article_summaries, function(x) substr(x$PubDate, start = 1, stop = 4))
)

# Display the extracted information
print(article_info)

# Fetch the sequences using the article IDs
sequences <- entrez_fetch(db, id = id_list, rettype = "fasta", retmode = "text")

# Display the fetched sequences
cat("\nFetched sequences:\n")
cat(sequences)
```
