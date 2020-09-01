# Ironhack Project 2 - Sharks
Data Cleaning project using the Sharks Attack data from Kaggle for Ironhack Data Analytics bootcamp.

Before setting out the objective of the analysis, I have imported the common packages/modules used for data analysis and the data itself for quick exploration to inform what kind of infomration is within the file.

## Exploration
    Quick examination of the dataset to help planning the direction of this project.
  3 functions were created to present a quick snapshot on the data:
1. del_dup(df)
    - Passes the name of the dataframe as argument
    - Delete all the duplicated rows
    - Return the number of rows deleted
2. explore(df)
    - Passes the name of the dataframe as argument
    - Return information on number of rows and columns 
    - Return information on all the columns, each column's number of non-null values and data type
    - Return information on number of unique values in each column
3. further_look(df)
    - Passes the name of the dataframe as argument
    - Return a snapshot of the dataframe, displaying the top 5 rows of data

    ### Observation
    - 19411 duplicated rows are removed
    - There are 6312 rows and 24 columns in the data set.
    - There are columms with little non-null values, will need to delete columns that may not yield useful information
    "Unnamed: 22", "Unnamed: 23"
    - There are columns with around half missing values, usefulness for analysis to be determined
    "Age", "Time", "Species"
    - There were little numerical values in the data upon examining the data types of each columns, further cleansing required
        - "Age" expected to be a numerical field
        - "Date" expected to be a datetime field
    - The number of unique values for each column revealsd:
        - Data cleaning required for "Sex" and "Fatal (Y/N)" as one would expect no more than 3 different values used
        - Too much variations in "Location", "Name", "Injury", "Investigator or Source" - may not be useful in terms of analysing its data
     - "Case Number", "Case Number.1", "Case Number.2", "original order" may help providing data on missing values for "Year"
     
## Plan
The aim of the project will be examining the fatal shark attacks in the past 25 years.
In order to identify which data is relevant to the project, "Year", "Fatal (Y/N)" will be required.
This project will examine the country where the attack took place, the activity of which the victim was undertaking during the attack, different type of attacks and the species of shark involved.

## Tools
    Some functions have been created to assist with the data cleaning
1. des_cat_col(df, col)
    - Passes the name of the dataframe and the name of the column as argument
    - Returns the unique value of the column and its count
    - Returns the number of missing value
2. clean_initial(df, col)
In preparation for columns that typically rely on one letter as value e.g. M/F, Y/N
    - Passes the name of the dataframe and the name of the column as argument
    - Strip out of the numerical, special characters and spaces on either side
    - Update column with the first character of the value in uppercase
3. clean(df, col)
    - Passes the name of the dataframe and the name of the column as argument
    - Strip out of the special characters and spaces on the either side
    - An option to strip out numerical values on the either side
4. removenum(df, col)
    - Passes the name of the dataframe and the name of the column as argument
    - Remove numerical and punctuation marks from the value
5. replace_contain(df, col, contain, case, value)
    - Passes the name of the dataframe and the name of the column, condition, whether the search should be case sensitve, and the new value as argument
    - Filter on the column based on whether the value contains the specified characters and whether this is case sensitive
    - Update the value with the specified new value
6. clean_cols(df)
    - Passes the name of the dataframe as argument
    - Returns a list of column names and update it to lowercase and remove spaces for ease of coding input
       
## Irrelevant Data Removal
Some columns are removed due to the usefulness and irrelevance to current project.

1. Missing Rate
    drop_missingrate(df)
    - Passes the name of the dataframe as argument
    - Returns the number of rows and columns, each column and its missing value percentage
    - Takes in the missing value percentage threshold to determine those columns that exceed the threshold will be removed (confirmation required before)
    - Returns the updated number of rows and columns, each column and its missing value count
    60% was used as a threshold, "unnamed:_22", "unnamed:_23" were removed
 2. Empty Rows
    empty_row(df)
    - Passes the name of the dataframe as argument
    - Returns number of columns and ask to input the number of missing value threshold for each row
    - Summarise the number of rows that have missing value higher than the indicated threshold and remove them (confirmation required before)
    12 was used as a threshold (more than 66%), 10 rows are removed.
    
## Data Cleaning
### Incorrect Values
#### Sex
gender(df, col)
Sex is usually input as "M"/"Male"/"F"/"Female" therefore first letter is after stripping is used.
    - Passes the name of the dataframe and column as argument
    - Returns the value and frequency table and perform the stripping of irrelevant characters before updating the value with the first character in uppercase
    - If the value is not M/F, then updating to "Unknown"
    - Returns the updated value frequency table
Data entry mistakes often occurs with "sex" being store in the "name" column, a quick glance filtering on those with missing value in "sex" has any gender information within "name".n
7 missing values are updated using the information from "name"

#### Fatal
yes_no(df, col)
Column expected with Y/N values will be stripped of irrelevant characters and updated with its first letter in uppercase
    - Passes the name of the dataframe and column as argument
    - Returns the value and frequency table and perform the stripping of irrelevant characters before updating the value with the first character in uppercase
    - If the value is not Y/N, then updating to "Unknown"
    - Returns the update value frequency table
Examine the "injury" column to determin whether missing values can be obtained there.

### Missing Values
#### Year
Listing out all the unique values and the count of missing values to determine the extent of data cleaning required before filling in the missing values. In this case, only 2 missing present in the dataset.
Examine whether the "date", "case_number", "case_number.1", ""case_number.2" columns can fill in the gaps
Updated the missing values and update the data type of the column
Since the current project only concerns shark attacks in the past 25 years, those attacks occurred before 1995 are removed.

### Text Cleaning
#### Type
Examine the value frequency table and fill in the missing value as "Unknown"
Update the categories of type to: Unprovoked, Unknown, Provoked, Boating, Sea Disaster

#### Country
Examine the value frequency table, update values as uppercase for ease of text cleaning exercise and fill in the missing value as "UNKNOWN"
Apply functions removenum and clean to remove unwanted characters before proceeding onto text cleaning
88 distinct values are left.

#### Activity
Examine the value frequency table and fill in the missing value as "Unknown"
There are 113 missing values
Define the different types of activities for the analysis: "Swimming", "Wading", "Diving", "Fishing", "Unknown", "Others", "Surfing/Boarding",'Accident/Disaster', 'Boating/Sailing'

#### Species
Examine the value frequency table and fill in the missing value as "Missing"
There are 828 missing values
Apply functions removenum and fullclean before text cleaning
In the midst of text cleaning it turns out that there are instances where it was a hoax or did not involve a shark, those cases are updated as "Unconfirmed"
Any entries that confirms shark involvement but with unknown species is mapped as "Shark" as this can be used as a filter to select these cases as valid shark attack

### Columns Removal
Remove others columns not used

## Analysis
    Ensure only cases with confirmed shark involvements were selected.
 Display the shark attacks by year.
 The percentage of the shark attacks being fatal.
 Display the shark attacks by year that are fatal. (suddenly 2015 doesn't look as bad as 2000)
 The percentage of the fatal shark attacks across the gender
 Display the activities the victims were doing before the fatal shark attack (swimming in the sea doesn't seem so safe)
 Display the fatal shark attack by attack type (who is that stupid to provoke a shark?!)
 Display the fatal shark attack by species (white sharks are scary)
 Display the fatal shark attack by country
