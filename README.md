# How-to-access-NCBI-using-R
This tutorial will walk you through how to use R code to access NCBI and Pubmed
## Tutorial
### Step 1: Install and load the required packages
The following packages each have a specific function:
  ```rentrez``` is used to access the NCBI Entrez API for retrieving PubMed data.
  ```dplyr``` is used for data manipulation tasks.
  ```zml2``` is used to work with XML data.
```
install.packages("rentrez")
install.packages("dplyr")
install.packages("xml2")

library(rentrez)
library(dplyr)
library(xml2)
```

### Step 2: Retrive PubMed IDs (PMIDs)
To access PubMed articles, you typically start by retrieving a list of PubMed IDs (PMIDs) based on your search criteria. Here's an example of how to retrieve PMIDs using the ```entrez_search()``` function:
```
# Search for articles related to a specific query
search_query <- "your search query"

#Perform the search
search_results <- entrez_search(db = "pubmed", term = search_query, retmax = 10)

#Extract the IDs
pmids <- search_results$ids
print(pmids)
```
Replace "your search query" with your desired search terms, and retmax determines the maximum number of results to retrieve (in this case, 10). Print your results to view the PubMed IDs that were generated.

### Step 3: Fetch the MEDLINE records using IDs
```
medline_records <- entrez_fetch(db, id = id_list, rettype = "medline", retmode = "xml")
```

### Step 4: Parse the XML data using 'xml2::read_xml'
```
parsed_records <- xml2::read_xml(medline_records)
```
## Extract the desired information from the parsed records
```
article_info <- data.frame(
  Title = xml2::xml_text(xml2::xml_find_all(parsed_records, "//ArticleTitle")),
  Journal = xml2::xml_text(xml2::xml_find_all(parsed_records, "//ISOAbbreviation")),
  Year = xml2::xml_text(xml2::xml_find_all(parsed_records, "//PubDate/Year"))
  Abstract = xml2::xml_text(xml2::xml_find_all(parsed_records, "//AbstractText"))
)

#Display the information
print(article_info)
```
## Further Notes
XML data stands for eXtensible Markup Language much like HTML. It is designed to store and transport data.

In the code `xml2::read_xml`, `xml2::` is used to specify the `read_xml` function from the `xml2` package.

When multiple packages have functions with the same name, using `package_name::function_name` explicitly tells R which package's function to use. In this case, `read_xml` is a function from the `xml2` package, so we use `xml2::read_xml` to call that specific function.

By specifying `xml2::read_xml`, we ensure that we are using the `read_xml` function from the `xml2` package, even if there are other packages with functions named `read_xml`. This helps avoid any conflicts or ambiguity when multiple packages are loaded, as it explicitly tells R which package's function to use.

So, `xml2::read_xml` refers to the `read_xml` function from the `xml2` package, which is used to parse XML data in this code.
