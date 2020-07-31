# Data wrangling 2

## Activity 1: Set-up

* Download the <a href="files/stub-wrangling-2.zip" download>`Wrangling 2 Markdown`</a>, extract the file and then move it in to your Data Skills folder.
* Open R Studio and ensure the environment is clear.  
* Open the `stub-wrangling-2.Rmd` file and ensure that the working directory is set to your Data Skills folder and that the two .csv data files (`participant-info.csv` and `ahi-cesd.csv`) are in your working directory (you should see them in the file pane).   
* Type and run the below code to load the `tidyverse` package and to load in the data files. 


```r
library(tidyverse) 

dat <- read_csv('ahi-cesd.csv')
pinfo <- read_csv('participant-info.csv')
all_dat <- inner_join(dat, pinfo, by= c("id", "intervention"))
```




Now let's start working with our `tidyverse` verb functions...

## Activity 2: Select  

Select the columns all_dat, ahiTotal, cesdTotal, sex, age, educ, income, occasion, elapsed.days from the data and create a variable called ```summarydata```. 


```r
summarydata <- select(all_dat, ahiTotal, cesdTotal, sex, age, educ, income, occasion, elapsed.days)
```

******

**Pause here and interpret the above code and output**

* How you would translate this code into English 
* What columns have been removed from the data? 

******

## Activity 3: Arrange  

Arrange the data in the variable created above (```summarydata```) by ahiTotal with lowest score first. 

```r
ahi_asc <- arrange(summarydata, by = ahiTotal)
```

******

* How could you arrange this data in **descending** order (highest score first)?  


<div class='solution'><button>Solution</button>


```r
arrange(summarydata, by = desc(ahiTotal))
```

</div>


* What is the smallest ahiTotal score? <input class='solveme nospaces' size='2' data-answer='["32"]'/>

* What is the largest ahiTotal score? <input class='solveme nospaces' size='3' data-answer='["114"]'/>

******

## Activity 4: Filter  

Filter the data ```ahi_asc``` by taking out those who are over 65 years of age.  

```r
age_65max <- filter(ahi_asc, age < 65)
```

******

* What does `filter()` do? 

<select class='solveme' data-answer='["removes information that we are not interested in"]'> <option></option> <option>splits a column into multiple columns</option> <option>transforms existing columns</option> <option>takes multiple columns and collapses them together</option> <option>removes information that we are not interested in</option></select>

* How many observations are left in `age_65max` after running `filter()`? <input class='solveme nospaces' size='3' data-answer='["939"]'/>

******

## Activity 5: Summarise  

Then, use summarise to create a new variable ```data_median```, which calculates the median ahiTotal score in this grouped data and assign it a table head called ```median_score```.

```r
data_median <- summarise(age_65max, median_score = median(ahiTotal))
```

******

* What is the median score? <input class='solveme nospaces' size='2' data-answer='["74"]'/>

* Change the above code to give you the **mean score**. What is the mean score to 2 decimal places? <input class='solveme nospaces' size='5' data-answer='["72.48"]'/>


<div class='solution'><button>Solution</button>


```r
summarise(age_65max, mean_score = mean(ahiTotal))
```

</div>


******

## Activity 6: Group_by  

Use mutate to create a new column called `Happiness_Category` in `age_65max` which categorises participants based on whether they score above the median `ahiTotal` score for all participants. 

Then, group the data stored in  ```age_65max``` by `Happiness_Category`, and store it in an object named ```happy_dat```. 

Finally, use summarise to calculate the median `cesdTotal` score for participants who scored above and below the median `ahiTotal` score and save it in a new object named `data_median_group`.


```r
age_65max <- mutate(age_65max, Happiness_Category = (ahiTotal > 74))
happy_dat <- group_by(age_65max, Happiness_Category)

data_median_group <- summarise(happy_dat, median_score = median(cesdTotal))
```

******

**Pause here and interpret the above code and output**

* What does `group_by()` do? 

<select class='solveme' data-answer='["groups data frames based on a specific column so that all later operations are carried out on a group basis"]'> <option></option> <option>provides summary statistics of an existing dataframe</option> <option>organises information in ascending or descending order</option> <option>transforms existing columns</option> <option>groups data frames based on a specific column so that all later operations are carried out on a group basis</option></select>

* How would you change the code to group by education rather than `Happiness_Category`?


<div class='solution'><button>Solution</button>


```r
group_by(age_65max, educ)
```

</div>


******

## Activity 7: Data visualisation

Copy, paste and run the below code into a new code chunk to create a plot of depression scores grouped by income level using the `age_65max` data.


```r
ggplot(age_65max, aes(x = as.factor(income), y = cesdTotal, fill = as.factor(income))) +
  geom_violin(trim = FALSE, show.legend = FALSE, alpha = .6) +
  geom_boxplot(width = .2, show.legend = FALSE, alpha = .5) +
  scale_fill_viridis_d(option = "D") +
  scale_x_discrete(name = "Income Level", labels = c("Below Average", "Average", "Above Average")) +
  scale_y_continuous(name = "Depression Score")
```

* Which income group has the highest median depression scores? <select class='solveme' data-answer='["Below Average"]'> <option></option> <option>Below Average</option> <option>Average</option> <option>Above Average</option></select>

* Which group has the highest density of scores at any one point? <select class='solveme' data-answer='["Above Average"]'> <option></option> <option>Below Average</option> <option>Average</option> <option>Above Average</option></select>


<div class='solution'><button>Explain This Answer</button>

Density is represented by the curvy line around the boxplot that looks a little bit like a (drunk) violin. The fatter the violin, the more data points there are at any one point. This means that in the above plot, the Above Average group has the highest density because this has the widest violin, i.e., there are lots of people in the Above Average income group with a score of about 5.

</div>


* Is income group a between-subject or within-subject variable? <select class='solveme' data-answer='["Between-subjects"]'> <option></option> <option>Between-subjects</option> <option>Within-subjects</option></select>


<div class='solution'><button>Explain This Answer</button>

Between-subjects designs are where different participants are in different groups. Within-subject designs are when the same participants are in all groups. Income is an example of a between-subject variable because participants can only be in one grouping level of the independent variable

</div>


## Activity 8: Make R your own

Finally, you can customise how R Studio looks to make it feel more like your own personal version. Click `Tools` - `Global Options` - `Apperance`. You can change the default font, font size, and general appearance of R Studio, including using dark mode. Play around with the settings and see which one you prefer - you're going to spend a lot of time with R, it might as well look nice!


## Finished!

Well done! As a final step, try knitting the file to HTML. Remember to save your Markdown in your Data Skills folder and make a note of any mistakes you made and how you fixed them. 