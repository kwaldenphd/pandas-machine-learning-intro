# Getting Started With Pandas (for machine learning)

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab covers the core components of `pandas`, with a focus on elements of `pandas` used in machine learning. It covers loading a structured data file (CSV and JSON) as a `DataFrame`, and sorting, selecting, and filtering the resulting `DataFrame`. The lab also covers common data parsing and wrangling challenges like duplicate entries and missing data. It covers the basic of data wrangling and manipulation in Python using `pandas`, as well as the fundamentals of creating plots in Python using `matplotlib`.

[Link to lab overview video](https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=cac767ea-fca5-472b-97ed-acd1012e9630) (Panopto, ND users)

## Acknowledgements

Information and exercises in this lab are adapted from the following resources:
- `pandas` documentation
  * ["Getting started"](https://pandas.pydata.org/pandas-docs/stable/getting_started/intro_tutorials/)
  * [User Guide, "Visualization"](https://pandas.pydata.org/docs/user_guide/visualization.html)
  * [Getting Started, "Plotting"](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html)
- Wes McKinney's [*Python for Data Analysis: Data Wrangling With pandas, Numpy, and IPython*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017)
  * Chapter 5 "Getting Started with pandas" (125-168)
  * Chapter 7 "Data Cleaning and Preparation" (195-224)
  * Chapter 8 "Data Wrangling: Join, Combine, and Reshape" (225-256)
  * Chapter 9 "Plotting and Visualization" (257-292)
  * Chapter 10 "Data Aggregation and Group Operations" (293-322)
- `matplotlib`, ["Tutorials"](https://matplotlib.org/tutorials/index.html)

All figures shown in this lab are from the `pandas` "Getting Started" tutorials or the `matplotlib` documentation and tutorials.

# Table of Contents

- [Lab notebook template](#lab-notebook-template)
- [What do we mean by `pandas`](#what-do-we-mean-by-pandas)
- [Data structures in `pandas`](#data-structures-in-pandas)
  * [`DataFrame`](#dataframe)
- [From structured data file to `DataFrame`](#from-structured-file-to-dataframe)
- [Interacting with a `DataFrame`](#interacting-with-a-dataframe)
  * [Sorting](#sorting)
  * [Subsetting](#subsetting)
    * [Select](#select)
    * [Filter](#filter)
    * [Selecting specific rows and columns](#selecting-specific-rows-and-columns)
  * [Removing duplicates](#removing-duplicates)
- [Summary Statistics and Calculations](#summary-statistics-and-calculations)
  * [Creating New Columns Based on Existing Columns](#creating-new-columns-based-on-existing-columns)
- [Renaming, Mapping, and Reindexing](#renaming-mapping-and-reindexing)
  * [Renaming Columns](#renaming-columns)
- [Getting started with `matplotlib`](#getting-started-with-matplotlib)
- [Anatomy of a `matplotlib` figure](#anatomy-of-a-matplotlib-figure)
  * [`Figure`](#figure)
  * [`Axes`](#axes)
  * [`Axis`](#axis)
  * [Everything Else (`Artists`)](#everything-else-artists)
- [`pandas` and `matplotlib`](#pandas-and-matplotlib)
  * [Plotting in `pandas`Uusing `.plot()`](#plotting-in-pandas-using-plot)
- [Lab Notebook Questions](#lab-notebook-questions)

[Link to this lab procedure as a Jupyter Notebook](https://colab.research.google.com/drive/1e3hLK2UNN6e_6fAeea2nhPvsXoy6-9kC?usp=sharing)

# Lab notebook template

[Click here](https://colab.research.google.com/drive/1HPD6LuBSZjbyAZVwRMD-lBqwB2H3wwxM?usp=sharing) to access the lab notebook template as a Jupyter Notebook.

# What do we mean by pandas

1. Some of you may be wondering why we are talking about pandas in a computer science course.

2. What is a panda?

<p align="center"><a href="https://github.com/kwaldenphd/pandas-intro/blob/main/Figure_2.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/pandas-intro/blob/main/Figure_2.png?raw=true" /></a></p>

3. Panda: "a large black-and-white mammal (Ailuropoda melanoleuca) of chiefly central China that feeds primarily on bamboo shoots and is now usually classified with the bears (family Ursidae)" ([Merriam-Webster](https://www.merriam-webster.com/dictionary/panda))

4. Wait that's not right....

5. `pandas`: "a fast, powerful, flexible and easy to use open source data analysis and manipulation tool, built on top of the Python programming language" ([`pandas` documentation](https://pandas.pydata.org/))

6. That makes more sense.

7. If you remember back to our earlier work with `.csv` files in Python, there are limitations to the kinds of things we can do with structured data using the `csv` module.

8. Particularly if we want to analyze and visualze structured data in a Python programming environment, we aren't going to get very far loading `.csv` files as lists or dictionaries.

9. We need Python to understand or interact with structured data as structured data.

10. Enter `pandas`.

11. Software developers at AQR Capital Management began working on a Python-based tool (written in a combination of C and Python) for quantitative data analysis in 2008.

12. The initial open-source version of `pandas` was released in 2008.

13. At its core, "`pandas` is a software library written for the Python programming language for data manipulation and analysis. In particular, it offers data structures and operations for manipulating numerical tables and time series" ([Wikipedia](https://en.wikipedia.org/wiki/Pandas_(software)).

14. The name `pandas` is derived from "panel data," an econometrics term used to describe particular types of datasets.

15. The name `pandas` is also a play on "Python data analysis."

16. For more on the history and origins of `pandas`, check out Wes McKinney's ["pandas: a Foundational Python Library for Data Analysis and Statistics"](https://www.dlr.de/sc/Portaldata/15/Resources/dokumente/pyhpc2011/submissions/pyhpc2011_submission_9.pdf) 2011 paper.

17. `pandas` is based on and has some similarities with another Python package, `NumPy`.

18. According to [package documentation](https://numpy.org/doc/stable/user/whatisnumpy.html), "`NumPy` is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more."

19. In `NumPy`, data are stored as list-like objects called arrays. 

20. `NumPy` allows users to access, split, reshape, join, etc. data stored in arrays.

21. `pandas` takes a similar approach to structured data, or data organized in a tabular (i.e. table-like) format.

22. "pandas adopts significant parts of NumPy's idiomatic style of array-based computing, especially array-based functions and a preference for data processing without for loops...the biggest difference is that pandas is designed for working with tabular or heterogeneous data. NumPy, by contrast, is best suited for working with homogenous numerical array data" (Wes McKinney, Chapter 5 "Getting Started with Pandas" in *Python for Data Analysis*, pg. 125)

23. For more on `NumPy`:
- [NumPy website](https://numpy.org/)
- [NumPy documentation](https://numpy.org/doc/stable/)
- ["NumPy Introduction," W3Schools](https://www.w3schools.com/python/numpy_intro.asp)
- ["Introduction to NumPy Tutorial," Software Carpentry](https://software-carpentry.org/blog/2012/06/introduction-to-numpy-tutorial.html)

## `DataFrame`

24. While a `Series` object is a one-dimensional array, a `DataFrame` includes a tabular data structure "and contains an ordered collection of columns, each of which can be a different value type" (McKinney, 130).

25. A `pandas` `DataFrame` has a row and column index--we can think of these as Series that all share the same index.

26. Behold, a two-dimensional data structure. 

27. In most situations, you'll create a `DataFrame` by reading in a structured data file.

28. But we're going to manually create a `DataFrame` to better understand how they work in `pandas`.

29. Let's go back to our state population data example.

30. Say we have two dictionaries that include equal-length lists:
```Python
# import pandas
import pandas as pd

# create dictionary with three equal-length lists
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'], 
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}

# write data dictionary values to data frame
frame = pd.DataFrame(data)

# show newly-created frame
frame
```

31. Behold, a two-dimensional `DataFrame` object with rows, columns, labels, and values.

32. The first object from each of the lists in our `data` dictionary now populate the first row of our `frame` DataFrame.

33. This pattern continues for subsequent rows.

34. We can start to explore the dimensions or overall characteristics of our DataFrame:
```Python
# shows index labels and values for first 5 rows
frame.head(5)

# shows list of column labels
frame.columns.values

# shows basic statistical information for the data frame
frame.describe()
```

36. The last command `.describe()` returns some statistical information about values in our dataset, including:
- count
- mean
- standard deviation
- minimum
- 25th percentile
- 50th percentile
- 75th percentile
- maximum

37. Let's say we want to specify the column sequence or the order in which columns are arranged in the `DataFrame`:
```Python
pd.DataFrame(data, columns=['year', 'state', 'pop'])
```

38. What happens if we specify a column that isn't represented in the data passed to the `DataFrame`? 
```Python
# create new dataframe from data dictionary with specific columns and index labels
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'], index=['one', 'two', 'three', 'four', 'five', 'six'])

# show frame2 DataFrame
frame2
```

39. We see only `NaN` missing values for the `debt` column since that column is not represented in the `data` dictionary used to create our `frame2` DataFrame.

40. We can select columns in our `DataFrame` using their index labels.
```Python
# returns state column
frame2['state']
```

41. We can also select a column using the `name` attribute.
```Python
# returns year column
frame2.year
```

42. We can retrieve rows based on their position using the `loc` (location) attribute.
```Python
# returns third row in the dataframe
frame2.loc['three']
```

```Python
# uses numerical index values to retrieve first four rows
frame2.iloc[0:3, :]
```

43. Let's say we wanted to manually assign the values in a particular column, for example the `debt` missing data column.

```Python
# returns frame2 DataFrame with newly-assigned 16.5 value for all rows in debt column
frame2['debt'] = 16.5

# show dataframe
frame2
```

44. We could also manually insert values for specific rows using their index labels.

45. If we wanted to add debt values for rows two, four, and five:
```Python
# variable with Series object
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])

# assign val Series object to debt column in frame2 DataFrame
frame2['debt'] = val

# show modified DataFrame
frame2
```

46. We now see the modified `debt` values for the rows specified in the `index=` portion of our code.

47. We can remove a column using the `del` keyword.
```Python
# delete column
del frame2['debt']

# returns updated list of columns
frame2.columns

# result will be 'year', 'state', and 'pop'
```

48. We can also pass a nested dictionary to a DataFrame.

```Python
# assign nested dictionary to variable
pop = {'Nevada': {2001: 2.4, 2002: 2.9}, 
       'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
       
# passing the nested dictionary to a DataFrame
frame3 = pd.DataFrame(pop)

# return new DataFrame
frame3
```

49. `pandas` interprets the outer dict keys as column names and inner keys as row names.

<blockquote>Q1: Describe a DataFrame in your own words.</blockquote>

<blockquote>Q2: Create your own small DataFrame. Write code that accomplishes the following tasks. Include code + comments.
 <ul>
  <li>Change the original column order</li>
  <li>Select a specific column(s) using its index label or name attribute</li>
  <li>Select a specific row(s) using its index label or index value</li>
  <li>Remove a column from the DataFrame</li>
  <li>Determine summary statistics for values in the DataFrame</li>
 </ul>
 </blockquote>

# From structured data file to `DataFrame`

50. As mentioned earlier in this lab, it's far more likely that you will load structured data from a file into Python, rather than manually creating a `DataFrame`.

51. For this section of the lab, we're going to work with data about *Titanic* passengers.

52. Navigate to https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic.csv in a web browser to see the dataset.

53. We can load structured data into Python from a file located on our computer or from a URL, using `pd.read_csv()`.
```Python
# import pandas
import pandas as pd

# load titanic data from csv file
titanic_file = pd.read_csv("titanic.csv")

# show first 5 rows of newly-loaded dataframe
titanic_file.head(5)

# load titanic data from url
titanic = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic.csv")

# show first 5 rows of newly-loaded dataframe
titanic.head(5)
```

54. `pandas` provides the `read_csv()` function which stores `.csv` data as a `pandas` `DataFrame`.

55. This `read_` prefix can be used with other structured data file formats, as we'll explore with JSON later.

56. Other parsing functions in `pandas`:
- `read_fwf`: fixed-width data with no delimiter
- `read_clipboard`: reads in data from clipboard
- `read_excel`: reads in data from `.xls` or `.xlsx` files
- `read_html`: reads in any tables contained in an HTML document
- `read_json`: reads in JSON data
- `read_sql`: reads in results of an SQL query as a pandas dataframe
- `read_sas`: reads in SAS dataset
- `read_stata`: reads in Stata file format

57. To check the first and last five rows of the `titanic` data frame:
```Python
titanic
```

58. We can also check the data type for each column using `.dtypes`.
```Python
titanic.dtypes
```

59. From this output, we know we have integers (`int64`), floats (`float64`), and strings (`object`).

60. Maybe we want a more technical summary of this `DataFrame`.
```Python
titanic.info()
```

61. `.info()` returns row numbers, the number of entries, column names, column data types, and the number of non-null values in each column. 

62. We can see from the `Non-Null Count` values that some columns do have null or missing values.

63. `.info()` also tells us how much memory (RAM) is used to store this `DataFrame`.

64. In the titanic data example, the `read` operation was fairly straightforward. 

65. The data being loaded contained headers and was formatted in a way we would expect for a `.csv` file.

66. But in many situations, data being loaded to a `DataFrame` will not conform to these formatting conventions.

67. Let's say we have a file that does not include a header row.

68. `pandas` can assign default column names, or you can set them manually.

69. The `titanic_no_header` file contains the same original data with the first row of column names removed.

```Python
# import pandas 
import pandas as pd

# load headless titanic data
headless_titanic_default = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic_no_header.csv", header=None)

# shows us the default column names assigned by pandas
headless_titanic_default

# load headless titanic data and manually assign column names
headless_titanic = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic_no_header.csv", names=['PassengerID', 'Survived', 'Pclass', 'Name', 'Sex', 'Age', 'SibSp', 'Parch', 'Ticket', 'Fare' ,'Cabin', 'Embarked'])

# shows data frame with manually assigned column names
headless_titanic
```

70. Let's say you have a `.txt` file that does not incldue a character delimiter.

71. `titanic.txt` is the same titanic data this time as tab-delimited values.

72. We would need to specify how `pandas` should parse this data as rows and columns.

```Python
# import pandas
import pandas as pd

# load titanic txt data
titanic_txt = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic.txt", sep="\t")

# shows first and last five rows of data frame
titanic_txt
```

73. We can also run into situations where the data we are loading into Python has missing values.

74. Just trying to load a file with missing data will return error messages.

75. We can specify what characters representing missing data when we create the `DataFrame`.

```Python
# import pandas
import pandas as pd

# load data where missing values are represented by NA or Null
sample = pd.read_csv("file_name.csv", na_values=['NA', 'Null'])

# for a situation where missing values in one column are represented by NA and another column's missing data are represented by Null
# first step is to create a dictionary for these column names and null values
null_symbols = {'column1': ['NA'], 'column2': ['Null']}

# load data where with column-specific missing values
sample2 = pd.read_csv("file_2_name.csv", na_values=null_symbols)
```

76. `pd.read_csv` includes other function arguments that can help with other common data formatting issues.

Argument | Description
--- | ---
`sep` or `delimiter` | Specifies the character sequence or regular expression used as a separator or delimiter
`header` | Specifies the row number to use as column names; `0` is default (does not need to be specified); `header=None` if no header
`index_col` | Specifies the column numbers or names to use as row index 
`names` | Specifies list of column names for result
`skiprows` | Row numbers (starting at 0) for rows to skip
`na_values` | Sequence of characters that represent missing or NA data
`comment` | Characters used to mark or split off comments that occur at end of lines of data
`parse_dates` | Specifies how date and time data will be parsed; `False` by default; `True` will attempt to parse all columns as `datetime` format; can also apply to select columns
`dayfirst` | Specifies international date format (DD/MM/YYYY); `False` by default
`date_parser` | Function used to parse dates
`nrows` | Number of rows to read starting at the beginning of the file; especially helpful when only needing part of a large file
`skip_footer` | Number of lines to ignore at the end of the file
`encoding` | Specifies encoding schema
`thousands` | Specifies `,` or `.` separater for thousands

77. Other elements of `.csv` dialect we might encounter when loading a file to a `DataFrame`:

Argument | Description
--- | ---
`delimiter` | One character string used to separate fields; default is `,`
`quotechar` | Quote character for fields with specific characters or fields that inclde the delimiter character
`skippinitialspace` | Instructs program to ignore whitespace after delimiter; default is `False`
`doublequote` | Specifies how to handle quoting character within a field
`escapechar` | Specifies the string used to escape the delimiter character if `quoting` is set to `QUOTE_NONE`

<blockquote>Q3: Write code that loads in a different data file as a DataFrame and accomplishes each of the following tasks. Include code + comments.
 <ul><li>Shows the first five rows</li>
  <li>Shows the last five rows</li>
  <li>Checks the data types for each column</li>
  <li>Returns a technical summary for the DataFrame</li>
 </ul>
 </blockquote>
 
# Interacting with a `DataFrame`

## Sorting

78. `pandas` includes a few different built-in sorting operations.

79. We can sort by an index for either axis of our `DataFrame` (i.e. we can sort based on row index labels or by column name).

80. Going back to our Titanic passenger data, let's say we wanted to sort by passenger age.

```Python
# import pandas
import pandas as pd

# load titanic data from url
titanic = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/eda-pandas/main/data/titanic.csv")

# show first 5 rows of newly-loaded dataframe
titanic.head(5)

# sort by passenger age and show first five rows of the sorted data
titanic.sort_values(by="Age").head()
```

81. The default for `.sort_values` is to sort in ascending order.

82. We can use `ascending=False` to sort in descending order.
```Python
titanic.sort_values(by=['Age'], ascending=False)
```

83. NOTE: When sorting, we are returning a sorted `DataFrame`. We ARE NOT updating the `DataFrame` in place. 

84. We have a couple of options to sort in place.
```Python
# create new dataframe with sorted results
titanic_by_age = titanic.sort_values(by="Age")

# check newly-created dataframe
titanic_by_age.head()

# sort values in place
titanic.sort_values(['Age'], inplace=True)

# check newly-created dataframe
titanic.head()
```

85. We can also sort by multiple fields.

86. To sort by class cabin and age, in descending order:
```Python
titanic.sort_values(by=['Pclass', 'Age'], ascending=False).head()
```

87. When sorting by fields with string data, `a-z` is considered `ascending` and `z-a` would be `descending`.

## Subsetting

### Select

88. To review, we can select specific columns from a `DataFrame`. 
```Python
# creates Series object with age values
ages = titanic["Age"]

# show new object
ages
```

89. We can use `[" "]` to select a specific single column of interest. 

90. Python returns this single column's data as a `Series` object.

91. We can also create a new data frame based on multiple columns.
```Python
# selects multiple columns to form new dataframe
age_sex = titanic[["Age", "Sex"]]

# checks first five rows of new dataframe
age_sex.head()
```

92. When selecting multiple columns, the inner brackets (`[]`) define the column names to subset or select.

93. The outer brackets select data from a dataframe.

94. In this multi-column example, `age_sex` is a `DataFrame` because it is a two-dimensional object.

95. For more on sorting operations in `pandas`, check out the package's ["Sorting" documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#sorting).

### Filter

96. We can select or return rows in our `DataFrame` that meet specific conditions.

97. Let's say we wanted to create a new `DataFrame` only containing data for passengers older than 35 years.
```Python
above_35 = titanic[titanic["Age"] > 35]

# check first five rows of newly-created dataframe
above_35.head()
```

98. We use brackets (`[]`) to set a condition rows must meet to be assigned to the new dataframe.

99. If we just wanted to see whether rows meet this condition in the original `DataFrame`, we could just test for the condition without creating a new `DataFrame`.
```Python
titanic["Age"] > 35
```

100. Maybe we want to create a new data frame containing data on passegers in cabin class 2 and 3.
```Python
class_23 = titanic[titanic["Pclass"].isin([2, 3])]

# check first five rows of newly-created dataframe
class_23.head()
```

101. The `isin()` conditional function on its own would return a `True` or `False` value.

102. By nesting the `isin()` function in brackets (`[]`), we are filtering rows based on rows  that meet the function critera, or return as `True` from this function.

103. We could also break out the chained or compound conditional statement using an `OR` operator, `|`.
```Python
class_23_alternate =  titanic[(titanic["Pclass"] == 2) | (titanic["Pclass"] == 3)]

class_23_alternate.head()
```

104. For more on Boolean indexing and the `isin()` function:
- ["Boolean indexing," Pandas documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-boolean)
- ["Indexing with isin," Pandas documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-basics-indexing-isin)

105. We could also create a new dataframe with passenger data for only passengers that have a known age.
```Python
age_known = titanic[titanic["Age"].notna()]

age_known.head()
```

106. `.notna()` is a conditional function that returns `True` for rows that do not have a `Null` value.

107. For more on missing values and related functions, check out [the "Working with missing data" package documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/missing_data.html#missing-data).

### Selecting specific rows and columns

108. Selecting lets us isolate columns, and filtering identifies specific rows.

109. But we can imagine a scenario in which we would want to combine these elements.

110. We might want to create a new dataframe containing only the names of passengers who are over 35 years old.
```Python
over_35_names = titanic.loc[titanic["Age"] > 35, "Name"]

over_35_names
```

111. `.loc` identifies the rows we are writing to the new dataframe.

112. What we are passing to the `loc` operator includes the row filter condition (`titanic["Age"] > 35`) and the column we are writing to the new dataframe (`Name`).

113. We can also select rows and columns based on their index position.

114. We could isolate rows 10-25 and columns 3-5 with the following expression:
```Python
titanic.iloc[9:25, 2:5]
```

115. We can also assign new values to our selection using `loc` or `iloc`.

116. `loc` isolates rows based on their value, while `iloc` isolates rows based on their index position or label.

117. Let's say we wanted to anonymize the first three names in the dataset.

118. We could do this using `titanic.iloc[0:3, 3] = "anonymous"`.

119. Consult the ["Different choices for indexing"](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-choice) documentation for more on indexing options.

120. A few key takeaways:
- We use square brackets `[]` to subset data.
- We can specify single rows or columns, a list of rows/columns, or conditional expression within those brackets
- Select specific rows and/or columns using `loc` when working with row/column names
- Select specific rows and/or columns usign `iloc` when working with index positions
- You can assign new values to selection using `loc` or `iloc`

## Removing duplicates

121. A useful place to start is identifying and removing any duplicate rows in a dataframe.

122. We can do this using a few key functions.

123. `.duplicated()` will return a `True` or `False` value indicating if a row is a duplicate of a previously occuring row.
```Python
data_frame.duplicated()
```

124. `.drop_duplicates()` will return a dataframe containing only rows that are not duplicated.
```Python
new_data_frame = old_data_frame.drop_duplicates()
```

<blockquote>Q4: Using the DataFrame you created for Q3, write code that executes AT LEAST FOUR of the following tasks. Include code + comments.
 <ul>
  <li>Sorts a column by ascending values</li>
  <li>Sorts a column by descending values</li>
  <li>Selects a specific column in the DataFrame</li>
  <li>Creates a new DataFrame with select columns from existing DataFrame</li>
  <li>Uses a comparison operator to filter rows in the DataFrame</li>
  <li>Uses an isin statement to filter rows in the DataFrame</li>
  <li>Selects specific rows and columns</li>
  <li>Removes duplicate rows</li>
 </ul>
 </blockquote>
 
# Summary Statistics and Calculations

123. `pandas` comes with built-in functionality for performing common mathematical and statistical calculations.

124. Most of these mathematical methods fall under the umbrella of summary statistics.

125. We could use `.mean()` to get the average age of Titanic passengers.
```Python
# load titanic data from url
titanic = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/pandas-intro/main/data/titanic.csv")

# show first 5 rows of newly-loaded dataframe
titanic.head(5)

# calculate mean
titanic["Age"].mean()
```

126. We could also calculate the median age and ticket fare using `.median()`.
```Python
titanic[["Age", "Fare"]].median()
```

127. By default, these descriptive statistics will ignore missing data.

128. In a situation where missing data or `NaN` values need to be part of the calculation, we would set `skipna` to `False`.
```Python
titanic.sum(axis=0, skipna=True)
```

129. In the `.sum()` example, we use `axis=` to specify what axis to perform the mathematical operation on.
- `axis=0` indicates columns
- `axis=1` indicates rows

130. `skipna=True` means rows with missing data will not be part of the mathematical operation.

131. Remember `.describe()` returns aggregate information on the entire dataset.

132. We can use `.agg()` to return specific combinations of aggregate statistics for specific columns.

133. Say we wanted to compute `min`, `max`, `median`, and `skew` for the `Age` column and `min`, `max`, `median`, and `mean` for the `Fare` column.

134. We can specify those statistics and those columns using `.agg()`.
```Python
titanic.agg(
  {
    "Age": ["min", "max", "median", "skew"],
    "Fare": ["min", "max", "median", "mean"],
   }
 )
```

134. Consult `pandas` ["Descriptive statistics"](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#descriptive-statistics) documentation for more on descriptive statistics in `pandas`.

135. We can also aggregate summary statistics by category, using `.groupby()`.

136. To find out the average age for female versus male passengers:
```Python
titanic[["Sex", "Age"]].groupby("Sex").mean()
```

137. The first set of brackets (`[]`) isolates a subselection with only these two columns.

138. Then we apply the `.groupby()` method to the `Sex` column to calculate `.mean()` for each unique value represented in the `Sex` field.

<p align="center"><a href="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_1.svg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_1.svg?raw=true" /></a></p>

139. Generally speaking, this type of `pandas` operation follows a `split-apply-combine` pattern.
- First we `split` the data into groups.
- Then we `apply` a function to each group independently.
- Then we `combine` the function results into a data structure.

140. `pandas` syntax combines the `apply` and `combine` steps.

141. Another way to calculate average age by gender:
```Python
titanic.groupby("Sex")["Age"].mean()
```

<p align="center"><a href="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_2.svg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_2.svg?raw=true" /></a></p>

142. In this alternate example, the column name in the `.groupby()` parenthesis specifies the column to group by.

143. The column name in brackets (`[]`) specifies the column to perform the mathematical function on.

144. As a last example, let's say we want the mean ticket fare price for each of the gender and cabin class combinations.

145. We can think through the underlying logic for that program.
- First we need to `split` the data into groups for gender and cabin class
- Then we need to `apply` a mean calculation to the ticket fare column for each of those  groups
- The last step will be to `combine` all that information into a data structure

146. To express that programatically in Python:
```Python
titanic.groupby(["Sex", "Pclass"])["Fare"].mean()
```

147. Consult `pandas` ["Group by" documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/groupby.html#groupby) to learn more about this function.

148. We can also count the number of records by category using `.value_counts()`.

149. The `.value_counts()` method returns the number of records for each category in a column.

150. Let's say we wanted to know the number of passengers for each cabin class.

151. Thinking through the underlying logic, we want Python to count how many records are contained for each unique value in the `Pclass` column.

152. That requires identifying the unique values, counting the number of instances for each, and returning those values as a sum.

153. To express that progrmatically in Python:
```Python
titanic["Pclass"].value_counts()
```

154. We could also break out these steps using a combination of `.groupby()` and `.count()`:
```Python
titanic.groupby("Pclass")["Pclass"].count()
```

155. In the second example, we are explicitly categorizing the data by category in the `Pclass` column, then performing a `.count()` operation on the records for each `Pclass` category.

156. A few key takeaways:
- We can calculate aggregate statistics for entire rows or columns
- `.groupby()` follows a `split-apply-combine` pattern
- `.value_counts()` can be a shorthand for getting the number of entries for each category in a field

<blockquote>Q5: Using the titanic dataset (or another dataset), write code that calculates at least 3 unique summary statistics (.agg() counts as one). Include code + comments for each.</blockquote>

## Creating New Columns Based on Existing Columns

157. Let's introduce a new sample dataset, this time with air quality data for measurement stations in London, Paris, and Antwerp.

158. Values in this dataset include nitrogen dioxide (<code>NO<sub>2</sub></code>) concentration expressed as parts per million (`ppm`).

```Python
# load air quality data from url
air_quality = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/eda-pandas/main/data/air_quality_no2.csv", index_col=0, parse_dates=True)

# show first 5 rows of newly-loaded dataframe
air_quality.head()
```

159. Let's say we wanted to express the London station's <code>NO<sub>2</sub></code> concentration as milligrams per cubic meter (mg/m<sup>3</sup>).

160. For our purposes, we are assuming a temperature of 25 degrees Celsius and pressure of 1013 hPa, which means the conversion factor is 1.882.

161. We would need to convert all of the `station_london` column values from `ppm` to <code>mg/m<sup>3</sup></code>. And we would want to store the results of that calculation in a newly-created column.

<p align="center"><a href="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_2.svg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_2.svg?raw=true" /></a></p>

162. To express those steps programatically in Python:
```Python
air_quality["london_mg_per_cubic"] = air_quality["station_london"] * 1.882

air_quality.head()
```

163. Note that we do not need to iterate over all rows for a specific dataframe column to perform this calculation.

164. `pandas` performs the calculation `element_wise`, that is on all of the values in the column at once.

165. Let's say we wanted to calculate the ratio of the Paris versus Antwerp station values and store that result in a new column.

166. We would need to calculate the ratio for each row and store the results of the calculation in a new column.

<p align="center"><a href="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_3.svg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/eda-pandas/blob/main/figures/Figure_3.svg?raw=true" /></a></p>

167. To express those steps programatically in Python:
```Python
air_quality["ratio_paris_antwerp"] = (air_quality["station_paris"] / air_quality["station_antwerp"])

air_quality.head()
```

168. Again, because this is an element-wise calculation, the division operation is applied to all rows in the data frame.

169. Python's other mathematical (`+`, `-`, `*`, `/`) and logical (`<`, `>`, `=`, etc.) all work element-wise.

<blockquote>Q6: Describe element-wise calculation in your own words.</blockquote>

<blockquote>Q7: Using the air quality data or another dataset, write code that generates a new column based on an existing column(s). Include code + comments.</blockquote>

# Renaming, Mapping, and Reindexing

170. As a result or as part of data wrangling operations, you may need to rename columns or renumber the rows in a dataframe.

## Renaming Columns

171. We can use `.rename()` to rename columns in a dataframe.

172. We use a dictionary's key-value pairs to specify the new name for an existing column.

173. To do this in place and change the names in an existing dataframe:
```Python
data_frame.rename(columns={"old_name_1": "new_name_1", "old_name_2": "new_name_2", "old_name_3": "new_name_3"})

data_frame.head()
```

174. We can also create a new dataframe with the renamed columns.
```Python
new_data_frame = old_data_frame.rename(columns={"old_name_1": "new_name_1", "old_name_2": "new_name_2", "old_name_3": "new_name_3"})

new_data_frame.head()
```

175. We can also standardize axis labels using `str` case methods (`upper`, `lower`, `title`, etc.)
```Python
# relabel index labels to upper case version of existing labels
data_frame.rename(str.upper)

# relabel index labels to lower case version of existing labels
data_frame.rename(str.lower)
```

176. We can also rename row index labels usign `.rename()`.
```Python
data_frame.rename({"old_row_1": "new_row_1", "old_row_2": "new_row_2", "old_row_3": "new_row_3"}, axis="index")
```

177. In the `data_frame.rename()` examples, the `.rename()` method is applied to a copy of the data--the underlying data is not changed.

178. To alter the underlying data, we would set the `inplace` parameter to `True`.
```Python
data_frame.rename(RENAMING OPERATION, inplace=True)
```

179. For more on renaming functions, consult the `pandas` documentation for [`pandas.DataFrame.rename`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html#pandas.DataFrame.rename).

180. We can also imagine a scenario in which you have sliced or filtered the data and now have row index labels that no longer make sense.

181. For example, if your original rows had sequential numerical index labels, the transformed data will retain the original index labels.

182. In these situations, we can reset the index to a simple ascending integer index using `.reset_index()`.

```Python
data_frame.reset_index()
```

183. For more on indexing, consult `pandas` ["Different choices for indexing"](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html?highlight=reindex#different-choices-for-indexing) documentation.

# Getting started with `matplotlib`

184. For our purposes, a plot is defined as "a graphic representation (such as a chart)" (Merriam Webster).

185. These graphic representations of data are often called charts, graphs, figures, etc. 

186. In the context of programming, computer science, and data science, we refer to these as plots.

187. We can generate plots for data stored in `pandas` using the `matplotlib` package.

188. `matplotlib` was developed in 2002 as a MATLAB-like plotting interface for Python.

189. "Matplotlib is a comprehensive library for creating static, animated, and interactive visualizations in Python...Matplotlib produces publication-quality figures in a variety of hardcopy formats and interactive environments across platforms. Matplotlib can be used in Python scripts, the Python and IPython shell, web application servers, and various graphical user interface toolkits" ([Matplotlib documentation, Github](https://github.com/matplotlib/matplotlib))

190. As described [by the original developer John Hunter](https://matplotlib.org/users/history.html), "Matplotlib is a library for making 2D plots of arrays in Python. Although it has its origins in emulating the MATLAB graphics commands, it is independent of MATLAB, and can be used in a Pythonic, object oriented way. Although Matplotlib is written primarily in pure Python, it makes heavy use of NumPy and other extension code to provide good performance even for large arrays. Matplotlib is designed with the philosophy that you should be able to create simple plots with just a few commands, or just one! If you want to see a histogram of your data, you shouldn't need to instantiate objects, call methods, set properties, and so on; it should just work."

191. For more on `matplotlib`'s development and history: John Hunter, ["History"](https://matplotlib.org/users/history.html) *Matplotlib* (2008)

192. To be able to call the `matplotlib` API (application programming interface) within Python, we need to make sure the package is installed and loaded.
- To install at the command line: `pip install matplotlib`
- To load in a `.py` script: `import matplotlib.pyplot as plot`
- To work with `matplotlib` from a Jupyter notebook: `%matplotlib notebook`

193. The default `matplotlib` plot is a line plot.

```Python
# import matplotlib
import matplotlib.pyplot as plt

# import numpy
import numpy as np

# create dataset of numbers 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
data = np.arange(10)

# create line plot of data 
plt.plot(data)
```

194. We pass our data to `plt.plot()` and a line plot is generated.

195. Plots in `matplotlib` reside within a `Figure` object.

196. These figures contain one or more `Axes`, which are the area where points can be added or specified usign x-y coordinates for 2-D plots.

197. Once the `Figure` object has been created, we can add start adding elements to the plot.

198. The easiest way to create a figure is using `pyplot.subplots()`.

199. NOTE: Because we have loaded the package using `import matplotlib.pyplot as plot`, we can use `plt` as shorthand for `pyplot`.

200. We can then use `axes.plot()` to create axes and add data.

201. Another line plot using a list of number values:
```Python
# import package
import matplotlib.pyplot as plt

# create figure with axes
fig, ax = plt.subplots()

# plot data on axes
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])
```

202. We can see the first list of `1, 2, 3, 4` is shown on the `X` axis.

203. The second list `1, 4, 2, 3` is shown on the `Y` axis.

204. NOTE: The `Y` axis values are plotted in the original order--for this type of plot (a line plot), `matplotlib` keeps the list data in its original order.

205. Let's generate a line plot using a square number sequence

```Python
# import matplotlib
import matplotlib.pyplot as plt

# create dataset for y axis
squares = [1, 4, 9, 16, 25]

# create dataset for x axis
input_values = [1, 2, 3, 4, 5]

# create figure for new plot
fig, ax = plt.subplots()

# generate plot
ax.plot(input_values, squares)

# show plot
plt.show()
```

206. In this example, we import `matplotlib` and create two lists, `squares` and `input_values` that hold the data that will be plotted.

207. NOTE: We need to specify `input_values`, because the default in `matplotlib` will start the axis at `0`.

208. We then create a new variable `fig` that represents the entire figure or collection of plots.

209. The variable `ax` represents a single plot in the `fig` figure.

210. We then pass `squares` to the `plot()` method to create the plot.

211. And `plt.show()` opens the `matplotlib` viewer to show us the newly-generated plot.

212. From within the `matplotlib` viewer, we can zoom in and navigate the plot, as well as save images of the plot.

# Anatomy of a `matplotlib` figure

213. Before we start customizing plots or generating more complex plots, it's useful to know the components of a `matplotlib` figure.

<p align="center"><a href="https://github.com/kwaldenphd/matplotlib-intro/blob/main/figures/Figure_1.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/matplotlib-intro/blob/main/figures/Figure_1.png?raw=true" /></a></p>

## `Figure`

214. `Figure`: A figure object that can include multiple `Axes` or plots; a `Figure` contains at least one `Axes`

```Python
# create an empty figure with no axes
fig = plt.figure()

# create a figure with a single axes
fig, ax = plt.subplots()

# create a figure with a 2x2 grid of axes
fig, axs = plt.subplots(2, 2)
```

215. Having multiple `Axes` in the same `Figure` is useful when creating side-by-side visualizations or a dashboard-style collection of visualizations.

## `Axes`

216. In `matplotlib` syntax, `Axes` are what we would think of as a single plot, where data is plotted.

217. A `Figure` can contain many `Axes`, but a given `Axes` object can only be in one `Figure`.

218. For cartesian coordinate plane visualizations, an `Axes` contains two `Axis` objects.

## `Axis`

219. `matplotlib` works in a Cartesian coordinate system, with an `X` (horizontal) and `Y` (vertical) axis.

220. In a `matplotlib` plot, the `Axis` objects set graph limits and generate tick marks and labels.

221. The location of ticks is determined by a `Locator` object.

222. Tick labels are strings formatted using `Formatter`.

## Everything Else (`Artists`)

223. The other components of the `Figure` include things like axis labels, marker or line style, tick labels, figure title, etc.

224. These are all referred to as `Artists` in `matplotlib` documentation.

225. Knowing how to configure or customize these plot components is not just about aesthetics--in many cases, customizing a plot is necessary for readability.

226. For more on `matplotlib`:
- [Introduction to `matplotlib`](https://github.com/kwaldenphd/matplotlib-intro)
- [More with `matplotlib`](https://github.com/kwaldenphd/more-with-matplotlib/)

# `pandas` and `matplotlib`

227. Having to load data manually to build a visualization or plot gets cumbersome quickly.

228. In many situations, we might want to work with data in a `pandas` `DataFrame` when building a visualization.

229. `pandas` includes a `.plot()` attribute that interacts with the `matplotlib` API to generate plots.

230. The `pandas` `.plot()` attribute relies on the `matplotlib` API to generate plots, so our work with `matplotlib` will come in handy when we need to customize plots generated using `.plot()`.

231. And in many cases, the `.plot()` syntax is similar to `matplotlib` `OO` syntax.

## Plotting in `pandas` Using `.plot()`

232. Let's go back to the air quality data we were working with previously in `pandas`.

233. To load the data as a dataframe:
```Python
# import pandas
import pandas as pd

# load data from url to dataframe
air_quality = pd.read_csv('https://raw.githubusercontent.com/kwaldenphd/more-with-matplotlib/main/air_quality_no2.csv', index_col=0, parse_dates=True)

# check data has loaded
air_quality.head()
```

234. We can do a quick visual check of the data by passing the entire data frame to `.plot()`.
```Python
# import matplotlib
import matplotlib.pyplot as plt

# generate plot
air_quality.plot()

# show plot
plt.show()
```

235. This isn't a particularly meaningful visualization, but it shows us how the default for `.plot()` creates a line for each column with numeric data.

236. The index is used for the `X` axis, and all numeric columns are plotted on the `Y` axis.

237. We can also see that `.plot()` pulls tick marks, tick labels, and axis titles from the underlying `dataframe`.

238. Let's say we only wanted to plot Paris data.

239. We can select that column in the `dataframe` before calling `.plot()`.
```Python
air_quality["station_paris"].plot()
```

240. We can plot a specific column in the `dataframe` using the `[" "]` selection method.

241. Let's say we want to visually compare NO<sub>2</sub> values measured in London and Paris.

242. We need to specify what column is going to be used for the `X` axis as well as what column is going to be used for the `Y` axis.

243. For this example, a scatterplot will be more effective than a lineplot.

244. We can create a scatterplot using `.plot.scatter()`.

```Python
air_quality.plot.scatter(x="station_london", y="station_paris", alpha=0.5)
```

245. This example generates a scatterplot with London data on the `X` axis and Paris data on the `Y` axis.

246. While the default for `.plot()` is a lineplot, there are a number of other methods we can use with `.plot()`.

247. Again, you will see some overlap with `matplotlib` syntax.

248. We can use these strings as as method in combination with `.plot()` or we can pass them to `.plot()` as a `kind` parameter.

Plot Type | Method Syntax | Parameter Syntax
--- | --- | ---
Default (line) | `.plot()` | NA
Area | `.plot.area()` | `.plot(kind='area')`
Bar | `.plot.bar()` | `.plot(kind='bar')`
Horizontal bar | `.plot.barh()` | `.plot(kind='barh')`
Box | `.plot.box()` | `.plot(kind='box')`
Density | `.plot.density()` | `.plot(kind='density')`
Hexbin | `.plot.hexbin()` | `.plot(kind='hexbin')`
Histogram | `.plot.hist()` | `.plot(kind='hist')`
Kernel density | `.plot.kde()` | `.plot(kind='hist')`
Line | `.plot.line()` | `.plot(kind='line')` *this would be redundant since the default for `.plot()` is line*
Pie | `.plot.pie()` | `.plot(kind='pie')`
Scatter | `.plot.scatter()` | `.plot(kind='scatter')`

249. In general, it's more effective to use the method syntax to note plot type, rather than treating plot type as a parameter.

250. Let's create a box plot using our air quality data.
```Python
air_quality.plot.box()
```

251. We can see each numeric column (each station) has its own box in the default boxplot.

252. Let's say we wanted to generate a lineplot with separate subplots for each of the numeric columns.

253. We can accomplish this by setting the `subplots` parameter to `True`.

```Python
axs = air_quality.plot.area(figsize=(12, 4), subplots=True)
```

254. Let's say we want to further customize this plot.

255. This is where we start to see more `matplotlib` syntax in play.

```Python
# create figure and axes
fig, axs = plt.subplots(figsize=(12, 4))

# build area plot
air_quality.plot.area(ax=axs)

# set y axis label
axs.set_ylabel("NO$_2$ concentration")

# save plot as png file
fig.savefig("no2_concentrations.png")
```

256. Each plot object created by `pandas` is also a `matplotlib` object.

257. Having a deep knowledge of `matplotlib` allows you to customize plots generated using `pandas` `.plot()` function.

258. That said, there are some parameters you can set as part of `.plot()` without having to have separate `matplotlib` syntax commands.

259. The plot `kind` is one parameter option, as is the `dataframe`, `X` axis data, and `Y` axis data.

260. Other parameters:

Parameter | Explanation
--- | ---
`ax` | `matplotlib` `axes` object; axes for the current figure
`subplots` | Default is `False`; set to `True` to enable multiple plots in the same figure
`layout` | Follows `(rows, columns)` syntax to set subplot layout
`figsize` | Follows `(width, height)` syntax; Sets figure object width and height in inches
`use_Index` | Default is `True`; uses dataframe index as ticks for `X` axis
`title` | Title to use for the plot; can take a list with titles for corresponding subplots
`grid` | Default is `False`; set to `True` to show axis grid lines
`legend` | Places legend on axis subplot
`xticks` | Values to use for `X` axis ticks
`yticks` | Values to use for `Y` axis ticks
`xlim` | Follows `(lower limit, upper limit)` syntax; Sets `X` axis limits
`ylim` | Follows `(lower limit, upper limit)` syntax; Sets `Y` axis limits
`xlabel` | Label for `X` axis; default uses index column name
`ylabel` | Label for `Y` axis; default uses `Y` column name
`fontsize` | Sets font size for tickmarks
`colormap` | Sets `matplotlib` colormap to select colors from
`table` | Default is `False`; set to `True` to draw a table from data in the `DataFrame`
`stacked` | Default is `False` in line and bar plots, `True` in area plot; if `True`, creates stacked plot

261. For more parameters that can be passed to `.plot()`: [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)

<blockquote>Q8: Describe in your own words the core components of a matplotlib figure. What is the general sequence of steps involved in generating a matplotlib figure?</blockquote>

# Lab Notebook Questions

[Click here](https://colab.research.google.com/drive/1HPD6LuBSZjbyAZVwRMD-lBqwB2H3wwxM?usp=sharing) to access the lab notebook template as a Jupyter Notebook.

Q1: Describe a DataFrame in your own words.

Q2: Create your own small DataFrame. Write code that accomplishes the following tasks. Include code + comments.
- Change the original column order
- Select a specific column(s) using its index label or name attribute
- Select a specific row(s) using its index label or index value
- Remove a column from the DataFrame
- Determine summary statistics for values in the DataFrame

Q3: Write code that loads in a different data file as a DataFrame and accomplishes each of the following tasks. Include code + comments.
- Shows the first five rows
- Shows the last five rows
- Checks the data types for each column
- Returns a technical summary for the DataFrame

Q4: Using the DataFrame you created for Q3, write code that executes AT LEAST FOUR of the following tasks. Include code + comments.
- Sorts a column by ascending values
- Sorts a column by descending values
- Selects a specific column in the DataFrame
- Creates a new DataFrame with select columns from existing DataFrame
- Uses a comparison operator to filter rows in the DataFrame
- Uses an isin statement to filter rows in the DataFrame
- Selects specific rows and columns
- Removes duplicate rows

Q5: Using the titanic dataset (or another dataset), write code that calculates at least 3 unique summary statistics (.agg() counts as one). Include code + comments for each.

Q6: Describe element-wise calculation in your own words.

Q7: Using the air quality data or another dataset, write code that generates a new column based on an existing column(s). Include code + comments.

Q8: Describe in your own words the core components of a matplotlib figure. What is the general sequence of steps involved in generating a matplotlib figure?
