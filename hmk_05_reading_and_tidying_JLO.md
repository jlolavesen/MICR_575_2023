HMK 5: Reading and tidying data
================

# Reading

- [R4DS Chapters 6-9](https://r4ds.hadley.nz/data-import)

# Data import

## Q1:

- Create a directory, within your main class directory, called `data`.
  - Note: in general, you should store raw data in a directory called
    `data`.
- Download the example file for Ch 9
  [here](https://pos.it/r4ds-students-csv). Save it inside the `data`
  directory.
- Use `read_csv()` to read the file to an R data frame. Follow the
  instructions in Ch 9 to format it properly. Follow the directions in
  Ch 9 to make sure the following are true:
  - Column names should be *syntactic*, meaning they don’t contain
    spaces.
  - N/A values should be represented with the R value `NA`, not the
    character “N/A”.
  - Data types (character vs factor vs numeric) should be appropriate.

``` r
library(tidyverse)

students <- read_csv("data/students.csv", na = c("N/A", ""))

students <- students |> 
  rename(
    student_id = `Student ID`,
    full_name = `Full Name`,
    age = AGE,
    favorite_food = favourite.food,
    meal_plan = mealPlan
    )|>
  mutate(
    meal_plan = factor(meal_plan),
    age = parse_number(if_else(age == "five", "5", age))
    )
```

## Q2

Find (or make) a data file of your own, in text format. Read it into a
well-formatted data frame.

``` r
erstwhile_sos <- read_csv("data/erstwhile_sos.csv")

erstwhile_sos <- erstwhile_sos |>
  mutate(sex = factor(sex))

glimpse(erstwhile_sos)
```

    Rows: 6
    Columns: 6
    $ name        <chr> "Beca", "Tessa", "Jackie", "Ashley", "Jimena", "Bre"
    $ sex         <fct> female, female, female, female, female, female
    $ current_age <dbl> 28, 30, 36, 33, 32, 25
    $ sane        <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE
    $ emo_avail   <lgl> FALSE, TRUE, FALSE, TRUE, FALSE, FALSE
    $ scrub       <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, TRUE

# Tidying

Download the data set available at
<https://tiny.utk.edu/MICR_575_hmk_05>, and save it to your `data`
folder. **This is a real data set:** it is the output from the
evaluation forms for student colloquium speakers in the Microbiology
department. I have eliminated a few columns, changed some of the scores,
and edited comments, to protect student privacy, but the output is real.

First, open this .csv file with Microsoft Excel or a text reading app,
to get a sense of the structure of the document. It is weird.

Why is the file formatted so inconveniently? I have no idea, but I do
know that this is about an average level of inconvenient formatting for
real data sets you will find in the wild.

## Q3a

Next, use `read_csv()` to read the data into a data frame. Note that
you’ll need to make use of some of the optional arguments. Use
`?read_csv` to see what they are.

As we discussed in class, the correct shape depends on what you want to
do with the data. Use `pivot_longer()` to make the data frame longer, in
a way that makes sense.

``` r
assessment <- read_csv("data/colloquium_assessment2.csv")|>
  filter(!is.na(StartDate))|>
  rename(duration = `Duration (in seconds)`)|>
  pivot_longer(
    cols = starts_with("Q"), 
    names_to = "question_number", 
    values_to = "qscore")|>
  group_by(question_number)
```

## Q3b

Finally, calculate this student’s average score for each of questions
7-10.

``` r
filter(assessment,
       question_number == "Q7" | question_number == "Q8"|
         question_number == "Q9" | question_number == "Q10")|>
  summarize(
    mean_qscore = mean(qscore)
    )
```

    # A tibble: 4 × 2
      question_number mean_qscore
      <chr>                 <dbl>
    1 Q10                    4.44
    2 Q7                     4.5 
    3 Q8                     4.62
    4 Q9                     4.31
