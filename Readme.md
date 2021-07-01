# **Netflix IMDB Scores**

## **Project Summary**
This dataset consists of all Netflix original films released as of June 1st, 2021. Additionally, it also includes all Netflix documentaries and specials.IMDB scores are voted on by community members, and the majority of the films have 1,000+ reviews.

I used PowerBi to create a visualization with filter options. 

## **Key words**
* Power BI
* Dax
* Power Query
* M-language
* Dynamic Language selection
* Dynamic Conditional Formating

## **Top Row**

In the toprow the values of the dataset exploration are displayed. 
* The total number of titles in the whole dataset. 
* The overall average IMDB score is. 
* The Standard Deviation
* The minimum and maximum 
* The quartiles of the IMDB Scores. 

These values will always display the same, despite any filter placed. They will only change when new titles with new scores are added to the dataset. 
I used the Dax function "CALCULATE" for this in combination with "REMOVEFILTERS" function. 

``` Dax
IMDB_75th = CALCULATE(PERCENTILE.EXC(NetflixOriginals[IMDB],0.75),
            REMOVEFILTERS(NetflixOriginals))
```

## **Dynamic Language selection**
On the right side, 2 filters can be applied:
* Genre
* Original language.

If in the future more titles are added to the dataset and 1 language 'moves' from the other category to a top 9 position it will automically get it's own button the Treemap. Another less popular language will automatically be grouped with in the other category.
Here are the steps I took:
* I added a query referencing the original dataset. 
* Added a column Count language and ordered descending based on the count. 
* Added an index column starting at 1. 
* Then I added a conditional column: if the index was <10 it would have the category value [language] if not; it would have category value [other].

This way the most popular languages would have their own filter button in the Treemap, the rest is grouped under the [other] category button. 

## **Dynamic Conditional Formating**
Based on the filters applied, the values underneath the "Selection" header change. The Avg IMBD score in the selection area is colour coded based on the quartile range over the whole dataset. 
This way it's easily determined how the group of selected titles scores on the IMDB scores compared to all scores in the dataset. 

For this to work I created a Dax Measure called "Dynamic Color". 
```Dax
DAX:
Dynamic Color = if(AVERAGE(NetflixOriginals[IMDB])<[IMDB25th],"#faaba5",
                if(AVERAGE(NetflixOriginals[IMDB])<[IMDB_Median],"#faaba5",
                if(AVERAGE(NetflixOriginals[IMDB])<[IMDB_75th],"#4CF269",
                if(AVERAGE(NetflixOriginals[IMDB])<[IMDB_Max],"#36E053",
                "#06C31C"))))
```




## **Outcome**
![netflix](Images/Netflix.jpg)

Unfortunately I don't have a Microsoft Pro liscense, so i'm not able to embed the dashboard. I can however share the .pbix file. 