# nyc_restaurant_inspection
EDA on NYC restaurant inspection scores from NYC Open Data

The data can be found here: https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/xx67-kt59

Code files:
* data_exploration.ipynb: the Jupyter notebook holding the code and results for Step 2, 3, and 4.
* data_exploration.html: exported version of data_exploration.ipynb after execution
*	keyword_extraction.ipynb: the Jupyter notebook holding the code and results for Step 5
*	keyword_extraction.html: exported version of keyword_extraction.ipynb after execution

Dependencies and run instructions:
*	Both notebooks were run with python 3.5, with the following required packages:
  * matplotlib
  * scipy
  *	numpy
  *	pandas
  * seaborn
  * tqdm
  * scikit-learn
  * statsmodel
  *jupyter
  * xgboost
*	How to run:
In command line, type ‘jupyter notebook’, open prompted url to go to jupyter notebook in a browser window. Then navigate to the ipynb files, click to run.

Notes on data preprocessing:

The downloaded data set (as of 9/29/2017) has 396,617 rows with 18 columns. A column dictionary and data set description document are available online at NYC Open Data, which provide explanations on a number of topics. (https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/xx67-kt59/about) 

Here are the key steps I took to clean and modify the data set:
1. Check the emptiness, unique values, data type for each column.
2. Because later on I’ll need ‘SCORE’ column as a response variable, I dropped rows with ‘SCORE’ column empty (~5% of the rows).
3. A number of rows (~100) have negative scores, which might be due to errors. Dropped them.
4. Removing rows with inspection date as 1/1/1900. According to documentation, these entries are new establishments that have not full finished inspection.
5. ‘GRADE’ column missing values are filled with string ‘Missing’
6. The original table has one violation per entry. Each inspection visit may result in multiple violations, each have its own row. Thus multiple violations sometimes share one inspection score due to a common inspection visit. For the purpose of studying inspection scores, it makes more sense to create a table with one inspection per row. So, I created a new table by aggregating violations of the same inspection into one row. A number of new columns are added
  * ‘violation_description’ holds the concatenated ‘VIOLATION DESCRIPTION’ strings
  *	‘violation_count’ holds the number of violations of an inspection
  * ‘critical_count’ holds the number of critical violations of an inspection
  *	‘inspection_year’, ‘inspection_month’, ‘inspection_day’, ‘inspection_weekday’ are the year, month, day of month and day of week generated from ‘INSPECTION DATE’
  * Additionally, for reasons that will become clear later, I added 77 columns for each of the violation code to hold one-hot encoding of incurred violation codes for each inspection visit.
  *	Removed columns that are no longer relevant: 'VIOLATION CODE', 'VIOLATION DESCRIPTION', 'CRITICAL FLAG', 'ACTION','RECORD DATE'
  * The new table, i.e table of inspections (instead of violations), has 133777 entries and 97 columns. I will refer to the original table as Violations Table and this new table as the Inspections Table and they are mysteriously named “df2” and “new_df” respectively in the code.


