# RSS-Reader
#### RSS reader with sorting by time of post, repeats and works with list of urls. Will be do more, as real parser, GUI and more.

## PROGRAM OPERATION DESCRIPTION ##
#### The programme consists of 5 functions and uses 2 modules:
##### Functions:
1. <code> get_latest_articles() </code>
2. <code> get_old_articles() </code>
3. <code> isChange() </code>
4. <code> clean_new() </code>
5. <code> clean_old() </code>

#### Modules:
##### 1. FeedParser
##### 2. Schedule

#### Each function is called regularly using the schedule module. The construction and logic of the program avoids many errors in receiving and processing RSS streams, such as duplicate entries, loss of entries during processing, as well as the ability to reduce delays in receiving entries to a minimum.

##  DETAILED PROGRAMME DESCRIPTION ##

### 1. <code> get_latest_articles() </code> ###
#### This function is the backbone of the entire programme. It receives a header and a reference to the entries and returns a list <code> latest_titles </code> and a list <code> latest_links </code> that is further processed and sorted. As will be indicated in function 4, these lists are clean and updates every 10 minutes (the value can be changed in the programme schedule block) this is done to avoid possible entries repeating and memory overloads. ####

### 2. <code>get_old_articles()</code> ### 
#### This function has the same logic as <code> get_latest_articles() </code>, and for the most part is purely a sorting tool. Returns a list <code> old_titles</code> and a list <code> old_links </code>. These lists are then used only in the function <code> isChange() </code> and <code> clean_old() </code> function , where they are cleaned up every 60 minutes (the value can be changed in the programme schedule block). ####  
### IMPORTANT TIP: FOR CORRECT OPERATION, THE FREQUENCY OF CLEANING LISTS <code> old_titles </code>  AND  <code> old_links </code> SHOULD BE STRICTLY GREATER THAN THE FREQUENCY OF CLEANING AND UPDATING LISTS <code> latest_titles </code> AND <code> latest_links </code>, IT IS RECOMMENDED TO SET THE TIME TO AT LEAST 1 MINUTE GREATER THAN FOR LISTS <code> latest_titles </code> AND <code> latest_links </code>.

### 3. <code> isChange() </code> ###
#### The most important sorting function. It checks everything from discarding repetitions with <code> set </code>, to updating entries. The logic of the function is simple: lists <code> old_titles </code> and <code> old_links </code> and lists <code> latest_titles </code> and <code> latest_links </code> are re-created into sets, then the difference is found, the difference is the headers and links of the new entries. It returns 3 values: <code>title</code>, <code>link</code> and <code>text</code>, the value <code>text</code> is optional, e.g. it can be used for telegram bot replies. All values are cleans with a new run of the function, which should always be run at the same time as function <code> get_latest_articles() </code>, to avoid repetitions, delays and missing entries.

### 4. <code> clean_new() </code>
#### It's just clean <code> latest_titles </code> and <code> latest_links </code>. Should always be run at the same time as function <code> get_latest_articles</code>

### 5. <code> clean_old() </code>
#### It's just clean <code> old_titles </code> and <code> old_links </code>. Should always be run at the same time as function <code>get_old_articles()</code>. 

### 6. Schedule block.
#### The Schedule block is the block of the Schedule module which sets the schedule for the functions to be performed. ####
#### It has strict rules:
#### 1. The function must be executed with a delay of at least 1 minute from the execution time of the function
