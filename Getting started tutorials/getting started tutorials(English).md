# 1.What kind of data does pandas handle?

*I want to start using pandas*

```python
In [1]: import pandas as pd
```

To load the pandas package and start working with it, import the package. The community agreed alias for pandas is `pd`, so loading pandas as `pd` is assumed standard practice for all of the pandas documentation.

## 1.1 pandas data table representation

![](https://pandas.pydata.org/docs/_images/01_table_dataframe.svg)

*I want to store passenger data of the Titanic. For a number of passengers, I know the name (characters), age (integers) and sex (male/female) data.*

```python
In [2]: df = pd.DataFrame(
   ...:     {
   ...:         "Name": [
   ...:             "Braund, Mr. Owen Harris",
   ...:             "Allen, Mr. William Henry",
   ...:             "Bonnell, Miss. Elizabeth",
   ...:         ],
   ...:         "Age": [22, 35, 58],
   ...:         "Sex": ["male", "male", "female"],
   ...:     }
   ...: )
   ...: 

In [3]: df
Out[3]: 
                       Name  Age     Sex
0   Braund, Mr. Owen Harris   22    male
1  Allen, Mr. William Henry   35    male
2  Bonnell, Miss. Elizabeth   58  female
```

To manually store data in a table, create a `DataFrame`. When using a Python dictionary of lists, the dictionary keys will be used as column headers and the values in each list as columns of the `DataFrame`.

A [`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame) is a 2-dimensional data structure that can store data of different types (including characters, integers, floating point values, categorical data and more) in columns. It is similar to a spreadsheet, a SQL table or the `data.frame` in R.

* The table has 3 columns, each of them with a column label. The column labels are respectively `Name`, `Age` and `Sex`.
* The column `Name` consists of textual data with each value a string, the column `Age` are numbers and the column `Sex` is textual data.

In spreadsheet software, the table representation of our data would look very similar:

![](https://pandas.pydata.org/docs/_images/01_table_spreadsheet.png)

## 1.2 Each column in a `DataFrame` is a `Series`

![](https://pandas.pydata.org/docs/_images/01_table_series.svg)

*I’m just interested in working with the data in the column `Age`*

```python
In [4]: df["Age"]
Out[4]: 
0    22
1    35
2    58
Name: Age, dtype: int64
```

When selecting a single column of a pandas [`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame), the result is a pandas [`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series). To select the column, use the column label in between square brackets `[]`.

> If you are familiar with Python [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#tut-dictionaries), the selection of a single column is very similar to the selection of dictionary values based on the key.

You can create a `Series` from scratch as well:

```python
In [5]: ages = pd.Series([22, 35, 58], name="Age")

In [6]: ages
Out[6]: 
0    22
1    35
2    58
Name: Age, dtype: int64
```

A pandas `Series` has no column labels, as it is just a single column of a `DataFrame`. A Series does have row labels.

## 1.3 Do something with a DataFrame or Series

*I want to know the maximum Age of the passengers*

We can do this on the `DataFrame` by selecting the `Age` column and applying `max()`:

```python
In [7]: df["Age"].max()
Out[7]: 58
```

Or to the `Series`:

```python
In [8]: ages.max()
Out[8]: 58
```

As illustrated by the `max()` method, you can do things with a `DataFrame` or `Series`. pandas provides a lot of functionalities, each of them a method you can apply to a `DataFrame` or `Series`. As methods are functions, do not forget to use parentheses `()`.

*I’m interested in some basic statistics of the numerical data of my data table*

```python
In [9]: df.describe()
Out[9]: 
             Age
count   3.000000
mean   38.333333
std    18.230012
min    22.000000
25%    28.500000
50%    35.000000
75%    46.500000
max    58.000000
```

The [`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe) method provides a quick overview of the numerical data in a `DataFrame`. As the `Name` and `Sex` columns are textual data, these are by default not taken into account by the [`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe) method.

Many pandas operations return a `DataFrame` or a `Series`. The `describe()` method is an example of a pandas operation returning a pandas `Series` or a pandas `DataFrame`.

> This is just a starting point. Similar to spreadsheet software, pandas represents data as a table with columns and rows. Apart from the representation, also the data manipulations and calculations you would do in spreadsheet software are supported by pandas. Continue reading the next tutorials to get started!

**REMEMBER**

- Import the package, aka import pandas as pd
- A table of data is stored as a pandas DataFrame
- Each column in a DataFrame is a Series
- You can do things by applying a method to a DataFrame or Series

# 2.How do I read and write tabular data?

![](https://pandas.pydata.org/docs/_images/02_io_readwrite.svg)

```
This tutorial uses the Titanic data set, stored as CSV. The data consists of the following data columns:
- PassengerId: Id of every passenger.
- Survived: Indication whether passenger survived. 0 for yes and 1 for no.
- Pclass: One out of the 3 ticket classes: Class 1, Class 2 and Class 3.
- Name: Name of passenger.
- Sex: Gender of passenger.
- Age: Age of passenger in years.
- SibSp: Number of siblings or spouses aboard.
- Parch: Number of parents or children aboard.
- Ticket: Ticket number of passenger.
- Fare: Indicating the fare.
- Cabin: Cabin number of passenger.
- Embarked: Port of embarkation.
```

*I want to analyze the Titanic passenger data, available as a CSV file.*

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")
```

pandas provides the [`read_csv()`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv) function to read data stored as a csv file into a pandas `DataFrame`. pandas supports many different file formats or data sources out of the box (csv, excel, sql, json, parquet, …), each of them with the prefix `read_*`.

Make sure to always have a check on the data after reading in the data. When displaying a `DataFrame`, the first and last 5 rows will be shown by default:

```python
In [3]: titanic
Out[3]: 
     PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0              1         0       3  ...   7.2500   NaN         S
1              2         1       1  ...  71.2833   C85         C
2              3         1       3  ...   7.9250   NaN         S
3              4         1       1  ...  53.1000  C123         S
4              5         0       3  ...   8.0500   NaN         S
..           ...       ...     ...  ...      ...   ...       ...
886          887         0       2  ...  13.0000   NaN         S
887          888         1       1  ...  30.0000   B42         S
888          889         0       3  ...  23.4500   NaN         S
889          890         1       1  ...  30.0000  C148         C
890          891         0       3  ...   7.7500   NaN         Q

[891 rows x 12 columns]
```

*I want to see the first 8 rows of a pandas DataFrame.*

```python
In [4]: titanic.head(8)
Out[4]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S
5            6         0       3  ...   8.4583   NaN         Q
6            7         0       1  ...  51.8625   E46         S
7            8         0       3  ...  21.0750   NaN         S

[8 rows x 12 columns]
```

To see the first N rows of a `DataFrame`, use the [`head()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.head.html#pandas.DataFrame.head) method with the required number of rows (in this case 8) as argument.

> Interested in the last N rows instead? pandas also provides a [`tail()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.tail.html#pandas.DataFrame.tail) method. For example, `titanic.tail(10)` will return the last 10 rows of the DataFrame.

A check on how pandas interpreted each of the column data types can be done by requesting the pandas `dtypes` attribute:

```python
In [5]: titanic.dtypes
Out[5]: 
PassengerId      int64
Survived         int64
Pclass           int64
Name            object
Sex             object
Age            float64
SibSp            int64
Parch            int64
Ticket          object
Fare           float64
Cabin           object
Embarked        object
dtype: object
```

For each of the columns, the used data type is enlisted. The data types in this `DataFrame` are integers (`int64`), floats (`float64`) and strings (`object`).

>  When asking for the `dtypes`, no brackets are used! `dtypes` is an attribute of a `DataFrame` and `Series`. Attributes of a `DataFrame` or `Series` do not need brackets. Attributes represent a characteristic of a `DataFrame`/`Series`, whereas methods (which require brackets) do something with the `DataFrame`/`Series` as introduced in the [first tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html#min-tut-01-tableoriented).

*My colleague requested the Titanic data as a spreadsheet.*

```python
In [6]: titanic.to_excel("titanic.xlsx", sheet_name="passengers", index=False)
```

Whereas `read_*` functions are used to read data to pandas, the `to_*` methods are used to store data. The [`to_excel()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html#pandas.DataFrame.to_excel) method stores the data as an excel file. In the example here, the `sheet_name` is named passengers instead of the default Sheet1. By setting `index=False` the row index labels are not saved in the spreadsheet.

The equivalent read function `read_excel()` will reload the data to a `DataFrame`:

```python
In [7]: titanic = pd.read_excel("titanic.xlsx", sheet_name="passengers")

In [8]: titanic.head()
Out[8]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

*I’m interested in a technical summary of a `DataFrame`*

```python
In [9]: titanic.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   PassengerId  891 non-null    int64  
 1   Survived     891 non-null    int64  
 2   Pclass       891 non-null    int64  
 3   Name         891 non-null    object 
 4   Sex          891 non-null    object 
 5   Age          714 non-null    float64
 6   SibSp        891 non-null    int64  
 7   Parch        891 non-null    int64  
 8   Ticket       891 non-null    object 
 9   Fare         891 non-null    float64
 10  Cabin        204 non-null    object 
 11  Embarked     889 non-null    object 
dtypes: float64(2), int64(5), object(5)
memory usage: 83.7+ KB
```

The method [`info()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.info.html#pandas.DataFrame.info) provides technical information about a `DataFrame`, so let’s explain the output in more detail:

- It is indeed a `DataFrame`.
- There are 891 entries, i.e. 891 rows.
- Each row has a row label (aka the `index`) with values ranging from 0 to 890.
- The table has 12 columns. Most columns have a value for each of the rows (all 891 values are `non-null`). Some columns do have missing values and less than 891 `non-null` values.
- The columns `Name`, `Sex`, `Cabin` and `Embarked` consists of textual data (strings, aka `object`). The other columns are numerical data with some of them whole numbers (aka `integer`) and others are real numbers (aka `float`).
- The kind of data (characters, integers,…) in the different columns are summarized by listing the `dtypes`.
- The approximate amount of RAM used to hold the DataFrame is provided as well.

**REMEMBER**

- Getting data in to pandas from many different file formats or data sources is supported by `read_*` functions.
- Exporting data out of pandas is provided by different `to_*`methods.
- The `head`/`tail`/`info` methods and the `dtypes` attribute are convenient for a first check.

# 3.How do I select a subset of a DataFrame?

> This tutorial uses the Titanic data set, stored as CSV

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")

In [3]: titanic.head()
Out[3]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

## 3.1 How do I select specific columns from a `DataFrame`?

![](https://pandas.pydata.org/docs/_images/03_subset_columns.svg)

*I’m interested in the age of the Titanic passengers.*

```python
In [4]: ages = titanic["Age"]

In [5]: ages.head()
Out[5]: 
0    22.0
1    38.0
2    26.0
3    35.0
4    35.0
Name: Age, dtype: float64
```

To select a single column, use square brackets `[]` with the column name of the column of interest.

Each column in a [`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame) is a [`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series). As a single column is selected, the returned object is a pandas `Series`. We can verify this by checking the type of the output:

```python
In [6]: type(titanic["Age"])
Out[6]: pandas.core.series.Series
```

And have a look at the `shape` of the output:

```python
In [7]: titanic["Age"].shape
Out[7]: (891,)
```

[`DataFrame.shape`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.shape.html#pandas.DataFrame.shape) is an attribute (remember [tutorial on reading and writing](https://pandas.pydata.org/docs/getting_started/intro_tutorials/02_read_write.html#min-tut-02-read-write), do not use parentheses for attributes) of a pandas `Series` and `DataFrame` containing the number of rows and columns: (nrows, ncolumns). A pandas Series is 1-dimensional and only the number of rows is returned.

*I’m interested in the age and sex of the Titanic passengers.*

```python
In [8]: age_sex = titanic[["Age", "Sex"]]

In [9]: age_sex.head()
Out[9]: 
    Age     Sex
0  22.0    male
1  38.0  female
2  26.0  female
3  35.0  female
4  35.0    male
```

To select multiple columns, use a list of column names within the selection brackets `[]`.

> The inner square brackets define a [Python list](https://docs.python.org/3/tutorial/datastructures.html#tut-morelists) with column names, whereas the outer brackets are used to select the data from a pandas `DataFrame` as seen in the previous example.

The returned data type is a pandas DataFrame:

```python
In [10]: type(titanic[["Age", "Sex"]])
Out[10]: pandas.core.frame.DataFrame

In [11]: titanic[["Age", "Sex"]].shape
Out[11]: (891, 2)
```

The selection returned a `DataFrame` with 891 rows and 2 columns. Remember, a `DataFrame` is 2-dimensional with both a row and column dimension.

> For basic information on indexing, see the user guide section on [indexing and selecting data](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-basics).

## 3.2 How do I filter specific rows from a DataFrame?

![](https://pandas.pydata.org/docs/_images/03_subset_rows.svg)

*I’m interested in the passengers older than 35 years.*

```python
In [12]: above_35 = titanic[titanic["Age"] > 35]

In [13]: above_35.head()
Out[13]: 
    PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
1             2         1       1  ...  71.2833   C85         C
6             7         0       1  ...  51.8625   E46         S
11           12         1       1  ...  26.5500  C103         S
13           14         0       3  ...  31.2750   NaN         S
15           16         1       2  ...  16.0000   NaN         S

[5 rows x 12 columns]
```

To select rows based on a conditional expression, use a condition inside the selection brackets `[]`.

The condition inside the selection brackets `titanic["Age"] > 35` checks for which rows the `Age` column has a value larger than 35:

```python
In [14]: titanic["Age"] > 35
Out[14]: 
0      False
1       True
2      False
3      False
4      False
       ...  
886    False
887    False
888    False
889    False
890    False
Name: Age, Length: 891, dtype: bool
```

The output of the conditional expression (`>`, but also `==`, `!=`, `<`, `<=`,… would work) is actually a pandas `Series` of boolean values (either `True` or `False`) with the same number of rows as the original `DataFrame`. Such a `Series` of boolean values can be used to filter the `DataFrame` by putting it in between the selection brackets `[]`. Only rows for which the value is `True` will be selected.

We know from before that the original Titanic `DataFrame` consists of 891 rows. Let’s have a look at the number of rows which satisfy the condition by checking the `shape` attribute of the resulting `DataFrame` `above_35`:

```python
In [15]: above_35.shape
Out[15]: (217, 12)
```

*I’m interested in the Titanic passengers from cabin class 2 and 3.*

```python
In [16]: class_23 = titanic[titanic["Pclass"].isin([2, 3])]

In [17]: class_23.head()
Out[17]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
2            3         1       3  ...   7.9250   NaN         S
4            5         0       3  ...   8.0500   NaN         S
5            6         0       3  ...   8.4583   NaN         Q
7            8         0       3  ...  21.0750   NaN         S

[5 rows x 12 columns]
```

Similar to the conditional expression, the [`isin()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.isin.html#pandas.Series.isin) conditional function returns a `True` for each row the values are in the provided list. To filter the rows based on such a function, use the conditional function inside the selection brackets `[]`. In this case, the condition inside the selection brackets `titanic["Pclass"].isin([2, 3])` checks for which rows the `Pclass` column is either 2 or 3.

The above is equivalent to filtering by rows for which the class is either 2 or 3 and combining the two statements with an `|` (or) operator:

```python
In [18]: class_23 = titanic[(titanic["Pclass"] == 2) | (titanic["Pclass"] == 3)]

In [19]: class_23.head()
Out[19]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
2            3         1       3  ...   7.9250   NaN         S
4            5         0       3  ...   8.0500   NaN         S
5            6         0       3  ...   8.4583   NaN         Q
7            8         0       3  ...  21.0750   NaN         S

[5 rows x 12 columns]
```

> When combining multiple conditional statements, each condition must be surrounded by parentheses `()`. Moreover, you can not use `or`/`and` but need to use the `or` operator `|` and the `and` operator `&`.

> See the dedicated section in the user guide about [boolean indexing](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-boolean) or about the [isin function](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-basics-indexing-isin).

*I want to work with passenger data for which the age is known.*

```python
In [20]: age_no_na = titanic[titanic["Age"].notna()]

In [21]: age_no_na.head()
Out[21]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

The [`notna()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.notna.html#pandas.Series.notna) conditional function returns a `True` for each row the values are not a `Null` value. As such, this can be combined with the selection brackets `[]` to filter the data table.

You might wonder what actually changed, as the first 5 lines are still the same values. One way to verify is to check if the shape has changed:

```python
In [22]: age_no_na.shape
Out[22]: (714, 12)
```

For more dedicated functions on missing values, see the user guide section about [handling missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html#missing-data).

## 3.3 How do I select specific rows and columns from a DataFrame?

![](https://pandas.pydata.org/docs/_images/03_subset_columns_rows.svg)

*I’m interested in the names of the passengers older than 35 years.*

```python
In [23]: adult_names = titanic.loc[titanic["Age"] > 35, "Name"]

In [24]: adult_names.head()
Out[24]: 
1     Cumings, Mrs. John Bradley (Florence Briggs Th...
6                               McCarthy, Mr. Timothy J
11                             Bonnell, Miss. Elizabeth
13                          Andersson, Mr. Anders Johan
15                     Hewlett, Mrs. (Mary D Kingcome) 
Name: Name, dtype: object
```

In this case, a subset of both rows and columns is made in one go and just using selection brackets `[]` is not sufficient anymore. The `loc`/`iloc` operators are required in front of the selection brackets `[]`. When using `loc`/`iloc`, the part before the comma is the rows you want, and the part after the comma is the columns you want to select.

When using the column names, row labels or a condition expression, use the `loc` operator in front of the selection brackets `[]`. For both the part before and after the comma, you can use a single label, a list of labels, a slice of labels, a conditional expression or a colon. Using a colon specifies you want to select all rows or columns.

*I’m interested in rows 10 till 25 and columns 3 to 5.*

```python
In [25]: titanic.iloc[9:25, 2:5]
Out[25]: 
    Pclass                                 Name     Sex
9        2  Nasser, Mrs. Nicholas (Adele Achem)  female
10       3      Sandstrom, Miss. Marguerite Rut  female
11       1             Bonnell, Miss. Elizabeth  female
12       3       Saundercock, Mr. William Henry    male
13       3          Andersson, Mr. Anders Johan    male
..     ...                                  ...     ...
20       2                 Fynney, Mr. Joseph J    male
21       2                Beesley, Mr. Lawrence    male
22       3          McGowan, Miss. Anna "Annie"  female
23       1         Sloper, Mr. William Thompson    male
24       3        Palsson, Miss. Torborg Danira  female

[16 rows x 3 columns]
```

Again, a subset of both rows and columns is made in one go and just using selection brackets `[]` is not sufficient anymore. When specifically interested in certain rows and/or columns based on their position in the table, use the `iloc` operator in front of the selection brackets `[]`.

When selecting specific rows and/or columns with `loc` or `iloc`, new values can be assigned to the selected data. For example, to assign the name `anonymous` to the first 3 elements of the fourth column:

```python
In [26]: titanic.iloc[0:3, 3] = "anonymous"

In [27]: titanic.head()
Out[27]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

> See the user guide section on [different choices for indexing](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-choice) to get more insight in the usage of `loc` and `iloc`.

**REMEMBER**

- When selecting subsets of data, square brackets `[]` are used.
- Inside these brackets, you can use a single column/row label, a list of column/row labels, a slice of labels, a conditional expression or a colon.
- Select specific rows and/or columns using `loc` when using the row and column names.
- Select specific rows and/or columns using `iloc` when using the positions in the table.
- You can assign new values to a selection based on `loc`/`iloc`.

# 4.How do I create plots in pandas?

![](https://pandas.pydata.org/docs/_images/04_plot_overview.svg)

> For this tutorial, air quality data about is used `NO2`, made available by [OpenAQ](https://openaq.org/) and using the [py-openaq](http://dhhagan.github.io/py-openaq/index.html) package. The `air_quality_no2.csv` data set provides `NO2` values for the measurement stations FR04014, BETR801 and London Westminster in respectively Paris, Antwerp and London.

```python
In [1]: import pandas as pd

In [2]: import matplotlib.pyplot as plt

In [3]: air_quality = pd.read_csv("data/air_quality_no2.csv", index_col=0, parse_dates=True)

In [4]: air_quality.head()
Out[4]: 
                     station_antwerp  station_paris  station_london
datetime                                                           
2019-05-07 02:00:00              NaN            NaN            23.0
2019-05-07 03:00:00             50.5           25.0            19.0
2019-05-07 04:00:00             45.0           27.7            19.0
2019-05-07 05:00:00              NaN           50.4            16.0
2019-05-07 06:00:00              NaN           61.9             NaN
```

> The usage of the `index_col` and `parse_dates` parameters of the `read_csv` function to define the first (0th) column as index of the resulting `DataFrame` and convert the dates in the column to [`Timestamp`](https://pandas.pydata.org/docs/reference/api/pandas.Timestamp.html#pandas.Timestamp) objects, respectively.

*I want a quick visual check of the data.*

```python
In [5]: air_quality.plot()
Out[5]: <Axes: xlabel='datetime'>

In [6]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_quick.png)

With a `DataFrame`, pandas creates by default one line plot for each of the columns with numeric data.

*I want to plot only the columns of the data table with the data from Paris.*

```python
In [7]: air_quality["station_paris"].plot()
Out[7]: <Axes: xlabel='datetime'>

In [8]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_paris.png)

To plot a specific column, use the selection method of the [subset data tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset) in combination with the [`plot()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.html#pandas.DataFrame.plot) method. Hence, the `plot()` method works on both `Series` and `DataFrame`.

*I want to visually compare the `NO2` values measured in London versus Paris.*

```python
In [9]: air_quality.plot.scatter(x="station_london", y="station_paris", alpha=0.5)
Out[9]: <Axes: xlabel='station_london', ylabel='station_paris'>

In [10]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_scatter.png)

Apart from the default `line` plot when using the `plot` function, a number of alternatives are available to plot data. Let’s use some standard Python to get an overview of the available plot methods:

```python
In [11]: [
   ....:     method_name
   ....:     for method_name in dir(air_quality.plot)
   ....:     if not method_name.startswith("_")
   ....: ]
   ....: 
Out[11]: 
['area',
 'bar',
 'barh',
 'box',
 'density',
 'hexbin',
 'hist',
 'kde',
 'line',
 'pie',
 'scatter']
```

> In many development environments as well as IPython and Jupyter Notebook, use the TAB button to get an overview of the available methods, for example `air_quality.plot`. + TAB.

One of the options is [`DataFrame.plot.box()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.box.html#pandas.DataFrame.plot.box), which refers to a [boxplot](https://en.wikipedia.org/wiki/Box_plot). The `box` method is applicable on the air quality example data:

```python
In [12]: air_quality.plot.box()
Out[12]: <Axes: >

In [13]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_boxplot.png)

> For an introduction to plots other than the default line plot, see the user guide section about [supported plot styles](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-other).

*I want each of the columns in a separate subplot.*

```python
In [14]: axs = air_quality.plot.area(figsize=(12, 4), subplots=True)

In [15]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_area_subplot.png)

Separate subplots for each of the data columns are supported by the `subplots` argument of the `plot` functions. The builtin options available in each of the pandas plot functions are worth reviewing.

> Some more formatting options are explained in the user guide section on [plot formatting](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-formatting).

*I want to further customize, extend or save the resulting plot.*

```python
In [16]: fig, axs = plt.subplots(figsize=(12, 4))

In [17]: air_quality.plot.area(ax=axs)
Out[17]: <Axes: xlabel='datetime'>

In [18]: axs.set_ylabel("NO$_2$ concentration")
Out[18]: Text(0, 0.5, 'NO$_2$ concentration')

In [19]: fig.savefig("no2_concentrations.png")

In [20]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_customized.png)

Each of the plot objects created by pandas is a [Matplotlib](https://matplotlib.org/) object. As Matplotlib provides plenty of options to customize plots, making the link between pandas and Matplotlib explicit enables all the power of Matplotlib to the plot. This strategy is applied in the previous example:

```python
fig, axs = plt.subplots(figsize=(12, 4))        # Create an empty Matplotlib Figure and Axes
air_quality.plot.area(ax=axs)                   # Use pandas to put the area plot on the prepared Figure/Axes
axs.set_ylabel("NO$_2$ concentration")          # Do any Matplotlib customization you like
fig.savefig("no2_concentrations.png")           # Save the Figure/Axes using the existing Matplotlib method.
plt.show()                                      # Display the plot
```

**REMEMBER**
- The `.plot.*` methods are applicable on both Series and DataFrames.
- By default, each of the columns is plotted as a different element (line, boxplot,…).
- Any plot created by pandas is a Matplotlib object.

> A full overview of plotting in pandas is provided in the [visualization pages](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization).

# 5.How to create new columns derived from existing columns

> For this tutorial, air quality data about is used `NO2`, made available by [OpenAQ](https://openaq.org/) and using the [py-openaq](http://dhhagan.github.io/py-openaq/index.html) package. The `air_quality_no2.csv` data set provides `NO2` values for the measurement stations FR04014, BETR801 and London Westminster in respectively Paris, Antwerp and London.

```python
In [1]: import pandas as pd

In [2]: air_quality = pd.read_csv("data/air_quality_no2.csv", index_col=0, parse_dates=True)

In [3]: air_quality.head()
Out[3]: 
                     station_antwerp  station_paris  station_london
datetime                                                           
2019-05-07 02:00:00              NaN            NaN            23.0
2019-05-07 03:00:00             50.5           25.0            19.0
2019-05-07 04:00:00             45.0           27.7            19.0
2019-05-07 05:00:00              NaN           50.4            16.0
2019-05-07 06:00:00              NaN           61.9             NaN
```

![](https://pandas.pydata.org/docs/_images/05_newcolumn_1.svg)

*I want to express the `NO2` concentration of the station in London in mg/m<math xmlns="http://www.w3.org/1998/Math/MathML"><msup><mi></mi><mn>3</mn></msup></math>.*

(If we assume temperature of 25 degrees Celsius and pressure of 1013 hPa, the conversion factor is 1.882)

```python
In [4]: air_quality["london_mg_per_cubic"] = air_quality["station_london"] * 1.882

In [5]: air_quality.head()
Out[5]: 
                     station_antwerp  ...  london_mg_per_cubic
datetime                              ...                     
2019-05-07 02:00:00              NaN  ...               43.286
2019-05-07 03:00:00             50.5  ...               35.758
2019-05-07 04:00:00             45.0  ...               35.758
2019-05-07 05:00:00              NaN  ...               30.112
2019-05-07 06:00:00              NaN  ...                  NaN

[5 rows x 4 columns]
```

To create a new column, use the `[]` brackets with the new column name at the left side of the assignment.

> The calculation of the values is done **element-wise**. This means all values in the given column are multiplied by the value 1.882 at once. You do not need to use a loop to iterate each of the rows!

![](https://pandas.pydata.org/docs/_images/05_newcolumn_2.svg)

*I want to check the ratio of the values in Paris versus Antwerp and save the result in a new column.*

```python
In [6]: air_quality["ratio_paris_antwerp"] = (
   ...:     air_quality["station_paris"] / air_quality["station_antwerp"]
   ...: )
   ...: 

In [7]: air_quality.head()
Out[7]: 
                     station_antwerp  ...  ratio_paris_antwerp
datetime                              ...                     
2019-05-07 02:00:00              NaN  ...                  NaN
2019-05-07 03:00:00             50.5  ...             0.495050
2019-05-07 04:00:00             45.0  ...             0.615556
2019-05-07 05:00:00              NaN  ...                  NaN
2019-05-07 06:00:00              NaN  ...                  NaN

[5 rows x 5 columns]
```

he calculation is again element-wise, so the `/` is applied for the values in each row.

Also other mathematical operators (`+`, `-`, `*`, `/`,…) or logical operators (`<`, `>`, `==`,…) work element-wise. The latter was already used in the [subset data tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset) to filter rows of a table using a conditional expression.

If you need more advanced logic, you can use arbitrary Python code via [`apply()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html#pandas.DataFrame.apply).

*I want to rename the data columns to the corresponding station identifiers used by [OpenAQ](https://openaq.org/).*

```python
In [8]: air_quality_renamed = air_quality.rename(
   ...:     columns={
   ...:         "station_antwerp": "BETR801",
   ...:         "station_paris": "FR04014",
   ...:         "station_london": "London Westminster",
   ...:     }
   ...: )
   ...: 
```

```python
In [9]: air_quality_renamed.head()
Out[9]: 
                     BETR801  FR04014  ...  london_mg_per_cubic  ratio_paris_antwerp
datetime                               ...                                          
2019-05-07 02:00:00      NaN      NaN  ...               43.286                  NaN
2019-05-07 03:00:00     50.5     25.0  ...               35.758             0.495050
2019-05-07 04:00:00     45.0     27.7  ...               35.758             0.615556
2019-05-07 05:00:00      NaN     50.4  ...               30.112                  NaN
2019-05-07 06:00:00      NaN     61.9  ...                  NaN                  NaN

[5 rows x 5 columns]
```

The [`rename()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rename.html#pandas.DataFrame.rename) function can be used for both row labels and column labels. Provide a dictionary with the keys the current names and the values the new names to update the corresponding names.

The mapping should not be restricted to fixed names only, but can be a mapping function as well. For example, converting the column names to lowercase letters can be done using a function as well:

```python
In [10]: air_quality_renamed = air_quality_renamed.rename(columns=str.lower)

In [11]: air_quality_renamed.head()
Out[11]: 
                     betr801  fr04014  ...  london_mg_per_cubic  ratio_paris_antwerp
datetime                               ...                                          
2019-05-07 02:00:00      NaN      NaN  ...               43.286                  NaN
2019-05-07 03:00:00     50.5     25.0  ...               35.758             0.495050
2019-05-07 04:00:00     45.0     27.7  ...               35.758             0.615556
2019-05-07 05:00:00      NaN     50.4  ...               30.112                  NaN
2019-05-07 06:00:00      NaN     61.9  ...                  NaN                  NaN

[5 rows x 5 columns]
```

> Details about column or row label renaming is provided in the user guide section on [renaming labels](https://pandas.pydata.org/docs/user_guide/basics.html#basics-rename).

**REMEMBER**
- Create a new column by assigning the output to the DataFrame with a new column name in between the `[]`.
- Operations are element-wise, no need to loop over rows.
- Use `rename` with a dictionary or function to rename row labels or column names.

> The user guide contains a separate section on [column addition and deletion](https://pandas.pydata.org/docs/user_guide/dsintro.html#basics-dataframe-sel-add-del).

# 6.How to calculate summary statistics

> This tutorial uses the Titanic data set, stored as CSV.

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")

In [3]: titanic.head()
Out[3]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

## 6.1 Aggregating statistics

![](https://pandas.pydata.org/docs/_images/06_aggregate.svg)

*What is the average age of the Titanic passengers?*

```python
In [4]: titanic["Age"].mean()
Out[4]: 29.69911764705882
```

Different statistics are available and can be applied to columns with numerical data. Operations in general exclude missing data and operate across rows by default.

![](https://pandas.pydata.org/docs/_images/06_reduction.svg)

*What is the median age and ticket fare price of the Titanic passengers?*

```python
In [5]: titanic[["Age", "Fare"]].median()
Out[5]: 
Age     28.0000
Fare    14.4542
dtype: float64
```

The statistic applied to multiple columns of a `DataFrame` (the selection of two columns returns a `DataFrame`, see the [subset data tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset)) is calculated for each numeric column.

The aggregating statistic can be calculated for multiple columns at the same time. Remember the `describe` function from the [first tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html#min-tut-01-tableoriented)?

```python
In [6]: titanic[["Age", "Fare"]].describe()
Out[6]: 
              Age        Fare
count  714.000000  891.000000
mean    29.699118   32.204208
std     14.526497   49.693429
min      0.420000    0.000000
25%     20.125000    7.910400
50%     28.000000   14.454200
75%     38.000000   31.000000
max     80.000000  512.329200
```

Instead of the predefined statistics, specific combinations of aggregating statistics for given columns can be defined using the [`DataFrame.agg()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg) method:

```python
In [7]: titanic.agg(
   ...:     {
   ...:         "Age": ["min", "max", "median", "skew"],
   ...:         "Fare": ["min", "max", "median", "mean"],
   ...:     }
   ...: )
   ...: 
Out[7]: 
              Age        Fare
min      0.420000    0.000000
max     80.000000  512.329200
median  28.000000   14.454200
skew     0.389108         NaN
mean          NaN   32.204208
```

> Details about descriptive statistics are provided in the user guide section on [descriptive statistics](https://pandas.pydata.org/docs/user_guide/basics.html#basics-stats).

## 6.2 Aggregating statistics grouped by category

![](https://pandas.pydata.org/docs/_images/06_groupby.svg)

*What is the average age for male versus female Titanic passengers?*

```python
In [8]: titanic[["Sex", "Age"]].groupby("Sex").mean()
Out[8]: 
              Age
Sex              
female  27.915709
male    30.726645
```

As our interest is the average age for each gender, a subselection on these two columns is made first: `titanic[["Sex", "Age"]]`. Next, the [`groupby()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby) method is applied on the Sex column to make a group per category. The average age for each gender is calculated and returned.

Calculating a given statistic (e.g. `mean` age) for each category in a column (e.g. male/female in the `Sex` column) is a common pattern. The `groupby` method is used to support this type of operations. This fits in the more general `split-apply-combine` pattern:

- **Split** the data into groups
- **Apply** a function to each group independently
- **Combine** the results into a data structure

The apply and combine steps are typically done together in pandas.

In the previous example, we explicitly selected the 2 columns first. If not, the `mean` method is applied to each column containing numerical columns by passing `numeric_only=True`:

```python
In [9]: titanic.groupby("Sex").mean(numeric_only=True)
Out[9]: 
        PassengerId  Survived    Pclass  ...     SibSp     Parch       Fare
Sex                                      ...                               
female   431.028662  0.742038  2.159236  ...  0.694268  0.649682  44.479818
male     454.147314  0.188908  2.389948  ...  0.429809  0.235702  25.523893

[2 rows x 7 columns]
```

It does not make much sense to get the average value of the `Pclass`. If we are only interested in the average age for each gender, the selection of columns (rectangular brackets `[]` as usual) is supported on the grouped data as well:

```python
In [10]: titanic.groupby("Sex")["Age"].mean()
Out[10]: 
Sex
female    27.915709
male      30.726645
Name: Age, dtype: float64
```

![](https://pandas.pydata.org/docs/_images/06_groupby_select_detail.svg)

> The `Pclass` column contains numerical data but actually represents 3 categories (or factors) with respectively the labels ‘1’, ‘2’ and ‘3’. Calculating statistics on these does not make much sense. Therefore, pandas provides a `Categorical` data type to handle this type of data. More information is provided in the user guide [Categorical data](https://pandas.pydata.org/docs/user_guide/categorical.html#categorical) section.

*What is the mean ticket fare price for each of the sex and cabin class combinations?*

```python
In [11]: titanic.groupby(["Sex", "Pclass"])["Fare"].mean()
Out[11]: 
Sex     Pclass
female  1         106.125798
        2          21.970121
        3          16.118810
male    1          67.226127
        2          19.741782
        3          12.661633
Name: Fare, dtype: float64
```

Grouping can be done by multiple columns at the same time. Provide the column names as a list to the [`groupby()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby) method.

> A full description on the split-apply-combine approach is provided in the user guide section on [groupby operations](https://pandas.pydata.org/docs/user_guide/groupby.html#groupby).

## 6.3 Count number of records by category

![](https://pandas.pydata.org/docs/_images/06_valuecounts.svg)

*What is the number of passengers in each of the cabin classes?*

```python
In [12]: titanic["Pclass"].value_counts()
Out[12]: 
Pclass
3    491
1    216
2    184
Name: count, dtype: int64
```

The [`value_counts()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html#pandas.Series.value_counts) method counts the number of records for each category in a column.

The function is a shortcut, as it is actually a groupby operation in combination with counting of the number of records within each group:

```python
In [13]: titanic.groupby("Pclass")["Pclass"].count()
Out[13]: 
Pclass
1    216
2    184
3    491
Name: Pclass, dtype: int64
```

> Both `size` and `count` can be used in combination with `groupby`. Whereas `size` includes `NaN` values and just provides the number of rows (size of the table), `count` excludes the missing values. In the `value_counts` method, use the `dropna` argument to include or exclude the `NaN` values.

> The user guide has a dedicated section on `value_counts` , see the page on [discretization](https://pandas.pydata.org/docs/user_guide/basics.html#basics-discretization).

**REMEMBER**
- Aggregation statistics can be calculated on entire columns or rows.
- `groupby` provides the power of the split-apply-combine pattern.
- `value_counts` is a convenient shortcut to count the number of entries in each category of a variable.

> A full description on the split-apply-combine approach is provided in the user guide pages about [groupby operations](https://pandas.pydata.org/docs/user_guide/groupby.html#groupby).

# 7.How to reshape the layout of tables

> This tutorial uses the Titanic data set, stored as CSV.

> his tutorial uses air quality data about `NO2` and Particulate matter less than 2.5 micrometers, made available by OpenAQ and using the py-openaq package.

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")

In [3]: titanic.head()
Out[3]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]

In [4]: air_quality = pd.read_csv(
   ...:     "data/air_quality_long.csv", index_col="date.utc", parse_dates=True
   ...: )
   ...: 

In [5]: air_quality.head()
Out[5]: 
                                city country location parameter  value   unit
date.utc                                                                     
2019-06-18 06:00:00+00:00  Antwerpen      BE  BETR801      pm25   18.0  µg/m³
2019-06-17 08:00:00+00:00  Antwerpen      BE  BETR801      pm25    6.5  µg/m³
2019-06-17 07:00:00+00:00  Antwerpen      BE  BETR801      pm25   18.5  µg/m³
2019-06-17 06:00:00+00:00  Antwerpen      BE  BETR801      pm25   16.0  µg/m³
2019-06-17 05:00:00+00:00  Antwerpen      BE  BETR801      pm25    7.5  µg/m³
```

## 7.1 Sort table rows

*I want to sort the Titanic data according to the age of the passengers.*

```python
In [6]: titanic.sort_values(by="Age").head()
Out[6]: 
     PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
803          804         1       3  ...   8.5167   NaN         C
755          756         1       2  ...  14.5000   NaN         S
644          645         1       3  ...  19.2583   NaN         C
469          470         1       3  ...  19.2583   NaN         C
78            79         1       2  ...  29.0000   NaN         S

[5 rows x 12 columns]
```

*I want to sort the Titanic data according to the cabin class and age in descending order.*

```python
In [7]: titanic.sort_values(by=['Pclass', 'Age'], ascending=False).head()
Out[7]: 
     PassengerId  Survived  Pclass  ...    Fare Cabin  Embarked
851          852         0       3  ...  7.7750   NaN         S
116          117         0       3  ...  7.7500   NaN         Q
280          281         0       3  ...  7.7500   NaN         Q
483          484         1       3  ...  9.5875   NaN         S
326          327         0       3  ...  6.2375   NaN         S

[5 rows x 12 columns]
```

With [`DataFrame.sort_values()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html#pandas.DataFrame.sort_values), the rows in the table are sorted according to the defined column(s). The index will follow the row order.

> More details about sorting of tables is provided in the user guide section on [sorting data](https://pandas.pydata.org/docs/user_guide/basics.html#basics-sorting).

## 7.2 Long to wide table format

Let’s use a small subset of the air quality data set. We focus on `NO2` data and only use the first two measurements of each location (i.e. the head of each group). The subset of data will be called `no2_subset`.

```python
# filter for no2 data only
In [8]: no2 = air_quality[air_quality["parameter"] == "no2"]
```

```python
# use 2 measurements (head) for each location (groupby)
In [9]: no2_subset = no2.sort_index().groupby(["location"]).head(2)

In [10]: no2_subset
Out[10]: 
                                city country  ... value   unit
date.utc                                      ...             
2019-04-09 01:00:00+00:00  Antwerpen      BE  ...  22.5  µg/m³
2019-04-09 01:00:00+00:00      Paris      FR  ...  24.4  µg/m³
2019-04-09 02:00:00+00:00     London      GB  ...  67.0  µg/m³
2019-04-09 02:00:00+00:00  Antwerpen      BE  ...  53.5  µg/m³
2019-04-09 02:00:00+00:00      Paris      FR  ...  27.4  µg/m³
2019-04-09 03:00:00+00:00     London      GB  ...  67.0  µg/m³

[6 rows x 6 columns]
```

![](https://pandas.pydata.org/docs/_images/07_pivot.svg)

*I want the values for the three stations as separate columns next to each other.*

```python
In [11]: no2_subset.pivot(columns="location", values="value")
Out[11]: 
location                   BETR801  FR04014  London Westminster
date.utc                                                       
2019-04-09 01:00:00+00:00     22.5     24.4                 NaN
2019-04-09 02:00:00+00:00     53.5     27.4                67.0
2019-04-09 03:00:00+00:00      NaN      NaN                67.0
```

The [`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.pivot.html#pandas.pivot) function is purely reshaping of the data: a single value for each index/column combination is required.

As pandas supports plotting of multiple columns (see [plotting tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html#min-tut-04-plotting)) out of the box, the conversion from long to wide table format enables the plotting of the different time series at the same time:

```python
In [12]: no2.head()
Out[12]: 
                            city country location parameter  value   unit
date.utc                                                                 
2019-06-21 00:00:00+00:00  Paris      FR  FR04014       no2   20.0  µg/m³
2019-06-20 23:00:00+00:00  Paris      FR  FR04014       no2   21.8  µg/m³
2019-06-20 22:00:00+00:00  Paris      FR  FR04014       no2   26.5  µg/m³
2019-06-20 21:00:00+00:00  Paris      FR  FR04014       no2   24.9  µg/m³
2019-06-20 20:00:00+00:00  Paris      FR  FR04014       no2   21.4  µg/m³
```

```python
In [13]: no2.pivot(columns="location", values="value").plot()
Out[13]: <Axes: xlabel='date.utc'>
```

![](https://pandas.pydata.org/docs/_images/7_reshape_columns.png)

> When the `index` parameter is not defined, the existing index (row labels) is used.

> For more information about [`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot.html#pandas.DataFrame.pivot), see the user guide section on [pivoting DataFrame objects](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-reshaping).

![](https://pandas.pydata.org/docs/_images/07_pivot_table.svg)

*I want the mean concentrations for `NO2` and `PM2.5` in each of the stations in table form.*

```python
In [14]: air_quality.pivot_table(
   ....:     values="value", index="location", columns="parameter", aggfunc="mean"
   ....: )
   ....: 
Out[14]: 
parameter                 no2       pm25
location                                
BETR801             26.950920  23.169492
FR04014             29.374284        NaN
London Westminster  29.740050  13.443568
```

In the case of [`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot.html#pandas.DataFrame.pivot), the data is only rearranged. When multiple values need to be aggregated (in this specific case, the values on different time steps), [`pivot_table()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot_table.html#pandas.DataFrame.pivot_table) can be used, providing an aggregation function (e.g. mean) on how to combine these values.

Pivot table is a well known concept in spreadsheet software. When interested in the row/column margins (subtotals) for each variable, set the `margins` parameter to `True`:

```python
In [15]: air_quality.pivot_table(
   ....:     values="value",
   ....:     index="location",
   ....:     columns="parameter",
   ....:     aggfunc="mean",
   ....:     margins=True,
   ....: )
   ....: 
Out[15]: 
parameter                 no2       pm25        All
location                                           
BETR801             26.950920  23.169492  24.982353
FR04014             29.374284        NaN  29.374284
London Westminster  29.740050  13.443568  21.491708
All                 29.430316  14.386849  24.222743
```

> For more information about [`pivot_table()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot_table.html#pandas.DataFrame.pivot_table), see the user guide section on [pivot tables](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-pivot).

In case you are wondering, `pivot_table()` is indeed directly linked to `groupby()`. The same result can be derived by grouping on both `parameter` and `location`:

```python
air_quality.groupby(["parameter", "location"])[["value"]].mean()
```

## 7.3 Wide to long format

Starting again from the wide format table created in the previous section, we add a new index to the `DataFrame` with [`reset_index()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.reset_index.html#pandas.DataFrame.reset_index).

```python
In [16]: no2_pivoted = no2.pivot(columns="location", values="value").reset_index()

In [17]: no2_pivoted.head()
Out[17]: 
location                  date.utc  BETR801  FR04014  London Westminster
0        2019-04-09 01:00:00+00:00     22.5     24.4                 NaN
1        2019-04-09 02:00:00+00:00     53.5     27.4                67.0
2        2019-04-09 03:00:00+00:00     54.5     34.2                67.0
3        2019-04-09 04:00:00+00:00     34.5     48.5                41.0
4        2019-04-09 05:00:00+00:00     46.5     59.5                41.0
```

![](https://pandas.pydata.org/docs/_images/07_melt.svg)

*I want to collect all air quality `NO2` measurements in a single column (long format).*

```python
In [18]: no_2 = no2_pivoted.melt(id_vars="date.utc")

In [19]: no_2.head()
Out[19]: 
                   date.utc location  value
0 2019-04-09 01:00:00+00:00  BETR801   22.5
1 2019-04-09 02:00:00+00:00  BETR801   53.5
2 2019-04-09 03:00:00+00:00  BETR801   54.5
3 2019-04-09 04:00:00+00:00  BETR801   34.5
4 2019-04-09 05:00:00+00:00  BETR801   46.5
```

The [`pandas.melt()`](https://pandas.pydata.org/docs/reference/api/pandas.melt.html#pandas.melt) method on a `DataFrame` converts the data table from wide format to long format. The column headers become the variable names in a newly created column.

The solution is the short version on how to apply `pandas.melt()`. The method will melt all columns NOT mentioned in `id_vars` together into two columns: A column with the column header names and a column with the values itself. The latter column gets by default the name `value`.

The parameters passed to `pandas.melt()` can be defined in more detail:

```python
In [20]: no_2 = no2_pivoted.melt(
   ....:     id_vars="date.utc",
   ....:     value_vars=["BETR801", "FR04014", "London Westminster"],
   ....:     value_name="NO_2",
   ....:     var_name="id_location",
   ....: )
   ....: 

In [21]: no_2.head()
Out[21]: 
                   date.utc id_location  NO_2
0 2019-04-09 01:00:00+00:00     BETR801  22.5
1 2019-04-09 02:00:00+00:00     BETR801  53.5
2 2019-04-09 03:00:00+00:00     BETR801  54.5
3 2019-04-09 04:00:00+00:00     BETR801  34.5
4 2019-04-09 05:00:00+00:00     BETR801  46.5
```

The additional parameters have the following effects:

- `value_vars` defines which columns to melt together
- `value_name` provides a custom column name for the values column instead of the default column name `value`
- `var_name` provides a custom column name for the column collecting the column header names. Otherwise it takes the index name or a default `variable`

Hence, the arguments `value_name` and `var_name` are just user-defined names for the two generated columns. The columns to melt are defined by `id_vars` and `value_vars`.

> Conversion from wide to long format with `pandas.melt()` is explained in the user guide section on [reshaping by melt](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-melt).

**REMEMBER**
- Sorting by one or more columns is supported by `sort_values`.
- The `pivot` function is purely restructuring of the data, `pivot_table` supports aggregations.
- The reverse of `pivot` (long to wide format) is `melt` (wide to long format).

> A full overview is available in the user guide on the pages about [reshaping and pivoting](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping).

# 8.How to combine data from multiple tables

> For this tutorial, air quality data about `NO2` is used, made available by OpenAQ and downloaded using the py-openaq package.

> For this tutorial, air quality data about Particulate matter less than 2.5 micrometers is used, made available by OpenAQ and downloaded using the py-openaq package.

```python
In [1]: import pandas as pd

In [2]: air_quality_no2 = pd.read_csv("data/air_quality_no2_long.csv",
   ...:                               parse_dates=True)
   ...: 

In [3]: air_quality_no2 = air_quality_no2[["date.utc", "location",
   ...:                                    "parameter", "value"]]
   ...: 

In [4]: air_quality_no2.head()
Out[4]: 
                    date.utc location parameter  value
0  2019-06-21 00:00:00+00:00  FR04014       no2   20.0
1  2019-06-20 23:00:00+00:00  FR04014       no2   21.8
2  2019-06-20 22:00:00+00:00  FR04014       no2   26.5
3  2019-06-20 21:00:00+00:00  FR04014       no2   24.9
4  2019-06-20 20:00:00+00:00  FR04014       no2   21.4

In [5]: air_quality_pm25 = pd.read_csv("data/air_quality_pm25_long.csv",
   ...:                                parse_dates=True)
   ...: 

In [6]: air_quality_pm25 = air_quality_pm25[["date.utc", "location",
   ...:                                      "parameter", "value"]]
   ...: 

In [7]: air_quality_pm25.head()
Out[7]: 
                    date.utc location parameter  value
0  2019-06-18 06:00:00+00:00  BETR801      pm25   18.0
1  2019-06-17 08:00:00+00:00  BETR801      pm25    6.5
2  2019-06-17 07:00:00+00:00  BETR801      pm25   18.5
3  2019-06-17 06:00:00+00:00  BETR801      pm25   16.0
4  2019-06-17 05:00:00+00:00  BETR801      pm25    7.5
```

## 8.1 Concatenating objects

![](https://pandas.pydata.org/docs/_images/08_concat_row.svg)

*I want to combine the measurements of `NO2` and `PM25`, two tables with a similar structure, in a single table.*

```python
In [8]: air_quality = pd.concat([air_quality_pm25, air_quality_no2], axis=0)

In [9]: air_quality.head()
Out[9]: 
                    date.utc location parameter  value
0  2019-06-18 06:00:00+00:00  BETR801      pm25   18.0
1  2019-06-17 08:00:00+00:00  BETR801      pm25    6.5
2  2019-06-17 07:00:00+00:00  BETR801      pm25   18.5
3  2019-06-17 06:00:00+00:00  BETR801      pm25   16.0
4  2019-06-17 05:00:00+00:00  BETR801      pm25    7.5
```

The [`concat()`](https://pandas.pydata.org/docs/reference/api/pandas.concat.html#pandas.concat) function performs concatenation operations of multiple tables along one of the axes (row-wise or column-wise).

By default concatenation is along axis 0, so the resulting table combines the rows of the input tables. Let’s check the shape of the original and the concatenated tables to verify the operation:

```python
In [10]: print('Shape of the ``air_quality_pm25`` table: ', air_quality_pm25.shape)
Shape of the ``air_quality_pm25`` table:  (1110, 4)

In [11]: print('Shape of the ``air_quality_no2`` table: ', air_quality_no2.shape)
Shape of the ``air_quality_no2`` table:  (2068, 4)

In [12]: print('Shape of the resulting ``air_quality`` table: ', air_quality.shape)
Shape of the resulting ``air_quality`` table:  (3178, 4)
```

Hence, the resulting table has 3178 = 1110 + 2068 rows.

> The **axis** argument will return in a number of pandas methods that can be applied **along an axis**. A `DataFrame` has two corresponding axes: the first running vertically downwards across rows (axis 0), and the second running horizontally across columns (axis 1). Most operations like concatenation or summary statistics are by default across rows (axis 0), but can be applied across columns as well.

Sorting the table on the datetime information illustrates also the combination of both tables, with the `parameter` column defining the origin of the table (either `no2` from table `air_quality_no2` or `pm25` from table `air_quality_pm25`):

```python
In [13]: air_quality = air_quality.sort_values("date.utc")

In [14]: air_quality.head()
Out[14]: 
                       date.utc            location parameter  value
2067  2019-05-07 01:00:00+00:00  London Westminster       no2   23.0
1003  2019-05-07 01:00:00+00:00             FR04014       no2   25.0
100   2019-05-07 01:00:00+00:00             BETR801      pm25   12.5
1098  2019-05-07 01:00:00+00:00             BETR801       no2   50.5
1109  2019-05-07 01:00:00+00:00  London Westminster      pm25    8.0
```

In this specific example, the `parameter` column provided by the data ensures that each of the original tables can be identified. This is not always the case. The `concat` function provides a convenient solution with the `keys` argument, adding an additional (hierarchical) row index. For example:

```python
In [15]: air_quality_ = pd.concat([air_quality_pm25, air_quality_no2], keys=["PM25", "NO2"])

In [16]: air_quality_.head()
Out[16]: 
                         date.utc location parameter  value
PM25 0  2019-06-18 06:00:00+00:00  BETR801      pm25   18.0
     1  2019-06-17 08:00:00+00:00  BETR801      pm25    6.5
     2  2019-06-17 07:00:00+00:00  BETR801      pm25   18.5
     3  2019-06-17 06:00:00+00:00  BETR801      pm25   16.0
     4  2019-06-17 05:00:00+00:00  BETR801      pm25    7.5
```

> The existence of multiple row/column indices at the same time has not been mentioned within these tutorials. *Hierarchical indexing* or *MultiIndex* is an advanced and powerful pandas feature to analyze higher dimensional data.

> Multi-indexing is out of scope for this pandas introduction. For the moment, remember that the function `reset_index` can be used to convert any level of an index to a column, e.g. `air_quality.reset_index(level=0)`

> Feel free to dive into the world of multi-indexing at the user guide section on [advanced indexing](https://pandas.pydata.org/docs/user_guide/advanced.html#advanced).

> More options on table concatenation (row and column wise) and how `concat` can be used to define the logic (union or intersection) of the indexes on the other axes is provided at the section on [object concatenation](https://pandas.pydata.org/docs/user_guide/merging.html#merging-concat).

## 8.2 Join tables using a common identifier

![](https://pandas.pydata.org/docs/_images/08_merge_left.svg)

*Add the station coordinates, provided by the stations metadata table, to the corresponding rows in the measurements table.*

> The air quality measurement station coordinates are stored in a data file `air_quality_stations.csv`, downloaded using the [py-openaq](http://dhhagan.github.io/py-openaq/index.html) package.

```python
In [17]: stations_coord = pd.read_csv("data/air_quality_stations.csv")

In [18]: stations_coord.head()
Out[18]: 
  location  coordinates.latitude  coordinates.longitude
0  BELAL01              51.23619                4.38522
1  BELHB23              51.17030                4.34100
2  BELLD01              51.10998                5.00486
3  BELLD02              51.12038                5.02155
4  BELR833              51.32766                4.36226
```

The stations used in this example (FR04014, BETR801 and London Westminster) are just three entries enlisted in the metadata table. We only want to add the coordinates of these three to the measurements table, each on the corresponding rows of the `air_quality` table.

```python
In [19]: air_quality.head()
Out[19]: 
                       date.utc            location parameter  value
2067  2019-05-07 01:00:00+00:00  London Westminster       no2   23.0
1003  2019-05-07 01:00:00+00:00             FR04014       no2   25.0
100   2019-05-07 01:00:00+00:00             BETR801      pm25   12.5
1098  2019-05-07 01:00:00+00:00             BETR801       no2   50.5
1109  2019-05-07 01:00:00+00:00  London Westminster      pm25    8.0
```

```python
In [20]: air_quality = pd.merge(air_quality, stations_coord, how="left", on="location")

In [21]: air_quality.head()
Out[21]: 
                    date.utc  ... coordinates.longitude
0  2019-05-07 01:00:00+00:00  ...              -0.13193
1  2019-05-07 01:00:00+00:00  ...               2.39390
2  2019-05-07 01:00:00+00:00  ...               2.39390
3  2019-05-07 01:00:00+00:00  ...               4.43182
4  2019-05-07 01:00:00+00:00  ...               4.43182

[5 rows x 6 columns]
```

Using the [`merge()`](https://pandas.pydata.org/docs/reference/api/pandas.merge.html#pandas.merge) function, for each of the rows in the `air_quality` table, the corresponding coordinates are added from the `air_quality_stations_coord` table. Both tables have the column `location` in common which is used as a key to combine the information. By choosing the `left` join, only the locations available in the `air_quality` (left) table, i.e. FR04014, BETR801 and London Westminster, end up in the resulting table. The `merge` function supports multiple join options similar to database-style operations.

*Add the parameters’ full description and name, provided by the parameters metadata table, to the measurements table.*

> The air quality parameters metadata are stored in a data file `air_quality_parameters.csv`, downloaded using the [py-openaq](http://dhhagan.github.io/py-openaq/index.html) package.

```python
In [22]: air_quality_parameters = pd.read_csv("data/air_quality_parameters.csv")

In [23]: air_quality_parameters.head()
Out[23]: 
     id                                        description  name
0    bc                                       Black Carbon    BC
1    co                                    Carbon Monoxide    CO
2   no2                                   Nitrogen Dioxide   NO2
3    o3                                              Ozone    O3
4  pm10  Particulate matter less than 10 micrometers in...  PM10
```

```python
In [24]: air_quality = pd.merge(air_quality, air_quality_parameters,
   ....:                        how='left', left_on='parameter', right_on='id')
   ....: 

In [25]: air_quality.head()
Out[25]: 
                    date.utc  ...   name
0  2019-05-07 01:00:00+00:00  ...    NO2
1  2019-05-07 01:00:00+00:00  ...    NO2
2  2019-05-07 01:00:00+00:00  ...    NO2
3  2019-05-07 01:00:00+00:00  ...  PM2.5
4  2019-05-07 01:00:00+00:00  ...    NO2

[5 rows x 9 columns]
```

Compared to the previous example, there is no common column name. However, the `parameter` column in the `air_quality` table and the `id` column in the `air_quality_parameters_name` both provide the measured variable in a common format. The `left_on` and `right_on` arguments are used here (instead of just `on`) to make the link between the two tables.

> pandas supports also inner, outer, and right joins. More information on join/merge of tables is provided in the user guide section on [database style merging of tables](https://pandas.pydata.org/docs/user_guide/merging.html#merging-join). Or have a look at the [comparison with SQL](https://pandas.pydata.org/docs/getting_started/comparison/comparison_with_sql.html#compare-with-sql-join) page.

**REMEMBER**
- Multiple tables can be concatenated both column-wise and row-wise using the `concat` function.
- For database-like merging/joining of tables, use the merge function.

> See the user guide for a full description of the various [facilities to combine data tables](https://pandas.pydata.org/docs/user_guide/merging.html#merging).

# 9.How to handle time series data with ease

> For this tutorial, air quality data about `NO2` and Particulate matter less than 2.5 micrometers is used, made available by [OpenAQ](https://openaq.org/) and downloaded using the [py-openaq](http://dhhagan.github.io/py-openaq/index.html) package.

```python
In [1]: import pandas as pd

In [2]: import matplotlib.pyplot as plt

In [3]: air_quality = pd.read_csv("data/air_quality_no2_long.csv")

In [4]: air_quality = air_quality.rename(columns={"date.utc": "datetime"})

In [5]: air_quality.head()
Out[5]: 
    city country                   datetime location parameter  value   unit
0  Paris      FR  2019-06-21 00:00:00+00:00  FR04014       no2   20.0  µg/m³
1  Paris      FR  2019-06-20 23:00:00+00:00  FR04014       no2   21.8  µg/m³
2  Paris      FR  2019-06-20 22:00:00+00:00  FR04014       no2   26.5  µg/m³
3  Paris      FR  2019-06-20 21:00:00+00:00  FR04014       no2   24.9  µg/m³
4  Paris      FR  2019-06-20 20:00:00+00:00  FR04014       no2   21.4  µg/m³

In [6]: air_quality.city.unique()
Out[6]: array(['Paris', 'Antwerpen', 'London'], dtype=object)
```

## 9.1 Using pandas datetime properties

*I want to work with the dates in the column datetime as datetime objects instead of plain text*

```python
In [7]: air_quality["datetime"] = pd.to_datetime(air_quality["datetime"])

In [8]: air_quality["datetime"]
Out[8]: 
0      2019-06-21 00:00:00+00:00
1      2019-06-20 23:00:00+00:00
2      2019-06-20 22:00:00+00:00
3      2019-06-20 21:00:00+00:00
4      2019-06-20 20:00:00+00:00
                  ...           
2063   2019-05-07 06:00:00+00:00
2064   2019-05-07 04:00:00+00:00
2065   2019-05-07 03:00:00+00:00
2066   2019-05-07 02:00:00+00:00
2067   2019-05-07 01:00:00+00:00
Name: datetime, Length: 2068, dtype: datetime64[ns, UTC]
```

Initially, the values in `datetime` are character strings and do not provide any datetime operations (e.g. extract the year, day of the week,…). By applying the `to_datetime` function, pandas interprets the strings and convert these to datetime (i.e. `datetime64[ns, UTC]`) objects. In pandas we call these datetime objects similar to `datetime.datetime` from the standard library as [pandas.Timestamp](https://pandas.pydata.org/docs/reference/api/pandas.Timestamp.html#pandas.Timestamp).

> As many data sets do contain datetime information in one of the columns, pandas input function like [`pandas.read_csv()`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv) and [`pandas.read_json()`](https://pandas.pydata.org/docs/reference/api/pandas.read_json.html#pandas.read_json) can do the transformation to dates when reading the data using the `parse_dates` parameter with a list of the columns to read as Timestamp:

```python
pd.read_csv("../data/air_quality_no2_long.csv", parse_dates=["datetime"])
```

Why are these [`pandas.Timestamp`](https://pandas.pydata.org/docs/reference/api/pandas.Timestamp.html#pandas.Timestamp) objects useful? Let’s illustrate the added value with some example cases.

What is the start and end date of the time series data set we are working with?

```python
In [9]: air_quality["datetime"].min(), air_quality["datetime"].max()
Out[9]: 
(Timestamp('2019-05-07 01:00:00+0000', tz='UTC'),
 Timestamp('2019-06-21 00:00:00+0000', tz='UTC'))
```

Using `pandas.Timestamp` for datetimes enables us to calculate with date information and make them comparable. Hence, we can use this to get the length of our time series:

```python
In [10]: air_quality["datetime"].max() - air_quality["datetime"].min()
Out[10]: Timedelta('44 days 23:00:00')
```

The result is a [`pandas.Timedelta`](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html#pandas.Timedelta) object, similar to `datetime.timedelta` from the standard Python library and defining a time duration.

> The various time concepts supported by pandas are explained in the user guide section on [time related concepts](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-overview).

*I want to add a new column to the `DataFrame` containing only the month of the measurement*

```python
In [11]: air_quality["month"] = air_quality["datetime"].dt.month

In [12]: air_quality.head()
Out[12]: 
    city country                  datetime  ... value   unit  month
0  Paris      FR 2019-06-21 00:00:00+00:00  ...  20.0  µg/m³      6
1  Paris      FR 2019-06-20 23:00:00+00:00  ...  21.8  µg/m³      6
2  Paris      FR 2019-06-20 22:00:00+00:00  ...  26.5  µg/m³      6
3  Paris      FR 2019-06-20 21:00:00+00:00  ...  24.9  µg/m³      6
4  Paris      FR 2019-06-20 20:00:00+00:00  ...  21.4  µg/m³      6

[5 rows x 8 columns]
```

By using `Timestamp` objects for dates, a lot of time-related properties are provided by pandas. For example the `month`, but also `year`, `quarter`,… All of these properties are accessible by the dt accessor.

> An overview of the existing date properties is given in the [time and date components overview table](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-components). More details about the `dt` accessor to return datetime like properties are explained in a dedicated section on the [dt accessor](https://pandas.pydata.org/docs/user_guide/basics.html#basics-dt-accessors).

*What is the average `NO2` concentration for each day of the week for each of the measurement locations?*

```python
In [13]: air_quality.groupby(
   ....:     [air_quality["datetime"].dt.weekday, "location"])["value"].mean()
   ....: 
Out[13]: 
datetime  location          
0         BETR801               27.875000
          FR04014               24.856250
          London Westminster    23.969697
1         BETR801               22.214286
          FR04014               30.999359
                                  ...    
5         FR04014               25.266154
          London Westminster    24.977612
6         BETR801               21.896552
          FR04014               23.274306
          London Westminster    24.859155
Name: value, Length: 21, dtype: float64
```

Remember the split-apply-combine pattern provided by `groupby` from the [tutorial on statistics calculation?](https://pandas.pydata.org/docs/getting_started/intro_tutorials/06_calculate_statistics.html#min-tut-06-stats) Here, we want to calculate a given statistic (e.g. mean `NO2`) **for each weekday** and **for each measurement location**. To group on weekdays, we use the datetime property weekday (with Monday=0 and Sunday=6) of pandas `Timestamp`, which is also accessible by the dt accessor. The grouping on both locations and weekdays can be done to split the calculation of the mean on each of these combinations.

> As we are working with a very short time series in these examples, the analysis does not provide a long-term representative result!

*Plot the typical `NO2` pattern during the day of our time series of all stations together. In other words, what is the average value for each hour of the day?*

```python
In [14]: fig, axs = plt.subplots(figsize=(12, 4))

In [15]: air_quality.groupby(air_quality["datetime"].dt.hour)["value"].mean().plot(
   ....:     kind='bar', rot=0, ax=axs
   ....: )
   ....: 
Out[15]: <Axes: xlabel='datetime'>

In [16]: plt.xlabel("Hour of the day");  # custom x label using Matplotlib

In [17]: plt.ylabel("$NO_2 (µg/m^3)$");
```

![](https://pandas.pydata.org/docs/_images/09_bar_chart.png)

Similar to the previous case, we want to calculate a given statistic (e.g. mean `NO2`) **for each hour of the day** and we can use the split-apply-combine approach again. For this case, we use the datetime property `hour` of pandas `Timestamp`, which is also accessible by the `dt` accessor.

## 9.2 Datetime as index

In the [tutorial on reshaping](https://pandas.pydata.org/docs/getting_started/intro_tutorials/07_reshape_table_layout.html#min-tut-07-reshape), [`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.pivot.html#pandas.pivot) was introduced to reshape the data table with each of the measurements locations as a separate column:

```python
In [18]: no_2 = air_quality.pivot(index="datetime", columns="location", values="value")

In [19]: no_2.head()
Out[19]: 
location                   BETR801  FR04014  London Westminster
datetime                                                       
2019-05-07 01:00:00+00:00     50.5     25.0                23.0
2019-05-07 02:00:00+00:00     45.0     27.7                19.0
2019-05-07 03:00:00+00:00      NaN     50.4                19.0
2019-05-07 04:00:00+00:00      NaN     61.9                16.0
2019-05-07 05:00:00+00:00      NaN     72.4                 NaN
```

> By pivoting the data, the datetime information became the index of the table. In general, setting a column as an index can be achieved by the `set_index` function.

Working with a datetime index (i.e. `DatetimeIndex`) provides powerful functionalities. For example, we do not need the `dt` accessor to get the time series properties, but have these properties available on the index directly:

```python
In [20]: no_2.index.year, no_2.index.weekday
Out[20]: 
(Index([2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019,
        ...
        2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019],
       dtype='int32', name='datetime', length=1033),
 Index([1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
        ...
        3, 3, 3, 3, 3, 3, 3, 3, 3, 4],
       dtype='int32', name='datetime', length=1033))
```

Some other advantages are the convenient subsetting of time period or the adapted time scale on plots. Let’s apply this on our data.

*Create a plot of the `NO2` values in the different stations from the 20th of May till the end of 21st of May*

```python
In [21]: no_2["2019-05-20":"2019-05-21"].plot();
```

![](https://pandas.pydata.org/docs/_images/09_time_section.png)

By providing a **string that parses to a datetime**, a specific subset of the data can be selected on a `DatetimeIndex`.

> More information on the `DatetimeIndex` and the slicing by using strings is provided in the section on [time series indexing](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-datetimeindex).

## 9.3 Resample a time series to another frequency

*Aggregate the current hourly time series values to the monthly maximum value in each of the stations.*

```python
In [22]: monthly_max = no_2.resample("ME").max()

In [23]: monthly_max
Out[23]: 
location                   BETR801  FR04014  London Westminster
datetime                                                       
2019-05-31 00:00:00+00:00     74.5     97.0                97.0
2019-06-30 00:00:00+00:00     52.5     84.7                52.0
```

A very powerful method on time series data with a datetime index, is the ability to [`resample()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.resample.html#pandas.Series.resample) time series to another frequency (e.g., converting secondly data into 5-minutely data).

The [`resample()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.resample.html#pandas.Series.resample) method is similar to a groupby operation:

- it provides a time-based grouping, by using a string (e.g. `M`, `5H`,…) that defines the target frequency
- it requires an aggregation function such as `mean`, `max`,…

> An overview of the aliases used to define time series frequencies is given in the [offset aliases overview table](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-offset-aliases).

When defined, the frequency of the time series is provided by the `freq` attribute:

```python
In [24]: monthly_max.index.freq
Out[24]: <MonthEnd>
```

*Make a plot of the daily mean `NO2` value in each of the stations.*

```python
In [25]: no_2.resample("D").mean().plot(style="-o", figsize=(10, 5));
```

![](https://pandas.pydata.org/docs/_images/09_resample_mean.png)

> More details on the power of time series `resampling` is provided in the user guide section on [resampling](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-resampling).

**REMEMBER**
- Valid date strings can be converted to datetime objects using `to_datetime` function or as part of read functions.
- Datetime objects in pandas support calculations, logical operations and convenient date-related properties using the `dt` accessor.
- A `DatetimeIndex` contains these date-related properties and supports convenient slicing.
- `Resample` is a powerful method to change the frequency of a time series.

> A full overview on time series is given on the pages on [time series and date functionality](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries).

# 10.How to manipulate textual data

> This tutorial uses the Titanic data set, stored as CSV.

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")

In [3]: titanic.head()
Out[3]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

*Make all name characters lowercase.*

```python
In [4]: titanic["Name"].str.lower()
Out[4]: 
0                                braund, mr. owen harris
1      cumings, mrs. john bradley (florence briggs th...
2                                 heikkinen, miss. laina
3           futrelle, mrs. jacques heath (lily may peel)
4                               allen, mr. william henry
                             ...                        
886                                montvila, rev. juozas
887                         graham, miss. margaret edith
888             johnston, miss. catherine helen "carrie"
889                                behr, mr. karl howell
890                                  dooley, mr. patrick
Name: Name, Length: 891, dtype: object
```

To make each of the strings in the `Name` column lowercase, select the `Name` column (see the [tutorial on selection of data](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset)), add the `str` accessor and apply the `lower` method. As such, each of the strings is converted element-wise.

Similar to datetime objects in the [time series tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/09_timeseries.html#min-tut-09-timeseries) having a `dt` accessor, a number of specialized string methods are available when using the `str` accessor. These methods have in general matching names with the equivalent built-in string methods for single elements, but are applied element-wise (remember [element-wise calculations?](https://pandas.pydata.org/docs/getting_started/intro_tutorials/05_add_columns.html#min-tut-05-columns)) on each of the values of the columns.

*Create a new column `Surname` that contains the surname of the passengers by extracting the part before the comma.*

```python
In [5]: titanic["Name"].str.split(",")
Out[5]: 
0                             [Braund,  Mr. Owen Harris]
1      [Cumings,  Mrs. John Bradley (Florence Briggs ...
2                              [Heikkinen,  Miss. Laina]
3        [Futrelle,  Mrs. Jacques Heath (Lily May Peel)]
4                            [Allen,  Mr. William Henry]
                             ...                        
886                             [Montvila,  Rev. Juozas]
887                      [Graham,  Miss. Margaret Edith]
888          [Johnston,  Miss. Catherine Helen "Carrie"]
889                             [Behr,  Mr. Karl Howell]
890                               [Dooley,  Mr. Patrick]
Name: Name, Length: 891, dtype: object
```

Using the [`Series.str.split()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.split.html#pandas.Series.str.split) method, each of the values is returned as a list of 2 elements. The first element is the part before the comma and the second element is the part after the comma.

```python
In [6]: titanic["Surname"] = titanic["Name"].str.split(",").str.get(0)

In [7]: titanic["Surname"]
Out[7]: 
0         Braund
1        Cumings
2      Heikkinen
3       Futrelle
4          Allen
         ...    
886     Montvila
887       Graham
888     Johnston
889         Behr
890       Dooley
Name: Surname, Length: 891, dtype: object
```

As we are only interested in the first part representing the surname (element 0), we can again use the `str` accessor and apply [`Series.str.get()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.get.html#pandas.Series.str.get) to extract the relevant part. Indeed, these string functions can be concatenated to combine multiple functions at once!

> More information on extracting parts of strings is available in the user guide section on [splitting and replacing strings](https://pandas.pydata.org/docs/user_guide/text.html#text-split).

*Extract the passenger data about the countesses on board of the Titanic.*

```python
In [8]: titanic["Name"].str.contains("Countess")
Out[8]: 
0      False
1      False
2      False
3      False
4      False
       ...  
886    False
887    False
888    False
889    False
890    False
Name: Name, Length: 891, dtype: bool
```

```python
In [9]: titanic[titanic["Name"].str.contains("Countess")]
Out[9]: 
     PassengerId  Survived  Pclass  ... Cabin Embarked  Surname
759          760         1       1  ...   B77        S   Rothes

[1 rows x 13 columns]
```

The string method [`Series.str.contains()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.contains.html#pandas.Series.str.contains) checks for each of the values in the column `Name` if the string contains the word `Countess` and returns for each of the values `True` (`Countess` is part of the name) or `False` (`Countess` is not part of the name). This output can be used to subselect the data using conditional (boolean) indexing introduced in the [subsetting of data tutorial](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset). As there was only one countess on the Titanic, we get one row as a result.

> More powerful extractions on strings are supported, as the [`Series.str.contains()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.contains.html#pandas.Series.str.contains) and [`Series.str.extract()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.extract.html#pandas.Series.str.extract) methods accept [regular expressions](https://docs.python.org/3/library/re.html), but out of scope of this tutorial.

> More information on extracting parts of strings is available in the user guide section on [string matching and extracting](https://pandas.pydata.org/docs/user_guide/text.html#text-extract).

*Which passenger of the Titanic has the longest name?*

```python
In [10]: titanic["Name"].str.len()
Out[10]: 
0      23
1      51
2      22
3      44
4      24
       ..
886    21
887    28
888    40
889    21
890    19
Name: Name, Length: 891, dtype: int64
```

To get the longest name we first have to get the lengths of each of the names in the `Name` column. By using pandas string methods, the [`Series.str.len()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.len.html#pandas.Series.str.len) function is applied to each of the names individually (element-wise).

```python
In [11]: titanic["Name"].str.len().idxmax()
Out[11]: 307
```

Next, we need to get the corresponding location, preferably the index label, in the table for which the name length is the largest. The [`idxmax()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.idxmax.html#pandas.Series.idxmax) method does exactly that. It is not a string method and is applied to integers, so no `str` is used.

```python
In [12]: titanic.loc[titanic["Name"].str.len().idxmax(), "Name"]
Out[12]: 'Penasco y Castellana, Mrs. Victor de Satode (Maria Josefa Perez de Soto y Vallejo)'
```

Based on the index name of the row (`307`) and the column (`Name`), we can do a selection using the loc operator, introduced in the [tutorial on subsetting](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset).

*In the “Sex” column, replace values of “male” by “M” and values of “female” by “F”.*

```python
In [13]: titanic["Sex_short"] = titanic["Sex"].replace({"male": "M", "female": "F"})

In [14]: titanic["Sex_short"]
Out[14]: 
0      M
1      F
2      F
3      F
4      M
      ..
886    M
887    F
888    F
889    M
890    M
Name: Sex_short, Length: 891, dtype: object
```

Whereas [`replace()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.replace.html#pandas.Series.replace) is not a string method, it provides a convenient way to use mappings or vocabularies to translate certain values. It requires a `dictionary` to define the mapping `{from : to}`.

There is also a [`replace()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.replace.html#pandas.Series.str.replace) method available to replace a specific set of characters. However, when having a mapping of multiple values, this would become:

```python
titanic["Sex_short"] = titanic["Sex"].str.replace("female", "F")
titanic["Sex_short"] = titanic["Sex_short"].str.replace("male", "M")
```

This would become cumbersome and easily lead to mistakes. Just think (or try out yourself) what would happen if those two statements are applied in the opposite order…

**REMEMBER**
- String methods are available using the `str` accessor.
- String methods work element-wise and can be used for conditional indexing.
- The `replace` method is a convenient method to convert values according to a given dictionary.

> A full overview is provided in the user guide pages on [working with text data](https://pandas.pydata.org/docs/user_guide/text.html#text).