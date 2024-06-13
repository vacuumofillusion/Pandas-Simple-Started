# 1. pandas处理什么样的数据？

*我想开始使用pandas*

```python
In [1]: import pandas as pd
```

要加载pandas包并开始使用它，需要导入该包。社区公认的pandas别名为`pd`，因此将pandas加载为`pd`是pandas文档中的所有标准做法。

## pandas数据表表示

![](https://pandas.pydata.org/docs/_images/01_table_dataframe.svg)

*我想存储泰坦尼克号乘客的数据。对于若干乘客，我知道他们的名字（字符）、年龄（整数）和性别（男/女）数据。*

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

要手动将数据存储在表中，可以创建一个`DataFrame`。当使用Python字典列表时，字典的键将用作列标题，每个列表中的值将用作`DataFrame`的列。

[`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame) 是一个二维数据结构，可以在列中存储不同类型的数据（包括字符、整数、浮点数值、分类数据等）。它与电子表格、SQL表或R中的`data.frame`类似。

* 该表格有3列，每列都有一个列标签。列标签分别是 `Name`、`Age` 和 `Sex`。
* `Name` 列包含文本数据，每个值都是一个字符串；`Age` 列是数字；`Sex` 列是文本数据。

在电子表格软件中，我们数据的表格表示会看起来非常相似：

![](https://pandas.pydata.org/docs/_images/01_table_spreadsheet.png)

## `DataFrame` 中的每一列都是一个 `Series`

![](https://pandas.pydata.org/docs/_images/01_table_series.svg)

* 我只对 `Age` 列中的数据感兴趣*

```python
In [4]: df["Age"]
Out[4]: 
0    22
1    35
2    58
Name: Age, dtype: int64
```

当从pandas的[`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)中选择单个列时，结果是一个pandas的[`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series)。要选择列，请使用列标签在方括号 `[]` 中进行。

> 如果你熟悉Python的[字典](https://docs.python.org/3/tutorial/datastructures.html#tut-dictionaries)，那么选择单个列的方式与基于键选择字典值的方式非常相似。

你也可以从头开始创建一个`Series`：

```python
In [5]: ages = pd.Series([22, 35, 58], name="Age")

In [6]: ages
Out[6]: 
0    22
1    35
2    58
Name: Age, dtype: int64
```

pandas的`Series`没有列标签，因为它只是`DataFrame`的一个单列。但是，`Series`有行标签。

## 对DataFrame或Series进行操作

*我想知道乘客的最大年龄*

我们可以在`DataFrame`上选择`Age`列并应用`max()`函数来得到这个结果：

```python
In [7]: df["Age"].max()
Out[7]: 58
```

或者对`Series`进行操作：

```python
In [8]: ages.max()
Out[8]: 58
```

正如`max()`方法所展示的，你可以对`DataFrame`或`Series`进行操作。pandas提供了许多功能，每个功能都是一个你可以应用到`DataFrame`或`Series`上的方法。由于方法是函数，所以不要忘记使用圆括号`()`。

*我对我的数据表中数值数据的一些基本统计信息感兴趣*

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

[`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe) 方法提供了对`DataFrame`中数值数据的快速概览。由于`Name`和`Sex`列是文本数据，这些列默认情况下不会被[`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe)方法考虑。

许多pandas操作返回`DataFrame`或`Series`。`describe()`方法是pandas操作返回pandas `Series`或pandas `DataFrame`的一个例子。

> 这只是一个起点。类似于电子表格软件，pandas将数据表示为具有列和行的表格。除了表示形式外，pandas还支持在电子表格软件中进行的数据操作和计算。继续阅读接下来的教程以开始使用！

**记住**

- 导入包，即`import pandas as pd`
- 数据表以pandas的`DataFrame`形式存储
- `DataFrame`中的每一列都是一个`Series`
- 你可以通过向`DataFrame`或`Series`应用一个方法来进行操作

# 2.我如何读取和写入表格数据？

![](https://pandas.pydata.org/docs/_images/02_io_readwrite.svg)

```
本教程使用存储为CSV格式的泰坦尼克号数据集。数据包含以下数据列：
- PassengerId：每位乘客的ID。
- Survived：表示乘客是否存活。0表示存活，1表示未存活。
- Pclass：三种票类之一：一等舱、二等舱和三等舱。
- Name：乘客的名字。
- Sex：乘客的性别。
- Age：乘客的年龄（以年为单位）。
- SibSp：船上的兄弟姐妹或配偶数量。
- Parch：船上的父母或子女数量。
- Ticket：乘客的票号。
- Fare：表示票价。
- Cabin：乘客的船舱号。
- Embarked：登船港口。
```

*我想分析作为CSV文件提供的泰坦尼克号乘客数据。*

```python
In [1]: import pandas as pd

In [2]: titanic = pd.read_csv("data/titanic.csv")
```

pandas 提供了 [`read_csv()`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv) 函数，用于将存储为csv文件的数据读取到pandas的`DataFrame`中。pandas内置支持许多不同的文件格式或数据源（csv、excel、sql、json、parquet等），每种格式或数据源都带有`read_*`的前缀。

在读取数据后，请务必对数据进行检查。在显示`DataFrame`时，默认情况下将显示前5行和后5行：

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

*我想查看pandas DataFrame的前8行。*

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

要查看`DataFrame`的前N行，请使用[`head()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.head.html#pandas.DataFrame.head)方法，并将所需的行数（在此为8）作为参数传入。

> 相反，如果对最后N行感兴趣？pandas还提供了一个[`tail()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.tail.html#pandas.DataFrame.tail)方法。例如，`titanic.tail(10)`将返回DataFrame的最后10行。

要检查pandas如何解释每一列的数据类型，可以通过请求pandas的`dtypes`属性来实现：

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

对于每一列，都列出了所使用的数据类型。在这个`DataFrame`中，数据类型有整数（`int64`）、浮点数（`float64`）和字符串（`object`）。

> 在请求`dtypes`时，不需要使用括号！`dtypes`是`DataFrame`和`Series`的属性。`DataFrame`或`Series`的属性不需要括号。属性表示`DataFrame`/`Series`的一个特性，而方法（需要括号）则对`DataFrame`/`Series`执行某种操作，这在[第一个教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html#min-tut-01-tableoriented)中已经介绍过。

*我的同事请求将泰坦尼克号数据作为电子表格提供。*

```python
In [6]: titanic.to_excel("titanic.xlsx", sheet_name="passengers", index=False)
```

使用`read_*`函数将数据读取到pandas中，而`to_*`方法则用于存储数据。[`to_excel()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html#pandas.DataFrame.to_excel)方法将数据存储为Excel文件。在这个例子中，`sheet_name`被命名为“passengers”而不是默认的Sheet1。通过设置`index=False`，行索引标签不会被保存在电子表格中。

等效的读取函数`read_excel()`会将数据重新加载到`DataFrame`中：

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

*我对`DataFrame`的技术摘要感兴趣*

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

方法[`info()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.info.html#pandas.DataFrame.info)提供了关于`DataFrame`的技术信息，让我们更详细地解释输出：

- 它确实是一个`DataFrame`。
- 有891个条目，即891行。
- 每行都有一个行标签（也称为`index`），其值范围从0到890。
- 该表有12列。大多数列对每一行都有一个值（所有891个值都是`non-null`）。有些列确实有缺失值，少于891个`non-null`值。
- `Name`、`Sex`、`Cabin`和`Embarked`列由文本数据（字符串，也称为`object`）组成。其他列是数值数据，其中一些是整数（`integer`），另一些是实数（`float`）。
- 不同列中的数据类型（字符、整数等）通过列出`dtypes`来概括。
- 还提供了用于保存DataFrame的大致RAM使用量。

**记住**

- `read_*` 函数支持从许多不同的文件格式或数据源中获取数据到 pandas 中。
- 不同的 `to_*` 方法提供了从 pandas 导出数据的功能。
- `head`/`tail`/`info` 方法和 `dtypes` 属性方便进行初步检查。

# 3.我如何选择DataFrame的子集？

> 本教程使用泰坦尼克号数据集，以CSV格式存储

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

## 我如何从`DataFrame`中选择特定的列？

![](https://pandas.pydata.org/docs/_images/03_subset_columns.svg)

*我对泰坦尼克号乘客的年龄感兴趣。*

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

要选择单个列，请使用方括号`[]`和感兴趣的列的名称。

在[`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)中，每一列都是一个[`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series)。当选择单个列时，返回的对象是一个pandas `Series`。我们可以通过检查输出的类型来验证这一点：

```python
In [6]: type(titanic["Age"])
Out[6]: pandas.core.series.Series
```

并查看输出的`shape`：

```python
In [7]: titanic["Age"].shape
Out[7]: (891,)
```

[`DataFrame.shape`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.shape.html#pandas.DataFrame.shape)是pandas `Series`和`DataFrame`的一个属性（记住[读取和写入教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/02_read_write.html#min-tut-02-read-write)，属性不需要使用括号），包含行数和列数：(nrows, ncolumns)。pandas Series是一维的，因此只返回行数。

*我对泰坦尼克号乘客的年龄和性别感兴趣。*

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

要选择多个列，请在选择括号`[]`内使用列名的列表。

> 内层方括号定义了一个包含列名的[Python列表](https://docs.python.org/3/tutorial/datastructures.html#tut-morelists)，而外层括号则用于从pandas `DataFrame`中选择数据，如前面的示例所示。

返回的数据类型是pandas DataFrame：

```python
In [10]: type(titanic[["Age", "Sex"]])
Out[10]: pandas.core.frame.DataFrame

In [11]: titanic[["Age", "Sex"]].shape
Out[11]: (891, 2)
```

选择返回了一个包含891行和2列的`DataFrame`。请记住，`DataFrame`是二维的，具有行和列两个维度。

> 关于索引的基本信息，请参阅用户指南中关于[索引和选择数据](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-basics)的部分。

## 我如何从DataFrame中过滤特定的行？

![](https://pandas.pydata.org/docs/_images/03_subset_rows.svg)

*我对年龄大于35岁的乘客感兴趣。*

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

基于条件表达式选择行时，在选择括号`[]`内使用条件。

选择括号内的条件`titanic["Age"] > 35`检查哪些行的“Age”列的值大于35：

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

条件表达式（如`>`，但也可以是`==`，`!=`，`<`，`<=`等）的输出实际上是一个具有与原始`DataFrame`相同行数的布尔值pandas `Series`（要么是`True`，要么是`False`）。这样的布尔值`Series`可以通过将其放在选择括号`[]`中来过滤`DataFrame`。只有值为`True`的行才会被选中。

我们之前知道原始的Titanic `DataFrame`包含891行。让我们通过检查结果`DataFrame` `above_35`的`shape`属性来看看满足条件的行数：

```python
In [15]: above_35.shape
Out[15]: (217, 12)
```

*我对泰坦尼克号船舱等级为2和3的乘客感兴趣。*

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

类似于条件表达式，`[`isin()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.isin.html#pandas.Series.isin)` 条件函数为列表中提供的值所在的每一行返回`True`。要基于这样的函数过滤行，请在选择括号`[]`内使用条件函数。在本例中，选择括号内的条件`titanic["Pclass"].isin([2, 3])`检查哪些行的`Pclass`列的值是2或3。

上述操作等同于通过类为2或3的行进行过滤，并使用`|`（或）运算符将两个语句组合在一起：

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

当组合多个条件语句时，每个条件必须用括号 `()` 包围。此外，你不能使用 `or`/`and`，而需要使用 `|` 作为 `or` 运算符，`&` 作为 `and` 运算符。

请参阅用户指南中关于[布尔索引](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-boolean)或关于[isin 函数](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-basics-indexing-isin)的专门部分。

*我想处理已知年龄的乘客数据。*

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

[`notna()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.notna.html#pandas.Series.notna) 条件函数为每个非 `Null` 值的行返回 `True`。因此，这可以与选择括号 `[]` 结合使用来过滤数据表。

你可能会好奇实际上有什么变化，因为前五行仍然是相同的值。一种验证方法是检查形状是否已更改：

```python
In [22]: age_no_na.shape
Out[22]: (714, 12)
```

对于缺失值的更多专用函数，请参阅用户指南中关于[处理缺失数据](https://pandas.pydata.org/docs/user_guide/missing_data.html#missing-data)的部分。

## 如何从DataFrame中选择特定的行和列？

![](https://pandas.pydata.org/docs/_images/03_subset_columns_rows.svg)

*我对年龄大于35岁的乘客的姓名感兴趣。*

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

在这个例子中，我们一次性选择了行和列的子集，仅仅使用选择括号 `[]` 已经不够了。在选择括号 `[]` 前面需要使用 `loc`/`iloc` 操作符。当使用 `loc`/`iloc` 时，逗号前的部分是你想要的行，逗号后的部分是你想要选择的列。

当使用列名、行标签或条件表达式时，在选择括号 `[]` 前面使用 `loc` 操作符。在逗号前后，你可以使用单个标签、标签列表、标签切片、条件表达式或冒号。使用冒号表示你想要选择所有的行或列。

*我对第10行到第25行以及第3列到第5列感兴趣。*

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

再次，要同时选择行和列的子集，仅使用选择括号 `[]` 已经不够了。当特别关注基于在表中位置的某些行和/或列时，在选择括号 `[]` 前面使用 `iloc` 操作符。

当使用 `loc` 或 `iloc` 选择特定的行和/或列时，可以给所选数据分配新值。例如，给第四列的前三个元素分配名字 `anonymous`：

```python
In [26]: titanic.iloc[0:3, 3] = "anonymous"

In [27]: titanic.head()
Out[27]: 
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...  anonymous  NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

> 请参阅用户指南中关于[不同索引选择的部分](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-choice)，以深入了解 `loc` 和 `iloc` 的用法。

**请记住**

- 当选择数据子集时，使用方括号 `[]`。
- 在这些括号内，你可以使用单个列/行标签、列/行标签列表、标签切片、条件表达式或冒号。
- 当使用行和列名时，使用 `loc` 来选择特定的行和/或列。
- 当使用表中的位置时，使用 `iloc` 来选择特定的行和/或列。
- 你可以基于 `loc`/`iloc` 给选择分配新值。

# 4.我如何在pandas中创建图表？

![](https://pandas.pydata.org/docs/_images/04_plot_overview.svg)

> 对于本教程，使用了来自[OpenAQ](https://openaq.org/)的关于`NO2`的空气质量数据，并使用了[py-openaq](http://dhhagan.github.io/py-openaq/index.html)包。`air_quality_no2.csv`数据集提供了分别在巴黎、安特卫普及伦敦的FR04014、BETR801和London Westminster测量站的`NO2`值。

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

> `read_csv`函数的`index_col`和`parse_dates`参数的使用是为了将第一（0th）列定义为结果`DataFrame`的索引，并将该列中的日期转换为[`Timestamp`](https://pandas.pydata.org/docs/reference/api/pandas.Timestamp.html#pandas.Timestamp)对象。

*我想要快速可视化检查这些数据。*

```python
In [5]: air_quality.plot()
Out[5]: <Axes: xlabel='datetime'>

In [6]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_quick.png)

使用`DataFrame`，pandas默认会为每个包含数值数据的列创建一个折线图。

*我只想绘制数据表中来自巴黎的数据列。*

```python
In [7]: air_quality["station_paris"].plot()
Out[7]: <Axes: xlabel='datetime'>

In [8]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_paris.png)

要绘制特定的列，请使用[子集数据教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset)中的选择方法与[`plot()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.html#pandas.DataFrame.plot)方法结合使用。因此，`plot()`方法既适用于`Series`也适用于`DataFrame`。

*我想要可视化比较伦敦和巴黎测得的`NO2`值。*

```python
In [9]: air_quality.plot.scatter(x="station_london", y="station_paris", alpha=0.5)
Out[9]: <Axes: xlabel='station_london', ylabel='station_paris'>

In [10]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_scatter.png)

除了使用`plot`函数时的默认`line`折线图外，还有许多其他可用于绘制数据的选项。让我们使用一些标准的Python代码来获取可用绘图方法的概述：

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

> 在许多开发环境以及IPython和Jupyter Notebook中，可以使用TAB键来获取可用方法的概述，例如输入`air_quality.plot` + TAB。

其中一个选项是[`DataFrame.plot.box()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.box.html#pandas.DataFrame.plot.box)，它指的是[箱线图](https://en.wikipedia.org/wiki/Box_plot)（Box plot）。`box`方法适用于空气质量示例数据：

```python
In [12]: air_quality.plot.box()
Out[12]: <Axes: >

In [13]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_boxplot.png)

> 要了解除默认折线图之外的其他绘图方式的介绍，请参阅用户指南中关于[支持的绘图样式](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-other)的部分。

*我想要将每一列单独绘制在一个子图中。*

```python
In [14]: axs = air_quality.plot.area(figsize=(12, 4), subplots=True)

In [15]: plt.show()
```

![](https://pandas.pydata.org/docs/_images/04_airqual_area_subplot.png)

`plot`函数的`subplots`参数支持为每个数据列分别绘制子图。每个pandas绘图函数中的内置选项都值得审查。

> 用户指南中关于[绘图格式](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-formatting)的部分解释了一些其他格式化选项。

*我想要进一步自定义、扩展或保存生成的图。*

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

由pandas创建的每个绘图对象都是一个[Matplotlib](https://matplotlib.org/)对象。由于Matplotlib提供了大量自定义图表的选项，因此明确地将pandas与Matplotlib联系起来可以使图表充分利用Matplotlib的全部功能。在前面的示例中采用了这种策略：

```python
fig, axs = plt.subplots(figsize=(12, 4))        # 创建一个空的Matplotlib图表和坐标轴
air_quality.plot.area(ax=axs)                   # 使用pandas将面积图绘制在准备好的图表/坐标轴上
axs.set_ylabel("NO$_2$ 浓度")                   # 使用Matplotlib进行任何你喜欢的自定义
fig.savefig("no2_concentrations.png")           # 使用现有的Matplotlib方法保存图表/坐标轴
plt.show()                                      # 显示图表
```

**注意**
- `.plot.*` 方法适用于Series和DataFrame。
- 默认情况下，每列都作为不同的元素（线条、箱线图等）进行绘制。
- 由pandas创建的任何图表都是Matplotlib对象。

> pandas中的绘图功能的完整概述可以在[可视化页面](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization)中找到。

# 5.如何从现有列创建新列

> 本教程中，我们将使用[OpenAQ](https://openaq.org/)提供的关于`NO2`的空气质量数据，并使用[py-openaq](http://dhhagan.github.io/py-openaq/index.html)包。`air_quality_no2.csv`数据集提供了在巴黎的FR04014、安特卫普的BETR801和伦敦的Westminster测量站点的`NO2`值。

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

*我想把伦敦站点的`NO2`浓度转换为mg/m³。*

（如果我们假设温度为25摄氏度，压力为1013百帕，转换因子是1.882）

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

要创建一个新列，请在赋值语句的左侧使用带有新列名的`[]`括号。

> 值的计算是**逐元素**进行的。这意味着给定列中的所有值都会一次性乘以值1.882。你不需要使用循环来迭代每一行！

![](https://pandas.pydata.org/docs/_images/05_newcolumn_2.svg)

*我想检查巴黎与安特卫普的值之间的比率，并将结果保存在一个新列中。*

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

计算同样是逐元素进行的，所以`/`运算符应用于每一行的值。

其他数学运算符（`+`，`-`，`*`，`/`，...）或逻辑运算符（`<`，`>`，`==`，...）也是逐元素工作的。后者已经在[子集数据教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset)中被用来使用条件表达式过滤表格的行。

如果你需要更复杂的逻辑，你可以通过[`apply()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html#pandas.DataFrame.apply)使用任意的Python代码。

*我想将数据列重命名为[OpenAQ](https://openaq.org/)使用的相应站点标识符。*

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

[`rename()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rename.html#pandas.DataFrame.rename)函数可以用于行标签和列标签的重命名。提供一个字典，其中键是当前的名称，值是新的名称，以更新相应的名称。

映射不应仅限于固定名称，它也可以是一个映射函数。例如，将列名转换为小写字母也可以使用一个函数来实现：

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

[5 行 x 5 列]
```

> 关于列或行标签重命名的详细信息在用户指南中关于[重命名标签](https://pandas.pydata.org/docs/user_guide/basics.html#basics-rename)的部分有提供。

**记住**
- 通过在`[]`中指定新的列名，并将输出分配给DataFrame来创建新列。
- 操作是逐元素的，无需遍历行。
- 使用带有字典或函数的`rename`来重命名行标签或列名。

> 用户指南中包含了关于[列的添加和删除](https://pandas.pydata.org/docs/user_guide/dsintro.html#basics-dataframe-sel-add-del)的单独部分。

# 6. 如何计算汇总统计量

> 本教程使用存储在CSV文件中的泰坦尼克号数据集。

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

## 聚合统计量

![](https://pandas.pydata.org/docs/_images/06_aggregate.svg)

*泰坦尼克号乘客的平均年龄是多少？*

```python
In [4]: titanic["Age"].mean()
Out[4]: 29.69911764705882
```

可以使用不同的统计量，并且它们可以应用于包含数值数据的列。这些操作通常会排除缺失的数据，并且默认情况下会跨行进行操作。

![](https://pandas.pydata.org/docs/_images/06_reduction.svg)

*泰坦尼克号乘客的中位年龄和船票票价是多少？*

```python
In [5]: titanic[["Age", "Fare"]].median()
Out[5]: 
Age     28.0000
Fare    14.4542
dtype: float64
```

对一个`DataFrame`中的多列（选择两列会返回一个`DataFrame`，请参阅[子集数据教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html#min-tut-03-subset)）应用的统计量，会为每个数值列进行计算。

聚合统计量可以同时对多列进行计算。还记得[第一个教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html#min-tut-01-tableoriented)中的`describe`函数吗？

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

除了预定义的统计量，还可以使用[`DataFrame.agg()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg)方法为给定列定义聚合统计量的特定组合：

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

> 描述性统计的详细信息在[用户指南](https://pandas.pydata.org/docs/user_guide/basics.html#basics-stats)的[描述性统计](https://pandas.pydata.org/docs/user_guide/basics.html#basics-stats)部分中提供。

## 按类别分组统计聚合数据

![](https://pandas.pydata.org/docs/_images/06_groupby.svg)

*“泰坦尼克号”上的男性和女性乘客的平均年龄分别是多少？*

```python
In [8]: titanic[["Sex", "Age"]].groupby("Sex").mean()
Out[8]: 
              Age
Sex              
female  27.915709
male    30.726645
```

由于我们感兴趣的是每个性别的平均年龄，因此首先选择这两列数据：`titanic[["Sex", "Age"]`。接下来，对性别列应用[`groupby()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby)方法，以每个类别为一组。然后计算并返回每个性别的平均年龄。

对某一列中的每个类别（例如“性别”列中的男性和女性）计算给定统计量（如平均年龄）是一个常见的模式。`groupby`方法用于支持此类操作。这符合更一般的“分割-应用-组合”模式：

- **分割**：将数据分割成组
- **应用**：对每个组独立地应用函数
- **组合**：将结果组合成数据结构

在pandas中，应用和组合步骤通常是一起完成的。

在前面的示例中，我们明确选择了2列。如果没有，`mean`方法将通过传递`numeric_only=True`应用于包含数值列的每一列：

```python
In [9]: titanic.groupby("Sex").mean(numeric_only=True)
Out[9]: 
        PassengerId  Survived    Pclass  ...     SibSp     Parch       Fare
Sex                                      ...                               
female   431.028662  0.742038  2.159236  ...  0.694268  0.649682  44.479818
male     454.147314  0.188908  2.389948  ...  0.429809  0.235702  25.523893

[2 rows x 7 columns]
```

计算`Pclass`的平均值并没有多大意义。如果我们只对每个性别的平均年龄感兴趣，那么在分组数据上也支持列的选择（使用常规的矩形括号`[]`）：

```python
In [10]: titanic.groupby("Sex")["Age"].mean()
Out[10]: 
Sex
female    27.915709
male      30.726645
Name: Age, dtype: float64
```

![](https://pandas.pydata.org/docs/_images/06_groupby_select_detail.svg)

>`Pclass`列包含数值数据，但实际上代表了三个类别（或因子），分别用标签‘1’、‘2’和‘3’表示。对这些类别计算统计量并没有多大意义。因此，pandas 提供了一个 `Categorical` 数据类型来处理这种类型的数据。用户指南中的 [Categorical data](https://pandas.pydata.org/docs/user_guide/categorical.html#categorical) 部分提供了更多信息。

*对于每种性别和船舱等级的组合，平均船票价格是多少？*

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

分组可以同时基于多个列进行。将列名作为列表提供给 [`groupby()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby) 方法即可。

> 用户指南中的 [groupby operations](https://pandas.pydata.org/docs/user_guide/groupby.html#groupby) 部分提供了关于 split-apply-combine 方法的完整描述。

## 按类别统计记录数量

![](https://pandas.pydata.org/docs/_images/06_valuecounts.svg)

*每个船舱等级中有多少乘客？*

```python
In [12]: titanic["Pclass"].value_counts()
Out[12]: 
Pclass
3    491
1    216
2    184
Name: count, dtype: int64
```

[`value_counts()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html#pandas.Series.value_counts) 方法统计列中每个类别的记录数量。

这个方法其实是一个快捷方式，因为它实际上是一个结合每个组内的记录数量统计的分组操作：

```python
In [13]: titanic.groupby("Pclass")["Pclass"].count()
Out[13]: 
Pclass
1    216
2    184
3    491
Name: Pclass, dtype: int64
```

> 在 `groupby` 操作中，可以使用 `size` 和 `count` 方法。`size` 包括 `NaN` 值，只是提供了行数（表的大小），而 `count` 则排除缺失值。在 `value_counts` 方法中，使用 `dropna` 参数来包含或排除 `NaN` 值。

> 用户指南中有一个专门关于 `value_counts` 的部分，请查看 [离散化](https://pandas.pydata.org/docs/user_guide/basics.html#basics-discretization) 页面。

**记住**
- 可以在整个列或行上计算聚合统计量。
- `groupby` 提供了 split-apply-combine 模式的强大功能。
- `value_counts` 是一个方便的快捷方式，用于统计变量每个类别的条目数量。

> 用户指南中关于 [groupby 操作](https://pandas.pydata.org/docs/user_guide/groupby.html#groupby) 的页面提供了 split-apply-combine 方法的完整描述。

# 如何重塑表格的布局

> 本教程使用泰坦尼克号数据集，存储为CSV格式。

> （注意：这里似乎有一个小错误，原文“his tutorial”应为“This tutorial”。）本教程使用OpenAQ提供的关于`NO2`和小于2.5微米的颗粒物（Particulate matter）的空气质量数据，并使用py-openaq包进行处理。

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

## 对表格行进行排序

*我想根据乘客的年龄对泰坦尼克号的数据进行排序。*

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

*我想根据船舱等级和年龄对泰坦尼克号的数据进行降序排序。*

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

使用[`DataFrame.sort_values()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html#pandas.DataFrame.sort_values)，表格中的行会根据定义的列进行排序。索引将跟随行的顺序。

> 关于表格排序的更多详细信息，请参考用户指南中的[数据排序](https://pandas.pydata.org/docs/user_guide/basics.html#basics-sorting)部分。

## 长表转宽表格式

让我们使用空气质量数据集的一个小子集。我们专注于`NO2`数据，并且仅使用每个位置的前两个测量值（即每个组的头部）。这个子集数据将被称为`no2_subset`。

```python
# 仅筛选NO2数据
In [8]: no2 = air_quality[air_quality["parameter"] == "no2"]
```

```python
# 对于每个位置（groupby），使用两个测量值（头部）
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

*我希望将三个站点的值作为单独的列并排显示。*

```python
In [11]: no2_subset.pivot(columns="location", values="value")
Out[11]: 
location                   BETR801  FR04014  London Westminster
date.utc                                                       
2019-04-09 01:00:00+00:00     22.5     24.4                 NaN
2019-04-09 02:00:00+00:00     53.5     27.4                67.0
2019-04-09 03:00:00+00:00      NaN      NaN                67.0
```

[`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.pivot.html#pandas.pivot) 函数纯粹是对数据的重新整形：它需要每个索引/列组合只有一个值。

由于 pandas 直接支持绘制多个列（参见 [绘图教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html#min-tut-04-plotting)），从长表格式转换为宽表格式使得能够同时绘制不同的时间序列：

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

> 如果未定义 `index` 参数，则使用现有的索引（行标签）。

> 关于 [`pivot()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot.html#pandas.DataFrame.pivot) 的更多信息，请参阅用户指南中关于 [DataFrame 对象的透视](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-reshaping) 的部分。

![](https://pandas.pydata.org/docs/_images/07_pivot_table.svg)

*我希望得到表格中每个站点`NO2`和`PM2.5`的平均浓度。*

```python
In [14]: air_quality.pivot_table(
   ....:     values="value", index="location", columns="parameter", aggfunc="mean"
   ....: )
   ....: 
Out[14]: 
parameter                 no2       pm2.5
location                                
BETR801             26.950920  23.169492
FR04014             29.374284        NaN
London Westminster  29.740050  13.443568
```

在`pivot()`函数的情况下，数据只是被重新排列。当需要聚合多个值（在这个特定情况下，是不同时间步上的值）时，可以使用`pivot_table()`函数，并提供一个聚合函数（例如平均值）来组合这些值。

透视表是电子表格软件中广为人知的概念。如果对每个变量的行/列边缘（小计）感兴趣，请将`margins`参数设置为`True`：

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
parameter                 no2       pm2.5       All
location                                           
BETR801             26.950920  23.169492  24.982353
FR04014             29.374284        NaN  29.374284
London Westminster  29.740050  13.443568  21.491708
All                 29.430316  14.386849  24.222743
```

> 关于`pivot_table()`函数的更多信息，请参阅用户指南中关于[透视表](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-pivot)的部分。

如果你很好奇，`pivot_table()`确实与`groupby()`直接相关。通过对`parameter`和`location`进行分组，也可以得到相同的结果：

```python
air_quality.groupby(["parameter", "location"])[["value"]].mean()
```

## 宽表到长表格式

从上一节创建的宽表格式开始，我们使用[`reset_index()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.reset_index.html#pandas.DataFrame.reset_index)方法给`DataFrame`添加一个新的索引。

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

*我想将所有的空气质量`NO2`测量值收集到一个单独的列中（长表格式）。*

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

`DataFrame`上的[`pandas.melt()`](https://pandas.pydata.org/docs/reference/api/pandas.melt.html#pandas.melt)方法将数据表从宽表格式转换为长表格式。列标题成为新创建列中的变量名。

解决方案是`pandas.melt()`的简短版本应用方法。该方法会将没有在`id_vars`中提到的所有列合并成两列：一列包含列标题名，一列包含这些值本身。后一列默认名为`value`。

传递给`pandas.melt()`的参数可以更详细地定义：

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

额外的参数有以下效果：

- `value_vars`定义了哪些列要合并在一起
- `value_name`为值列提供了一个自定义列名，而不是默认的列名`value`
- `var_name`为收集列标题名的列提供了一个自定义列名。否则，它会使用索引名或默认的`variable`

因此，`value_name`和`var_name`只是为两个生成的列定义的用户自定义名称。要合并的列由`id_vars`和`value_vars`定义。

> 使用`pandas.melt()`从宽表格式转换为长表格式在用户指南的[通过melt重塑](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-melt)部分中有解释。

**注意**
- `sort_values`支持按一列或多列进行排序。
- `pivot`函数纯粹是数据的重新结构，而`pivot_table`支持聚合操作。
- `pivot`的反向操作（从长表到宽表）是`melt`（从宽表到长表）。

> 在 Pandas 的用户指南中，关于[重塑和透视](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping)的页面提供了完整的概述。