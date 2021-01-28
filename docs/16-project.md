# Project analysis

We've spent the last 6 months giving you the skills you need to be able to deal with your own data. Now's the time to show us what you've learned. In this chapter we're going to describe the steps you will need to go through when analysing your data but, aside from a few lines that will help you deal with the questionnaire data that the Experimentum platform spits out, we're not going to give you any code solutions. 

Everything you need to do you've done before, so use this book to help you. Remember, you don't need to write the code from memory, you just need to find the relevant examples and then copy and paste and change what needs changing to make it work for you.

We suggest that you problems-solve the code as a group, however, make sure that you all have a separate copy of the final working code.

### Step 1: Load in packages and data

The data files are on Moodle.

Don't change ANY of the code from step 1 and 2. Just copy and paste it into R EXACTLY as it is below. 


```r
library(tidyverse)

demo <- read_csv("demographics.csv")
mslq <- read_csv("mslq.csv")
teams <- read_csv("team-name.csv")
```

### Step 2: Clean up the data

Run the below code - don't change anything. This code will clean up the Experimentum data a little bit to help you on your way. You will get a message saying `NAs introduced by coercion`. Ignore this message, it's a result of converting the employment hours to a numeric variable.


```r
demo_final <- demo %>% 
  group_by(user_id, q_id) %>% 
  filter(session_id == min(session_id), endtime == min(endtime)) %>% 
  filter(row_number() == 1) %>% 
  ungroup() %>% 
  filter(user_status %in% c("guest", "registered")) %>%
  select(user_id, user_sex, user_age, q_name, dv) %>%
  pivot_wider(names_from = q_name, values_from = dv)%>%
  mutate(employment = as.numeric(employment))

teams_final <- teams %>%
  group_by(user_id, q_id) %>% 
  filter(session_id == min(session_id), endtime == min(endtime)) %>% 
  filter(row_number() == 1) %>% 
  ungroup() %>% 
  filter(user_status %in% c("guest", "registered")) %>%
  select(user_id, user_sex, user_age, dv) %>%
  rename("team" = "dv")
  
mslq_final <- mslq %>% 
  group_by(user_id, q_id) %>% 
  filter(session_id == min(session_id), endtime == min(endtime)) %>% 
  filter(row_number() == 1) %>% 
  ungroup() %>% 
  filter(user_status %in% c("guest", "registered")) %>%
  select(user_id, user_sex, user_age, q_name, dv) %>%
  arrange(q_name) %>%
  pivot_wider(names_from = q_name, values_from = dv)
```

Right. Your turn. You may find it helpful to use the search function in this book to find examples of the code you need.

<div class="figure" style="text-align: center">
<img src="./images/searching.gif" alt="Searching for functions" width="75%" height="75%" />
<p class="caption">(\#fig:unnamed-chunk-3)Searching for functions</p>
</div>


### Step 3: Join 

Join together the data files by all their common columns. The resulting dataset is going to have 91 columns which means that R won't show you them all if you just click on the object, you'll need to run `summary()`. **Hint:** You can only join two objects at once, so you'll need to do multiple joins (in a pipeline if you're feeling snazzy).

### Step 4. Select your variables

Use select to retain only the variables you need for your chosen research design and analysis, i.e. the responses to the sub-scale you're interested in as well as the user id, sex, age, team name, and any variables you're going to use as criteria for inclusion. You might find it helpful to consult the MSLQ overview document to get the variable names.

### Step 5: Factors

Use `summary` or `str` to check what type of variable each column is. Recode any necessary variables as factors and then run summary again to see how many you have in each group. You will find the code book you downloaded with the data files from Moodle helpful for this task. You may find the Data Visualisation activity about factors helpful for this.

### Step 6: Filter

If necessary, use filter to retain only the observations you need, for example, you might need to delete participants above a certain age, or only use mature students etc. (and make sure you kept all these columns in step 4). Do not filter the data for your team yet. You will find the code book you downloaded with the data files from Moodle helpful for this task.

If your grouping variable is whether students undertake paid employment, you will need to create a new variable using mutate that categorises participants into employed (> 0 hours worked per week) and not employed (0 hours per week) categories.

An additional bit of syntax you might find useful for this is the `%in%` notation which allows you to filter by multiple values. For example, the following code will retain all rows where `user_sex` equals male OR female and nothing else (i.e., it would get rid of non-binary participants, prefer not to says, and missing values).


```r
dat %>%
  filter(user_sex %in% c("male", "female"))
```

You can also do it by exclusion with `!`. The below code would retain everything where `user_sex` doesn't equal male or female.


```r
dat %>%
  filter(!user_sex %in% c("male", "female"))
```

If you were feeling really fancy you could do steps 3 - 6 in a single pipeline of code.

### Step 7: Sub-scale scores

Calculate the mean score for each participant for your chosen sub-scale. There are a few ways you can do this but helpfully the [Experimentum documentation](https://gla-my.sharepoint.com/:w:/g/personal/2087153l_student_gla_ac_uk/EfFPtssPMV9HkrZALfdln8wBBJKClQ0eAXzrrHxa0nOo7g?e=SNIhSt) provides example code to make this easier, you just need to adapt it for the variables you need. You may also want to change the `na.rm = TRUE` for the calculation of means depending on whether you want to only include participants who completed all questions.



```r
dat_means <- data %>% # change data to the name of the data object you want to work from
  pivot_longer(names_to = "var", values_to = "val", question_1:question_5) %>% # change question_1:question_5 to the relevant variables for your sub-scale, don't change anything else 
  group_by_at(vars(-val, -var)) %>% # don't change this at all
  summarise(scale_mean = mean(val, na.rm = TRUE)) %>% # change scale_mean to the name of your sub-scale, e.g., anxiety_mean
  ungroup() # don't change this at all
```

### Step 8: Split the dataset

Next, use filter again to create a new dataset that only contains the data from participants who contributed to your team and call it `dat_means_team`. Once this is complete, you  will have the final large dataset that contains the scale scores for all participants, and a smaller dataset that just has data from the participants you recruited. Use the codebook to find which number corresponds to your team.

### Step 9: Demoraphic information 

That should be the really hard bit done, now you've got the data in the right format for analysis. 

First, calculate the demographic information you need: number of participants, gender split, grouping variable split (if you're using a variable that's not gender), mean age and SD. 

You can calculate mean age and SD using `summarise()` like you've done before. There's several different ways that you can count the number of participants in each group, we haven't explicitly shown you how to do this yet so we'll give you example code for this below. The code is fairly simple, you just need to plug in the variables you need.

Do this separately for the full dataset and your team dataset.


```r
# count the total number of participants in the dataset

dat_means %>%
  count()

# count the number of responses to each level of user_sex (for gender)
dat_means %>%
  group_by(user_sex) %>%
  count()

# count the number of responses to each level of mature student status (you may need to change this variable to the one you're using)
dat_means %>%
  group_by(mature) %>%
  count()

# count the number of responses across two categories (you might not need or want to do this)
dat_means %>%
  group_by(user_sex,mature) %>%
  count()
```

Once you've done this you might realise that you have participants in the dataset that shouldn't be there. For example, you might have people who have answered "Not applicable" to the mature student question, or you might have some NAs (missing data from when people didn't respond). 

You need to think about whether you need to get rid of any observationse from your dataset. For example, if you're looking at gender differences, then you can't have people who are missing gender information. You may have said in your pre-reg that you would only include non-binary people if they made up a certain proportion of the data. If you're looking at mature student status, you can't have people who didn't answer the question or who said not applicable (i.e., postgrad students). You need to decide whether any of this is a problem, and potentially go back and add in an extra filter to step 6.

### Step 10: Descriptive statistics

Use summarise and group_by to calculate the mean, median, and standard deviation of the sub-scale scores for each group. Do this separately for the full dataset and your team dataset.

### Step 11: visualisation

You now need to create a bar chart with error bars and a violin-boxplot for both the full dataset and your team dataset. You've done all of these before, just find a previous example code and change the variables and axis labels.

`













