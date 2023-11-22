DataAnalytics_Rintro
================
Marco Alegria
2023-11-22

### What are we going to do?

To begin with this crash course of the R programming we will use the
open source data sets from the Google Data Analytics certificate,
specifically the data from bike trips from the company Divvy. In
conjunction with this R wrap up, we will use the R studio versatility
for data analysis and exploration to prepare a report and proposals for
new campaigns to get more members, as this is the main objective of the
Cyclistic company.

<center>
<img
src="C:\Users\Marco\Documents\DataAnalytics_MarketImprovement_Campaign\res\google-data-analytics-certificate.2.png"
style="width:20.0%" alt="Google Data Analytics" /> <img
src="C:\Users\Marco\Documents\DataAnalytics_MarketImprovement_Campaign\res\cyclistic.jpg"
style="width:20.0%" alt="Cyclistic Bikes" />
</center>

We will explore the basics of the R programming needed to start with
this language.

1.  **The basics of R programming**
2.  **Explore**
3.  **Clean**
4.  **Manipulate**
5.  **Describe**
6.  **Visualize**
7.  **Analyze**

### 1. The basics of R programming

In the Rstudio we have 4 quadrants that make up the whole environment,
this 4 quadrants are identified with red numbers as seen on the picture
below, beginning with the top left corner, and these are as follow:

1.  Scripts: It is where the code is written.
2.  Console: This will display the result of the code written on the
    first quadrant.
3.  Environment: This will show the data frames, objects, and variables
    created on the session of Rstudio.
4.  Plots and misc: Here will be displayed the plots resulting from the
    codes written and variables manipulation. Also here we can find the
    packages installed, the files from the root directory, and more.

<center>
<img
src="D:\09_Trainings\03_DataScienceCERT\capstones\Google\00_RstudioWrapup\res\ss_01.png"
style="width:50.0%" alt="RStudio explained" />
</center>

#### The four pillars of the R programming:

1.a. How to get help when needed? - We can find the help included on the
Rstudio by writing a ‘?’ on the second quadrant before each function,
this will display on the 4th quadrant the explanation of the help
needed.

1.b. Objects and functions.

- We can use the R code to produce the basics of operators in every
  programming language:

``` r
5+6
```

    ## [1] 11

``` r
14-6
```

    ## [1] 8

``` r
3/5
```

    ## [1] 0.6

``` r
5*9
```

    ## [1] 45

- To assign a value or function to a variable we use ‘\<-’ the symbol,
  after the function is executed a new variable will appear on the 3rd
  quadrant and the result will be displayed on the 2nd quadrant.

``` r
a <- 5
b <- 7
a+b
```

    ## [1] 12

``` r
sum(a,b)
```

    ## [1] 12

- Another useful function of R is the creation of data frames, for this
  we assign the function: ‘data.frame(x,y)’ with the symbol ‘\<-’ to
  create a data frame in our environment that has the data we assigned
  to it. When working with data frames we can explore a specific column
  writing the following function: ‘dataframe\$columnName’. By using the
  function ‘View()’ we display in another window on the 1st quarter the
  structured form of the data set. To get the information of the data
  set we write ‘str(dataframe)’ and this will display in the console the
  characteristics of the data frame.

``` r
ages <- c(15,20)
#We can use operators as well on a Vector with the values inside it.
sum(ages)
```

    ## [1] 35

``` r
names <- c("Lalo", "Laura")
#We create now a data frame
people <- data.frame(names, ages)
str(people)
```

    ## 'data.frame':    2 obs. of  2 variables:
    ##  $ names: chr  "Lalo" "Laura"
    ##  $ ages : num  15 20

``` r
people
```

    ##   names ages
    ## 1  Lalo   15
    ## 2 Laura   20

- To explore an specific data point in the data set we use the following
  structure: ‘dataframe\[x,y\]’.

``` r
people$names #We can also use the symbol $ if we know the name of a column
```

    ## [1] "Lalo"  "Laura"

``` r
people[,1]
```

    ## [1] "Lalo"  "Laura"

1.c. Built in data sets to practice.

- Rstudio has built-in data sets which we can explore and manipulate, to
  see the data sets included we use: ‘data()’, and to see an specific
  data set: ‘View(dataset)’.

1.d. Installing and using packages.

- There are different packages in the internet that can be installed and
  used outside the native Rstudio for this we use the following command:
  `install.packages("tidyverse")` `library(tidyverse)`

### 2. Exploring datasets

For this Wrap up we will use the open source data provided on the Google
Data Analytics certificate. The information found on one of the data
sets is trip data found on a company of Bikes. With the information
provided on this data sets we will explore the functionality of R
programming for Data analysis and result presentation.

As in every other programming language, the first thing to do is
declaring the libraries that will be used through all the code, for this
we define a field that includes all of the “library” declarations to
make the identification easier. In Rstudio we can declare the basic and
more used libraries like:

- tidyverse: the most used library in R, contains most of the
  environment utilities.
- dplyr: enables the most common data manipulation commands.
- mgrittr: enables the use of pipe operators.

After the importation of the libraries, in this case, we will create the
data frame that will be used through all of this wrap up. For this we
have to import the data that was provided in CSV format, in order to
create our usable data frame.

``` r
#We use the command 'message=FALSE' inside the code chunk declaration to stop the feedback output to show on the HTML file.
#If this is the first time opening R studio, it is recommended to install the following libraries, following the previous process to install packages, this can be run on the console on the second quarter of the environment.
#install.packages("tidyverse")
#install.packages("magrittr")
#install.packages("dplyr")
#install.packages("gapminder")

#Libraries importation field:
library(tidyverse)
library(magrittr)
library(dplyr)
library(readr)
library(maps)

#We get the working directory of the R file in order to make the importation of the data sets easier and faster by looping through the files
dirFolder <- getwd() 
#Depending on the setup of each analyst we can use the structure of the folders to put the data in only one single folder
dataFolder <- paste(dirFolder, "/dataSet/BikeData", sep = "") 
```

It is recommended that for the data importation, as a data analyst with
good practices, we give a correct structure and naming convention to our
data, with this in mind we want to save all of the tabular data in CSV
format, in the same folder with a correct name that identifies and has a
serial number. Once we make sure that our data to be imported (in this
case 12 ‘CSV’ files that contain tabular structured info) is in one
single folder, has a naming convention useful for the data importation,
and that can be accessed. We can make use of R control flow functions to
loop through the files and creating a data set in our environment that
contains all the columns of each CSV file.

``` r
#We know that we only have 12 files, and that the number of the data starts on 4, the looping control will depend on the format and number of files. The importation of the CSV files is possible thanks to the "readr" library previously imported.
for (i in 4:12){
#The loop is limited by the condition of the months that will have a different name up to the ninth month, after that we reduce a 0 on the name, this helps the for loop to run through each file and its data.
  if (i<=9){
    dfName <- paste(dataFolder, "/20200", i, "-divvy-tripdata.csv", sep = "")
    bikeTrips <- read.csv(dfName, header = TRUE, sep = ',')
  }
  else{
    dfName <- paste(dataFolder, "/2020", i, "-divvy-tripdata.csv", sep = "")
    bikeTrips <- read.csv(dfName, header = TRUE, sep = ',')
  }
}

#At the end we know that the data set contains 131573 observations
glimpse(bikeTrips)
```

    ## Rows: 131,573
    ## Columns: 13
    ## $ ride_id            <chr> "70B6A9A437D4C30D", "158A465D4E74C54A", "5262016E0F…
    ## $ rideable_type      <chr> "classic_bike", "electric_bike", "electric_bike", "…
    ## $ started_at         <chr> "2020-12-27 12:44:29", "2020-12-18 17:37:15", "2020…
    ## $ ended_at           <chr> "2020-12-27 12:55:06", "2020-12-18 17:44:19", "2020…
    ## $ start_station_name <chr> "Aberdeen St & Jackson Blvd", "", "", "", "", "", "…
    ## $ start_station_id   <chr> "13157", "", "", "", "", "", "", "", "", "", "", ""…
    ## $ end_station_name   <chr> "Desplaines St & Kinzie St", "", "", "", "", "", ""…
    ## $ end_station_id     <chr> "TA1306000003", "", "", "", "", "", "", "", "", "",…
    ## $ start_lat          <dbl> 41.87773, 41.93000, 41.91000, 41.92000, 41.80000, 4…
    ## $ start_lng          <dbl> -87.65479, -87.70000, -87.69000, -87.70000, -87.590…
    ## $ end_lat            <dbl> 41.88872, 41.91000, 41.93000, 41.91000, 41.80000, 4…
    ## $ end_lng            <dbl> -87.64445, -87.70000, -87.70000, -87.70000, -87.590…
    ## $ member_casual      <chr> "member", "member", "member", "member", "member", "…

Once we have built our data set we can start to explore it to learn and
get used to “data exploration” on R. As a data analyst the first step to
begin exploring a data set is to propose the correct questions. As
described in the beginning, we want to identify the best strategy to
persuade the casual users of the bicycles, to become a member. The first
question that we want to answer is:

- How do members and casual riders use bikes differently?

For now we will explore the possibilities that R gives us, we will go
more in depth on each section of the exploration, explaining the
plotting capabilities, the utilities that “pipe operators” brings us are
possible thanks to the previously imported library “magrittr”.

#### 2.a. Explaining the exploration capabilites

- To review the structure of any data set, we use the command
  `View(dataset)`
- To see a quick overview of the data set, we use `glimpse(dataset)`.
- To see the first rows of the data set, we use `head(dataset)`
- To find the type/class of the variables on a column of a data set, we
  use `class(dataset$column)`.
- To find the length of the data set or columns we use the command
  `length(dataset)`.
- To know the names of the variables in the dataset we use the command
  `names(dataset)`.
- To find the unique categories in the data set we use
  `unique(dataset$column)`.

Once we have the basics concepts of exploring data in the R studio
environment. We can start playing and interpreting data to get more used
to this new world. Let’s begin with understanding what does our data set
contains.

``` r
#What are the names of the columns on each observations?
names(bikeTrips)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
#We know that there are 13 variables of each observation, that shows that each user has specifically information to its ride.
```

``` r
#Does every column has correct data?
glimpse(bikeTrips)
```

    ## Rows: 131,573
    ## Columns: 13
    ## $ ride_id            <chr> "70B6A9A437D4C30D", "158A465D4E74C54A", "5262016E0F…
    ## $ rideable_type      <chr> "classic_bike", "electric_bike", "electric_bike", "…
    ## $ started_at         <chr> "2020-12-27 12:44:29", "2020-12-18 17:37:15", "2020…
    ## $ ended_at           <chr> "2020-12-27 12:55:06", "2020-12-18 17:44:19", "2020…
    ## $ start_station_name <chr> "Aberdeen St & Jackson Blvd", "", "", "", "", "", "…
    ## $ start_station_id   <chr> "13157", "", "", "", "", "", "", "", "", "", "", ""…
    ## $ end_station_name   <chr> "Desplaines St & Kinzie St", "", "", "", "", "", ""…
    ## $ end_station_id     <chr> "TA1306000003", "", "", "", "", "", "", "", "", "",…
    ## $ start_lat          <dbl> 41.87773, 41.93000, 41.91000, 41.92000, 41.80000, 4…
    ## $ start_lng          <dbl> -87.65479, -87.70000, -87.69000, -87.70000, -87.590…
    ## $ end_lat            <dbl> 41.88872, 41.91000, 41.93000, 41.91000, 41.80000, 4…
    ## $ end_lng            <dbl> -87.64445, -87.70000, -87.70000, -87.70000, -87.590…
    ## $ member_casual      <chr> "member", "member", "member", "member", "member", "…

``` r
#We can identify by the glimpse of the data set that some columns show empty values, which we might have to ignore when we work with the analysis.
```

``` r
#We want to verify that there are only 2 types of users, however, are there different types of bikes that we have to take into consideration for the analysis later on?
unique(bikeTrips$rideable_type)
```

    ## [1] "classic_bike"  "electric_bike" "docked_bike"

``` r
unique(bikeTrips$member_casual)
```

    ## [1] "member" "casual"

We have already identify that we can work with only 3 types of bicycles
that are used for only 2 types of users. Thanks to this we can start
thinking how to identify the target group to improve the subscription to
member type users. We might have to think on the Pareto principle to
find the 80% most important and representative demographic.

However, first we have to clean our data so we are sure that our study
is correctly made, we will continue with this cleansing as follows.

### 3. Data cleaning

We are aware that not all of the columns are meaningful data to find the
best demographic target for our new marketing strategy, however, for us
the most important data can be the dates that the bycicles are most
used, and the places were they are used as well.

We will continue with selecting only meaningful data for our followin
analysis, adapting and cleaning our data to make a more meaningful and
readable data set.

``` r
bikeTrips %>% 
  #In R we can select the variables in the order we want/need, without affecting the main data frame
  select(rideable_type, member_casual, started_at, ended_at, start_lat, start_lng, end_lat, end_lng) %>%
  #For better identification of the variables, we can assign any meaningful name to each column
  rename("Bike_type" = "rideable_type", "User_type" = "member_casual") %>% 
  #Using mutate to the columns we will change the presentation of the data, so we can identify easily patterns and correlations between the data set columns. Reducing the presentation of the dates, and the coordinates to only 2 decimal places.
  mutate(started_at = as.Date(started_at), ended_at = as.Date(ended_at)) %>% 
  mutate(start_lat = round(start_lat, 2), start_lng = round(start_lng, 2), 
         end_lat = round(end_lat, 2), end_lng = round(end_lng, 2)) %>% 
  #We only want to focus on the casual users.
  filter(User_type == 'casual') %>% 
  arrange(.,Bike_type) %>% 
  head()
```

    ##      Bike_type User_type started_at   ended_at start_lat start_lng end_lat
    ## 1 classic_bike    casual 2020-12-19 2020-12-19     41.90    -87.63   41.90
    ## 2 classic_bike    casual 2020-12-06 2020-12-06     41.92    -87.64   41.92
    ## 3 classic_bike    casual 2020-12-22 2020-12-22     41.90    -87.62   41.90
    ## 4 classic_bike    casual 2020-12-06 2020-12-06     41.95    -87.66   41.95
    ## 5 classic_bike    casual 2020-12-04 2020-12-04     41.90    -87.62   41.90
    ## 6 classic_bike    casual 2020-12-10 2020-12-10     41.90    -87.62   41.90
    ##   end_lng
    ## 1  -87.63
    ## 2  -87.64
    ## 3  -87.62
    ## 4  -87.66
    ## 5  -87.62
    ## 6  -87.62

``` r
#We will create a new data frame that has clean data with which we can work later on the analysis.
#This will give us more insight on a truthful analysis.
ride_length <- c(difftime(bikeTrips$ended_at ,bikeTrips$started_at))/3600
bikeTrips['ride_length'] <- ride_length

okTrips <- bikeTrips[!(bikeTrips$start_station_name == "" | bikeTrips$ride_length<0),]

#For now we want to visualize only our casual users and resume data:
```

Now we start to see a more clean data frame, that can gives us
interesting insights. For example, we maybe consider to find the most
latitudes and longitudes are present to maybe consider focusing the
marketing campaigns on this places. As well as the dates, we can find
the months or weeks when casual members use more frequently the
bicycles.

All of this information can guide us to more specific data that we have
to focus on in order to make the marketing campaings more meaningful.

### 4. Manipulate

Now that we have a clean data set, we can start to manipulate the data
and try to find patterns or relationships between the variables in order
to understand the meaningful information.

Remember our main objective is to find how to make the best marketing
campaign to make the casual users interested in becoming members.

Now that we have our clean data set, we will ask and focus on the
following points: - The distance that our users travel. - The most
popular places that the users take the bicycles. - The most popular
dates were the users take the bicycles.

``` r
#For the geographic analysis we will use the geosphere library, this helps us greatly for the analysis of distances. If you don't have this library you can install it using the command: 
#install.packages('geosphere')
library(geosphere)
```

    ## The legacy packages maptools, rgdal, and rgeos, underpinning the sp package,
    ## which was just loaded, will retire in October 2023.
    ## Please refer to R-spatial evolution reports for details, especially
    ## https://r-spatial.org/r/2023/05/15/evolution4.html.
    ## It may be desirable to make the sf package available;
    ## package maintainers should consider adding sf to Suggests:.
    ## The sp package is now running under evolution status 2
    ##      (status 2 uses the sf package in place of rgdal)

``` r
start_Coord <- data.frame(okTrips$start_lng, okTrips$start_lat)
end_Coord <- data.frame(okTrips$end_lng, okTrips$end_lat)

distanceRides <- distHaversine(start_Coord, end_Coord)
distanceRides <- data.frame(distanceRides)

maxDistance <- sapply(distanceRides, max, na.rm=TRUE)
minDistance <- sapply(distanceRides, min, na.rm=TRUE)
meanDistance <- sapply(distanceRides, mean, na.rm=TRUE)

DistanceAnalysis <- data.frame(maxDistance, meanDistance, minDistance)
DistanceAnalysis
```

    ##               maxDistance meanDistance minDistance
    ## distanceRides    29351.87     1992.315           0

Now we have the information for our first question, our user that
travels the most in our bikes is almost 3Km. However from all of the
users the mean distance traveled is around 2Km, this tells us that one
of the focus that we can take is giving incentives to our clients that
travel more than 1.5Km. Proposing discounts or another type of offer to
improve our Member base.

``` r
getmode <- function(val){
  uniqval <- unique(val, na.rm=TRUE)
  uniqval[which.max(tabulate(match(val, uniqval)))]
}

popLat <- getmode(okTrips$start_lat) 
popLng <- getmode(okTrips$start_lng)
popDate <- getmode(okTrips$started_at)

popPlace <- data.frame(popLng, popLat, popDate)
popPlace
```

    ##      popLng   popLat             popDate
    ## 1 -87.63128 41.90297 2020-12-18 15:55:33

Now we have one of the most important information, now we have the most
popular place where we can focus the marketing campaign, we have now the
Longitude and Latitude of the place where must of the people take and
use our bikes. With a quick Google using this information, we know that
the place is the zone nearby the Washington Square Park in Chicago,
Illinois. Around this park we can start using new marketing campaigns to
attract new members.

<center>
<img
src="D:\09_Trainings\03_DataScienceCERT\capstones\Google\00_RstudioWrapup\res\maps.png"
style="width:75.0%" alt="RStudio explained" />
</center>

We can have more insights on the data recollected by our users, we may
found more information that we can use to learn more about our users and
how the bicycles are used. For example, what percentage of our users are
the casual riders, how would this improve our finances if the majority
of the casuals became members?

``` r
totalRiders <- unique(count(okTrips['ride_id']))
casualRiders <- nrow(okTrips[okTrips$member_casual == 'casual',])
casual_Classic <- nrow(okTrips[okTrips$member_casual == 'casual' & okTrips$rideable_type == 'classic_bike',])
casual_Docked <- nrow(okTrips[okTrips$member_casual == 'casual' & okTrips$rideable_type == 'docked_bike',])
casual_Electric <- nrow(okTrips[okTrips$member_casual == 'casual' & okTrips$rideable_type == 'electric_bike',])

riderStats <- data.frame(pctCasualClassic = (casual_Classic/casualRiders)*100,
                           pctCasualDocked = (casual_Docked/casualRiders)*100,
                           pctCasualElectric = (casual_Electric/casualRiders)*100,
                           pctCasualUser = (casualRiders/totalRiders)*100)

print(riderStats)
```

    ##   pctCasualClassic pctCasualDocked pctCasualElectric        n
    ## 1         42.79557        18.67367          38.53076 22.13639

From the 100% of the users, the casual members represent the 22% of the
monthly users. From this 22% of users the majority of them use our
Classic style bicycle, this represent the 42% of our casual users, in
this analysis this might be the most important focus for the marketing
campaing. Giving more incentive to classic bycicle users.

``` r
#We tell R that we are going to work with our imported data set and we will use pipe operators for the functions.
okTrips %>% 
  #We will only see the observations inside the parameters written
  count(rideable_type, member_casual) %>% 
  arrange(member_casual) %>% 
  ggplot(aes(fill=member_casual, x=rideable_type, y=n))+
  geom_bar(position = "dodge", stat = "identity")+
  geom_text(aes(label=n), position = position_dodge(width = 0.9), size=3.5)+
  ggtitle("Number of used bikes by members and casual users")+
  labs(x="Type of bike", y="Number of used bikes")
```

![](DataAnalytics_Rintro_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

Catching our casual users from the classic and electric bicycles, can
improve greatly our finances. As we can see on the graph, this two type
of users represent the majority of our casual users. If the marketing
campaing can only focus on one type at a time, we might focus on the
classic users.

``` r
okTrips %>% 
  select(ride_length, member_casual, rideable_type) %>% 
  ggplot(aes(x=rideable_type, y=ride_length))+geom_point(aes(colour = member_casual))
```

    ## Don't know how to automatically pick scale for object of type <difftime>.
    ## Defaulting to continuous.

![](DataAnalytics_Rintro_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

Another interesting insight on the information that we can find in the
analysis, is the distance traveled by our users, although the majority
of the casual users take the classic type bikes, our docked bikes are
the ones that travel much more distance compared to other types of
bikes. This can give us an idea of where we can start new marketing
campaigns on the docking stations to get more people interested in using
the Cyclistic bike share program.
