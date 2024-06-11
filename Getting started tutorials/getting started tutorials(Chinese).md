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