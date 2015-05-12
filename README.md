
# Quiz 2
## Question 1
-----------------------
Register an application with the Github API here https://github.com/settings/applications. Access the API to get information on your instructors repositories (hint: this is the url you want "https://api.github.com/users/jtleek/repos"). Use this data to find the time that the datasharing repo was created. What time was it created? This tutorial may be useful (https://github.com/hadley/httr/blob/master/demo/oauth2-github.r). You may also need to run the code in the base R package and not R studio.

## Answer
-------------
2013-11-07T13:25:07Z

## Explanation
----------------
First load httr package, if not install, type code: install.packages("httr")
```{r }
library(httr)
```

set oauth endpoint to github

```{r }
oauth_endpoints("github")
```

 connecet to github using your key and secret. replace with yours
 
```{r}
myapp<- oauth_app("github", key="a9d5df264ad77bd0059d",
secret="0b9c64ffa855c8c4cec8c6ad2352c3ccbdad6faf")
```

get token to access to API

```{r}
github_token<-oauth2.0_token(oauth_endpoints("github"), myapp, cache=FALSE)
gtoken<-config(token = github_token)
```


get information from the url you want information. 

```{r}
url<-GET("https://api.github.com/users/jtleek/repos", gtoken)
```


conver content to json dataform. 

```{r}
json1=content(url)
json2= jsonlite::fromJSON(toJSON(json1))
a=subset(json2, json2$name=="datasharing", )
a$created_at
```

## Question 2
------------------------------------
The sqldf package allows for execution of SQL commands on R data frames. We will use the sqldf package to practice the queries we might send with the dbSendQuery command in RMySQL. Download the American Community Survey data and load it into an R object called

`acs`

[https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv](https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv)

Which of the following commands will select only the data for the probability weights pwgtp1 with ages less than 50?
## Answer
----

```{r}
sqldf("select pwgtp1 from acs where AGEP < 50")
```

## Question 3
----
Using the same data frame you created in the previous problem, what is the equivalent function to unique(acs$AGEP)
## Answer
---


```{r}
sqldf("select distinct AGEP from acs")
```

## Question 4
---
How many characters are in the 10th, 20th, 30th and 100th lines of HTML from this page:

[http://biostat.jhsph.edu/~jleek/contact.html](http://biostat.jhsph.edu/~jleek/contact.html)

(Hint: the nchar() function in R may be helpful)

## Answer
---
45 31 7 25
## Explanation 
---


```{r}
url<-"http://biostat.jhsph.edu/~jleek/contact.html"
con<-url(url)
code<-readLines(con)
close(con)
sapply(htmlCode[c(10, 20, 30, 100)], nchar)
```

## Question 5
----
Read this data set into R and report the sum of the numbers in the fourth column.

[https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for]()

Original source of the data: 
[http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for]()

(Hint this is a fixed width file format)
## Answer
---
32426.7
## explanation
----


```{r}
url<- "http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for"
file<-download.file(url, destfile="./8110.for", method="curl")
data <- read.csv("./8110.for", header = TRUE)
file_name<-"./8110.for"
df <-read.fwf(file=file_name,widths=c(-1,9,-5,4,4,-5,4,4,-5,4,4,-5,4,4), skip=4)
sum(df[, 4])```

