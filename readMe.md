# Pandas

Most data you work with can be boiled down to a `tabular structure`, or more simply, as table. Tables are important because they're a simple way to process a lot of information, and the relationship that data points share with each other.

| Temperature | Measured At | Location |
|:------------|:------------|:---------|
| 76 | 2016-01-01 14:00:01 | valve |
| 86 | 2016-01-01 14:00:01 | compressor |
| 72 | 2016-01-01 15:00:01 | valve |
| 88 | 2016-01-01 15:00:01 | compressor |

At a glance, you can look at the temperature measurements of a chemical plant and see if there are any irregularities or concerning trends, and where any problems may be arising.

In order to apply this to Python, we need a dataset that can handle collections of different data types efficiently, which is where the `pandas` package comes in. You can create a dictionary, where each key is a column and their values is a list of data for that column, and use pandas to convert it into a table:

```py
import pandas as pd
```

```py
dictionary = {
    "Temperature": [76, 86, 72, 88],
    "Measured At": [
        '2016-01-01 14:00:01', '2016-01-01 14:00:01',
        '2016-01-01 15:00:01', '2016-01-01 15:00:01'
    ],
    "Location": ['valve', 'compressor', 'valve', 'compressor']
}

chem_plant_temps = pd.DataFrame(dictionary)

#    Temperature          Measured At    Location
# 0           76  2016-01-01 14:00:01       valve
# 1           86  2016-01-01 14:00:01  compressor
# 2           72  2016-01-01 15:00:01       valve
# 3           88  2016-01-01 15:00:01  compressor
```

This is a pretty simple way to create tables for any number of data collections:

```py
dictionary = {
    "Species": ["Bulbasaur", "Charmander", "Squirtle", "Pikachu", "Eevee"],
    "Pokedex #": ["001", "004", "007", "025", "133"],
    "Type": ["Grass/Poison", "Fire", "Water", "Electric", "Normal"]
}

kanto_starters = pd.DataFrame(dictionary)

#       Species Pokedex #          Type
# 0   Bulbasaur       001  Grass/Poison
# 1  Charmander       004          Fire
# 2    Squirtle       007         Water
# 3     Pikachu       025      Electric
# 4       Eevee       133        Normal
```

You can also set the indexes in tables generated with DataFrame by accessing the `index` attribute of the newly created data frame:

```py
# No index formatting:

state_data = {
    "initials": ["AL", "AK", "AR", "AK", "CA", "CO", "CT", "DE"],
    "names": ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut', 'Delaware'],
    "capitals": ['Montgomery', 'Juneau', 'Phoenix', 'Little Rock', 'Sacramento', 'Denver', 'Hartford', 'Dover'],
    "population": [5_024_279, 733_391, 7_151_502, 3_011_524, 39_538_223, 5_773_714, 3_605_944, 989_948]
} 

states = pd.DataFrame(state_data)

print(states)

#   initials        names     capitals  population
# 0       AL      Alabama   Montgomery     5024279
# 1       AK       Alaska       Juneau      733391
# 2       AR      Arizona      Phoenix     7151502
# 3       AK     Arkansas  Little Rock     3011524
# 4       CA   California   Sacramento    39538223
# 5       CO     Colorado       Denver     5773714
# 6       CT  Connecticut     Hartford     3605944
# 7       DE     Delaware        Dover      989948

```

```py
# Correct index formatting

state_data = {
    "names": ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut', 'Delaware'],
    "capitals": ['Montgomery', 'Juneau', 'Phoenix', 'Little Rock', 'Sacramento', 'Denver', 'Hartford', 'Dover'],
    "population": [5_024_279, 733_391, 7_151_502, 3_011_524, 39_538_223, 5_773_714, 3_605_944, 989_948]
}

initials = ["AL", "AK", "AR", "AK", "CA", "CO", "CT", "DE"]

states = pd.DataFrame(state_data)

states.index = initials

print(states)

#           names     capitals  population
# AL      Alabama   Montgomery     5024279
# AK       Alaska       Juneau      733391
# AR      Arizona      Phoenix     7151502
# AK     Arkansas  Little Rock     3011524
# CA   California   Sacramento    39538223
# CO     Colorado       Denver     5773714
# CT  Connecticut     Hartford     3605944
# DE     Delaware        Dover      989948
```

## CSV Files

Pandas also lets you utilize `.csv` files, or "comma separated values". This is what the file `brics.csv` looks like:

```csv
,country,capital,area,population
BR,Brazil,Brasilia,8.516,200.4
RU,Russia,Moscow,17.10,143.5
IN,India,New Delhi,3.286,1252
CH,China,Beijing,9.597,1357
SA,South Africa,Pretoria,1.221,52.98
```

If you have a csv file you want to reference, you can use pandas `read_csv` method to access it in your directory.

```py
brics = pd.read_csv("brics.csv")

print(brics)

#   Unnamed: 0       country    capital    area  population
# 0         BR        Brazil   Brasilia   8.516      200.40
# 1         RU        Russia     Moscow  17.100      143.50
# 2         IN         India  New Delhi   3.286     1252.00
# 3         CH         China    Beijing   9.597     1357.00
# 4         SA  South Africa   Pretoria   1.221       52.98
```

We do run into a formatting issue, where BR, RU, and so on are put under an unnamed column, because pandas gives number indexes by default. We can get around this by passing an argument into read_csv to tell it that the first column is actually the index column:

```py
brics = pd.read_csv("brics.csv", index_col = 0)

print(brics)

#          country    capital    area  population
# BR        Brazil   Brasilia   8.516      200.40
# RU        Russia     Moscow  17.100      143.50
# IN         India  New Delhi   3.286     1252.00
# CH         China    Beijing   9.597     1357.00
# SA  South Africa   Pretoria   1.221       52.98
```

## Index and Select Data

In addition to creating a table, we can also use panda tables to make accessing specific data values easier. If you only want to access a specific column, say, the "country" column in brics.csv, you can access it using brackets:

```py
print(brics['country'])

# BR          Brazil
# RU          Russia
# IN           India
# CH           China
# SA    South Africa
# Name: country, dtype: object
```

This will return the data you want, along with the index column, and the `dtype`, or data type of the response. To further understand what it means, you can look into it with the `type` function:

```py
print(type(brics['country']))

# <class 'pandas.core.series.Series'>
```

We're working with a `series`, which is similar to an array and acts mostly the same as a DataFrame column. In a general sense, DataFrames are a collection of Series objects. Without getting too deep into what a Series is yet, just know you can avoid getting the wrong data type and keep your response as DataFrame by using double brackets instead:

```py
print(brics[['country']])

#          country
# BR        Brazil
# RU        Russia
# IN         India
# CH         China
# SA  South Africa

print(type(brics[['country']]))

# <class 'pandas.core.frame.DataFrame'>
```

You can pass in multiple arguments as well to get more than one column:

```py
print(brics[['country', 'capital']])

#          country    capital
# BR        Brazil   Brasilia
# RU        Russia     Moscow
# IN         India  New Delhi
# CH         China    Beijing
# SA  South Africa   Pretoria
```

And access rows by using `slice` syntax:

```py
print(brics[1:3])

#    country    capital    area  population
# RU  Russia     Moscow  17.100       143.5
# IN   India  New Delhi   3.286      1252.0
```

But this bracket syntax is limiting, in that you can only access one axis at a time, the x axis (rows) with slice syntax, or the y axis (columns) by searching via strings. If you want to search for data within both axes at the same time, use the `loc` and `iloc` methods.

Loc is label-based, and iloc is integer position-based. So when you use loc, you want to have a search term in mind, and when you use iloc, you want to search by the index.

```py
#          country    capital    area  population
# BR        Brazil   Brasilia   8.516      200.40
# RU        Russia     Moscow  17.100      143.50
# IN         India  New Delhi   3.286     1252.00
# CH         China    Beijing   9.597     1357.00
# SA  South Africa   Pretoria   1.221       52.98

print(brics.loc[['RU']]) # Searches for the "RU" index

#    country capital  area  population
# RU  Russia  Moscow  17.1       143.5

print(brics.iloc[[4]]) # Searches for the row with an index of 4

#          country   capital   area  population
# SA  South Africa  Pretoria  1.221       52.98
```

Same as the regular bracket search, you can include multiple arguments to search by multiple keys. But unlike the regular bracket syntax, you can search for specific rows, while also limiting the result to specific columns. This uses a similar double bracket syntax, but now there are two lists nested inside the main outer brackets:

```py
print(brics.loc[["RU", "IN", "CH"], ["country", "capital"]])

#    country    capital
# RU  Russia     Moscow
# IN   India  New Delhi
# CH   China    Beijing
```

If you want to search for all of the row data within a fixed set of columns, just replace the first list with a colon and specify the columns you want afterward:

```py
print(brics.loc[:, ["country", "capital"]])

#          country    capital
# BR        Brazil   Brasilia
# RU        Russia     Moscow
# IN         India  New Delhi
# CH         China    Beijing
# SA  South Africa   Pretoria
```

So in summary, iloc and loc are effectively the same methods, specifying a list of rows, followed by a list of columns, wrapped in an outer set of brackets. The difference is iloc searches by integers representing the indexes, and loc using the names of the index:

```py
print(brics.loc[["RU", "CH"], ["country", "population"]])

#    country  population
# RU  Russia       143.5
# CH   China      1357.0

print(brics.iloc[[1, 3], [0, 3]])

#    country  population
# RU  Russia       143.5
# CH   China      1357.0
```
