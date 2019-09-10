---
title: "Working with pandas dataframes"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
[Pandas](https://pandas.pydata.org) is another Python package which is very popular for data analysis. The key feature of pandas is the `dataframe`. In this lesson, we will cover pandas dataframes and some basic analysis.

## What is pandas?
You are already familiar with the data analyis library `numpy` and the `numpy array`. NumPy is useful when you are working with data that is all numeric. Pandas, however, is capable of handling data of lots of different types. It is designed to make working with "relational" or "labeled" data easy and intuitive. Central to the `pandas` package are the special data structures called pandas Series and DataFrames. Pandas dataframes are 2 dimensional and tabular, and is particularly suited to data which is heterogenous and in columns, like an SQL table or Excel spreadsheet. In fact, there are even functions which allow you to read data directly from excel spreadsheets or SQL databases (more on this later).

Pandas is built to closely work with NumPy. Many functions which work on NumPy arrays will also work on Pandas DataFrames.

To use pandas, you must first make sure it is installed, then import it. If you do not have pandas installed, install it using conda with the following command

~~~
conda install -c anaconda pandas 
~~~
{: .language-bash}

Next, open a new Jupyter notebook and import the module. When imported, pandas is typically abbreviated to `pd`

~~~
import pandas as pd
~~~
{: .language-python}

## Reading data

Today, we will be working with a data set that contains information about the elements in the periodic table  (2019 is the [International Year of the Periodic Table](https://www.iypt2019.org)!).The data is a csv (comma separated value) file from [PubChem](https://pubchem.ncbi.nlm.nih.gov/). You can download the file [here](../data/PubChemElements_all.csv).

Once you have the file downloaded and saved in your directory, we will load it into pandas. This file is a csv (comma separated value) file, so we will load it using the [`pd.read_csv`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) command.

~~~
periodic_data = pd.read_csv('PubChemElements_all.csv')
~~~
{: .language-python}

Since this file is relatively simple, we do not need any additional arguments to the function. The `read_csv` function reads in tabular data which is comma delimited by default. 

## Examining the data
The variable `periodic_data` is now a pandas DataFrame with the information contained in the csv file. You can examine the DataFrame using the `.head()` method. This shows the first 5 rows stored in the DataFrame.

~~~
periodic_data.head()
~~~
{: .language-python}

<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">

<div class="w3-container">
<div class="w3-responsive">
<table class="w3-table-all">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AtomicNumber</th>
      <th>Symbol</th>
      <th>Name</th>
      <th>AtomicMass</th>
      <th>CPKHexColor</th>
      <th>ElectronConfiguration</th>
      <th>Electronegativity</th>
      <th>AtomicRadius</th>
      <th>IonizationEnergy</th>
      <th>ElectronAffinity</th>
      <th>OxidationStates</th>
      <th>StandardState</th>
      <th>MeltingPoint</th>
      <th>BoilingPoint</th>
      <th>Density</th>
      <th>GroupBlock</th>
      <th>YearDiscovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>H</td>
      <td>Hydrogen</td>
      <td>1.008000</td>
      <td>FFFFFF</td>
      <td>1s1</td>
      <td>2.20</td>
      <td>120.0</td>
      <td>13.598</td>
      <td>0.754</td>
      <td>+1, -1</td>
      <td>Gas</td>
      <td>13.81</td>
      <td>20.28</td>
      <td>0.000090</td>
      <td>Nonmetal</td>
      <td>1766</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>He</td>
      <td>Helium</td>
      <td>4.002600</td>
      <td>D9FFFF</td>
      <td>1s2</td>
      <td>NaN</td>
      <td>140.0</td>
      <td>24.587</td>
      <td>NaN</td>
      <td>0</td>
      <td>Gas</td>
      <td>0.95</td>
      <td>4.22</td>
      <td>0.000179</td>
      <td>Noble gas</td>
      <td>1868</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Li</td>
      <td>Lithium</td>
      <td>7.000000</td>
      <td>CC80FF</td>
      <td>[He]2s1</td>
      <td>0.98</td>
      <td>182.0</td>
      <td>5.392</td>
      <td>0.618</td>
      <td>+1</td>
      <td>Solid</td>
      <td>453.65</td>
      <td>1615.00</td>
      <td>0.534000</td>
      <td>Alkali metal</td>
      <td>1817</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>Be</td>
      <td>Beryllium</td>
      <td>9.012183</td>
      <td>C2FF00</td>
      <td>[He]2s2</td>
      <td>1.57</td>
      <td>153.0</td>
      <td>9.323</td>
      <td>NaN</td>
      <td>+2</td>
      <td>Solid</td>
      <td>1560.00</td>
      <td>2744.00</td>
      <td>1.850000</td>
      <td>Alkaline earth metal</td>
      <td>1798</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>B</td>
      <td>Boron</td>
      <td>10.810000</td>
      <td>FFB5B5</td>
      <td>[He]2s2 2p1</td>
      <td>2.04</td>
      <td>192.0</td>
      <td>8.298</td>
      <td>0.277</td>
      <td>+3</td>
      <td>Solid</td>
      <td>2348.00</td>
      <td>4273.00</td>
      <td>2.370000</td>
      <td>Metalloid</td>
      <td>1808</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Pandas has read the data into a table. The first row of the file was used for column headers.

> ## Previewing the end of the DataFrame
> You can see the **last** 5 rows of the dataframe using the `.tail()` command. For example, to see the last 5 rows of our DataFrame, we would type
> ~~~
> periodic_data.tail()
> ~~~
> {: .language-python}
{: .callout}

You can also see information about the DataFrame using the `.info()` method.

~~~
periodic_data.info()
~~~
{: .language-python}

~~~
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 118 entries, 0 to 117
Data columns (total 17 columns):
AtomicNumber             118 non-null int64
Symbol                   118 non-null object
Name                     118 non-null object
AtomicMass               118 non-null float64
CPKHexColor              108 non-null object
ElectronConfiguration    118 non-null object
Electronegativity        95 non-null float64
AtomicRadius             113 non-null float64
IonizationEnergy         102 non-null float64
ElectronAffinity         57 non-null float64
OxidationStates          103 non-null object
StandardState            118 non-null object
MeltingPoint             103 non-null float64
BoilingPoint             93 non-null float64
Density                  96 non-null float64
GroupBlock               118 non-null object
YearDiscovered           118 non-null object
dtypes: float64(8), int64(1), object(8)
memory usage: 15.8+ KB
~~~
{: .outuput}
 
 --       
- First, the data type is listed. This is a pandas DataFrame. Next, it tells us that we have 118 rows (118 entries) in the DataFrame.
- There are 17 columns of data
- Next, the name of each column along with the number of entries for that column, and the data type for that column. Note that for some columns, such as `Electronegativity`, there are fewer than 118 entries. This occurs because data is missing for some elements. When `pandas` read in our data file, it replaced these missing values with `NaN` (or 'not a number').

We can also see descriptive statistics easily using the `.describe()` command.

~~~
periodic_data.describe()
~~~
{: .language-python}

<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">

<div class="w3-container">
<div class="w3-responsive">
<table class="w3-table-all">
<thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AtomicNumber</th>
      <th>AtomicMass</th>
      <th>Electronegativity</th>
      <th>AtomicRadius</th>
      <th>IonizationEnergy</th>
      <th>ElectronAffinity</th>
      <th>MeltingPoint</th>
      <th>BoilingPoint</th>
      <th>Density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>118.000000</td>
      <td>118.000000</td>
      <td>95.000000</td>
      <td>113.000000</td>
      <td>102.000000</td>
      <td>57.000000</td>
      <td>103.000000</td>
      <td>93.000000</td>
      <td>96.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>59.500000</td>
      <td>146.607635</td>
      <td>1.732316</td>
      <td>201.902655</td>
      <td>7.997255</td>
      <td>1.072140</td>
      <td>1273.740553</td>
      <td>2536.212473</td>
      <td>7.608001</td>
    </tr>
    <tr>
      <td>std</td>
      <td>34.207699</td>
      <td>89.845304</td>
      <td>0.635187</td>
      <td>42.025707</td>
      <td>3.339066</td>
      <td>0.879163</td>
      <td>888.853859</td>
      <td>1588.410919</td>
      <td>5.878692</td>
    </tr>
    <tr>
      <td>min</td>
      <td>1.000000</td>
      <td>1.008000</td>
      <td>0.700000</td>
      <td>120.000000</td>
      <td>3.894000</td>
      <td>0.079000</td>
      <td>0.950000</td>
      <td>4.220000</td>
      <td>0.000090</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>30.250000</td>
      <td>66.480000</td>
      <td>1.290000</td>
      <td>180.000000</td>
      <td>6.020500</td>
      <td>0.470000</td>
      <td>516.040000</td>
      <td>1180.000000</td>
      <td>2.572500</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>59.500000</td>
      <td>142.573850</td>
      <td>1.620000</td>
      <td>202.000000</td>
      <td>6.960000</td>
      <td>0.754000</td>
      <td>1191.000000</td>
      <td>2792.000000</td>
      <td>7.072000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>88.750000</td>
      <td>226.777165</td>
      <td>2.170000</td>
      <td>229.000000</td>
      <td>8.998500</td>
      <td>1.350000</td>
      <td>1806.500000</td>
      <td>3618.000000</td>
      <td>10.275250</td>
    </tr>
    <tr>
      <td>max</td>
      <td>118.000000</td>
      <td>294.214000</td>
      <td>3.980000</td>
      <td>348.000000</td>
      <td>24.587000</td>
      <td>3.617000</td>
      <td>3823.000000</td>
      <td>5869.000000</td>
      <td>22.570000</td>
    </tr>
  </tbody>
  </table>
</div>
</div>

The describe function lists the `mean`, `max`, `min`, standard deviation and percentiles for each column excluding `NaN` values. 

## Accessing Data

Data in pandas DataFrames are stored in `rows` and `columns`.

## Accessing data using row and column numbers
Pandas DataFrames are organized using `columns`, which we have already discussed and `index` for the rows. 

### To access data using the row number and column number, use the `.iloc` method.

Data in pandas DataFrames can be accessed using slices in the same way as NumPy arrays using the `iloc` method.

~~~
# This will access row 35 (counting starting at 0)
periodic_data.iloc[35]
~~~
{: .language-python}

~~~
AtomicNumber                           36
Symbol                                 Kr
Name                              Krypton
AtomicMass                           83.8
CPKHexColor                        5CB8D1
ElectronConfiguration    [Ar]4s2 3d10 4p6
Electronegativity                       3
AtomicRadius                          202
IonizationEnergy                       14
ElectronAffinity                      NaN
OxidationStates                         0
StandardState                         Gas
MeltingPoint                       115.79
BoilingPoint                       119.93
Density                          0.003733
GroupBlock                      Noble gas
YearDiscovered                       1898
Name: 35, dtype: object
~~~
{: .output}

or, you can use slicing syntax.

~~~
periodic_data.iloc[35:45]
~~~
{: .language-python}

Like NumPy arrays, the second index is taken to be the column number.

~~~
# This selects the row 1 and column 2.
periodic_data.iloc[1, 2]
~~~
{: .language-python}

> ## Check your understanding
> Use the `iloc` function to   
>  Select row 5   
>  Select rows 30 to the end.   
>  Select column 2 through 4 and rows 50 to the end, every other row.
>> ## Solution
>> ~~~
>> periodic_data.iloc[5]
>> periodic_data.iloc[30:]
>> periodic_data.iloc[50::2, 2::5]
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


## Accessing information by name

Indices in pandas can either be identified using numbers (as in the row number, similar to numpy arrays), or by name using column names and index names.

### Accessing columns of data
Unless otherwise specified in the `read_csv` command, pandas will use the first row of the file as column names. You can use these column names to access data in a particular column.

To see all of the column names, you can type

~~~
periodic_data.columns
~~~
{: .language-python}

~~~
Index(['AtomicNumber', 'Symbol', 'Name', 'AtomicMass', 'CPKHexColor',
       'ElectronConfiguration', 'Electronegativity', 'AtomicRadius',
       'IonizationEnergy', 'ElectronAffinity', 'OxidationStates',
       'StandardState', 'MeltingPoint', 'BoilingPoint', 'Density',
       'GroupBlock', 'YearDiscovered'],
      dtype='object')
~~~
{: .output}

To access columns of data in pandas, you use the syntax

~~~
# Syntax for selecting pandas column
# dataframe_name['column name']
~~~
{: .language-python}

For example, to access the data in the Electronegativity column, we would use the syntax

~~~
periodic_data['Electronegativity']
~~~
{: .language-python}

~~~
0      2.20
1       NaN
2      0.98
3      1.57
4      2.04
       ... 
113     NaN
114     NaN
115     NaN
116     NaN
117     NaN
Name: Electronegativity, Length: 118, dtype: float64
~~~
{: .language-output}

If you would like to select multiple columns of data, you put multiple column names in a list.

~~~
periodic_data[['Name','Electronegativity']]
~~~
{: .language-python}

~~~
    Name	    Electronegativity
0	Hydrogen	2.20
1	Helium	    NaN
2	Lithium	    0.98
3	Beryllium	1.57
4	Boron	    2.04
...	...	...
113	Flerovium	NaN
114	Moscovium	NaN
115	Livermorium	NaN
116	Tennessine	NaN
117	Oganesson	NaN
118 rows × 2 columns
~~~
{: .language-output}

> ## Check your understanding
> How would you select the columns 'Name', 'AtomicMass', and 'StandardState''. 
>> ## Solution
>> To select these columns, you would put them in a list in square brackets (`[]`) following the name of the DataFrame.
>> ~~~
>> periodic_data[['Name', 'AtomicMass', 'StandardState']]
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

### Accessing rows and columns by name

We have already discussed column names, but what about row names? By default, the *names* of the rows are the same as the numbers. You may have noticed that when you are printing your dataframes, there is an extra first column of numbers going from 0 to 118. These are the row names. Unless otherwise specified in the `read_csv` command, the row names will default to the row number.

To access rows using the row name, use the `.loc` method. Currently our row names and numbers are the same, so this does not look any different than using `iloc` if only one index is specified.

~~~
periodic_data.loc[35]
~~~
{: .language-python}

~~~
AtomicNumber                           36
Symbol                                 Kr
Name                              Krypton
AtomicMass                           83.8
CPKHexColor                        5CB8D1
ElectronConfiguration    [Ar]4s2 3d10 4p6
Electronegativity                       3
AtomicRadius                          202
IonizationEnergy                       14
ElectronAffinity                      NaN
OxidationStates                         0
StandardState                         Gas
MeltingPoint                       115.79
BoilingPoint                       119.93
Density                          0.003733
GroupBlock                      Noble gas
YearDiscovered                       1898
Name: 35, dtype: object
~~~
{: .output}

However, now we can use the column name instead of the column number.

~~~
periodic_data.loc[35, 'YearDiscovered']
~~~
{: .language-python}

~~~
'1898'
~~~
{: .output}

Let's see what happens when we change our index to one of our columns, so that we can use `loc` to access data by name. Imagine that we wanted to be able to access rows in our DataFrame using the element symbol. We will use the command `set_index` to do this.

~~~
periodic_data.set_index('Symbol')
~~~
{: .language-python}

This will print your new DataFrame to the screen. You should notice that now the left most column is named 'Symbol' and lists the symbols for each element. Let's try accessing `Kr` using `loc`.

~~~
periodic_data.loc['Kr']
~~~
{: .language-python}

~~~
KeyError: 'Kr'
~~~
{: .error}

It didn't work! Why? Examine `periodic_data` again. The index is the same as before. This is because the `.set_index` method **returns** a new copy of the dataframe. If we wish to have later access to this copy, we have two options. We can capture the value in a variable (as in `periodic_data_index = periodic_data.set_index('Symbol')`), or we can use the keyword argument `inplace=True`. When we use this argument, pandas will overwrite the original DataFrame. **Always look for this argument in pandas functions.**

~~~
periodic_data.set_index('Symbol', inplace=True)
~~~
{: .language-python}

~~~
periodic_data.loc['Kr']
~~~
{: .language-python}

~~~
AtomicNumber                           36
Name                              Krypton
AtomicMass                           83.8
CPKHexColor                        5CB8D1
ElectronConfiguration    [Ar]4s2 3d10 4p6
Electronegativity                       3
AtomicRadius                          202
IonizationEnergy                       14
ElectronAffinity                      NaN
OxidationStates                         0
StandardState                         Gas
MeltingPoint                       115.79
BoilingPoint                       119.93
Density                          0.003733
GroupBlock                      Noble gas
YearDiscovered                       1898
Name: Kr, dtype: object
~~~
{: .output}

This lists all the information for the entry. Note the bottom, where it says `Name: Kr, dtype: object`. `Kr` is what we used for the index.

You can use row, column indexing with both the `loc` and `iloc` command. For example, to get the boiling point of Kr, you could use

~~~
periodic_data.loc['Kr', 'BoilingPoint']
~~~
{: .language-python}

~~~
119.93
~~~
{: .language-python}

Note that this is the same order as before - `row`, followed by `column`.

The same information could have been accessed (though less conveniently) using `iloc`. This would require you to know the numerical position of the 'BoilingPoint' column.
~~~
periodic_data.iloc[35, 12]
~~~
{: .language-python}

~~~
119.93
~~~
{: .output}

or, you could have combined ways to access data.

~~~
periodic_data['BoilingPoint'].iloc[35]
~~~
{: .language-python}

~~~
119.93
~~~
{: .output}

> ## Check your understanding
> How would you access each of the following:  
> The electron configuration of Boron.  
> The atomic radius of the element on row 115.  
> The value in cell (50, 5).  
>> ## Solution
>> ~~~
>> periodic_data.loc['B', 'ElectronConfiguration']
>> periodic_data['AtomicRadius'].iloc[115]
>> periodic_data.iloc[50,5]
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Reset, or change your index back to numbers using the command `reset_index`

~~~
periodic_data.reset_index(inplace=True)
~~~
{: .language-python}

This is a very important command. If you set another index without resetting the index, the `Symbol` column will be lost. This comman reverts it back to a column.

## Filtering and sorting your DataFrame

### Use `.query` to filter data
You can use the function `.query` to query your data. You type a logical expression in a string inside of the function.

For example, to find all of the elements with a melting point greater than 298,
~~~
periodic_data.query('MeltingPoint > 298')
~~~
{: .language-python}

You can combine several statements.

~~~
periodic_data.query('MeltingPoint > 298 and BoilingPoint < 500')
~~~
{: .language-python}

You can even use this synatx to compare two columns to one another.

~~~
periodic_data.query('MeltingPoint > BoilingPoint')
~~~
{: .language-python}

### Use `.sort_values` to sort data

To sort data, you can use the `sort_values` function. 

~~~
periodic_data.sort_values(by='MeltingPoint')
~~~

This will sort  the rows (axis=0) by the column values by default . It is also possible to columns based on values in a row. However, they all have to be the same type (numeric or string). 

For example,

~~~
numeric_data = periodic_data[['AtomicNumber', 'BoilingPoint', 'AtomicMass', 'MeltingPoint']].copy()

numeric_data.head()
~~~
{: .language-python}

~~~
numeric_data.sort_values(by='Au', axis=1)
~~~
{: .language-python}

**Note** how the column orders change.

### Use `.groupby` to group data

You can also group values in a DataFrame using the `groupby` function. 

~~~
grouped_data = periodic_data.groupby(by='StandardState')
~~~

Here, we are grouping the DataFrame based on values in the column 'ExpectedState'.

We can see the groups which have been created by using `.groups` 

~~~
grouped_data.groups
~~~
{: .language-python}

~~~
{'Expected to be a Gas': Int64Index([117], dtype='int64'),
 'Expected to be a Solid': Int64Index([109, 110, 111, 112, 113, 114, 115, 116], dtype='int64'),
 'Gas': Int64Index([0, 1, 6, 7, 8, 9, 16, 17, 35, 53, 85], dtype='int64'),
 'Liquid': Int64Index([34, 79], dtype='int64'),
 'Solid': Int64Index([  2,   3,   4,   5,  10,  11,  12,  13,  14,  15,  18,  19,  20,
              21,  22,  23,  24,  25,  26,  27,  28,  29,  30,  31,  32,  33,
              36,  37,  38,  39,  40,  41,  42,  43,  44,  45,  46,  47,  48,
              49,  50,  51,  52,  54,  55,  56,  57,  58,  59,  60,  61,  62,
              63,  64,  65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,
              76,  77,  78,  80,  81,  82,  83,  84,  86,  87,  88,  89,  90,
              91,  92,  93,  94,  95,  96,  97,  98,  99, 100, 101, 102, 103,
             104, 105, 106, 107, 108],
            dtype='int64')}
~~~
{: .output}

We can then retrieve any group by name. For example, to get the data associated with gases,

~~~
grouped_data.get_group('Gas')
~~~

This will return a pandas DataFrame where all of the elements returned have the expected state of Gas.

Grouping is particularly useful for calculating statistics about data that fits a particular criteria. 

~~~
for group, data in grouped_data:
    print(group, data['BoilingPoint'].mean(), data['BoilingPoint'].std())
~~~
{: .language-python}

~~~
Expected to be a Gas nan nan
Expected to be a Solid nan nan
Gas 102.45272727272727 76.27220858096467
Liquid 480.91499999999996 210.6683233189081
Solid 2922.236875 1361.7433032823824
~~~
{: .output}

## Built-in Plotting

Looking at our data above, we notice very high standard deviations for some of the groups. We can examine this visually by looking at a histogram plot. 

Pandas DataFrames have several built-in plotting functions, one of which is `.hist`.

We could thus make histograms for each of our groups by adding this command into our `for` loop.

~~~
for group, data in grouped_data:
    print(group, data['BoilingPoint'].mean(), data['BoilingPoint'].std())
    data.hist(column='BoilingPoint')
~~~
{: .language-python}

> ## Exercise
> Modify your `for` loop so that each graph has a title which is the group name. Save each image as a png with resolution 200 dpi with the file name `group_name`_bp_hist.png.
>> ## Solution
>> ~~~
>> for group, data in grouped_data:
>>    print(group, data['BoilingPoint'].mean(), data['BoilingPoint'].std())
>>    data.hist(column='BoilingPoint')
>>    plt.title(group)
>>    plt.savefig(F'{group}_bp_hist.png', dpi=200)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

## Adding new columns



{% include links.md %}

