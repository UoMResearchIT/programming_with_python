---
title: "Pandas Essential"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

This library is again based on the numpy library that we discuss in a previous lesson. It provides python with a new object which allows us to work with "relational" or "labeled" data in an easy way. It provides a similar access to data coming from Database or Spreadsheet as the one that you can find in languages like R.

Pandas is designed more for datascience than pure numerical analysis but the tools can be combined together.

The library is providing input/output tools with which it is possible to open/save data from MS Excel, CSV or HDF5 files.

The library is usually imported as `pd`:
~~~
import pandas as pd
~~~
{: .language-python}

Later we will need to plot data - so the matplotlib library should be loaded:
~~~
import matplotlib.pyplot as plt
~~~
{: .language-python}

We will be working with data in the `data/gapminder_gdp_europe.csv` file - open this
with a text editor and look at the data layout.

The data within this file is organised much as you'd expect the data within a spreadsheet.  The first row of the file contains the headers for each of the columns. The first column contains the name of the countries, while the remaining columns contain the GDP values for these countries for each year. Pandas has the `read_csv` function for reading structured data such as this, which makes reading the file easy:
~~~
data = pd.read_csv('data/gapminder_gdp_europe.csv',index_col='country')
~~~
{: .language-python}
Here we specify that the `country` column should be used as the index column (`index_col`).

This creates a `DataFrame` object containing the dataset. This is similar to a numpy array, but has a number of significant differences to these objects. The first is that there are more ways to quickly understand a pandas dataframe. For example, the `info` function gives an overview of the data types and layout of the DataFrame:
~~~
data.info()
~~~
{: .language-python}
~~~
<class 'pandas.core.frame.DataFrame'>
Index: 30 entries, Albania to United Kingdom
Data columns (total 12 columns):
 #   Column          Non-Null Count  Dtype
---  ------          --------------  -----
 0   gdpPercap_1952  30 non-null     float64
 1   gdpPercap_1957  30 non-null     float64
 2   gdpPercap_1962  30 non-null     float64
 3   gdpPercap_1967  30 non-null     float64
 4   gdpPercap_1972  30 non-null     float64
 5   gdpPercap_1977  30 non-null     float64
 6   gdpPercap_1982  30 non-null     float64
 7   gdpPercap_1987  30 non-null     float64
 8   gdpPercap_1992  30 non-null     float64
 9   gdpPercap_1997  30 non-null     float64
 10  gdpPercap_2002  30 non-null     float64
 11  gdpPercap_2007  30 non-null     float64
dtypes: float64(12)
memory usage: 3.0+ KB
~~~
{: .output}
You can also carry out quick analysis of the data using the `describe` function:
~~~
data.describe()
~~~
{: .language-python}
~~~
 	gdpPercap_1952 	gdpPercap_1957 	gdpPercap_1962  ...
count 	30.000000 	30.000000 	30.000000 ...
mean 	5661.057435 	6963.012816 	8365.486814 ...
std 	3114.060493 	3677.950146 	4199.193906 ...
min 	973.533195 	1353.989176 	1709.683679 ...
25% 	3241.132406 	4394.874315 	5373.536612 ...
50% 	5142.469716 	6066.721495 	7515.733738 ...
75% 	7236.794919 	9597.220820 	10931.085347 ...
max 	14734.232750 	17909.489730 	20431.092700 ...
~~~
{: .output}

## Accessing elements, rows, and columns

The other major difference to numpy arrays is that we cannot directly access the array elements using numerical indices such as `data[0,0]`. It is possible to access columns of data using the column headers as indices (for example, `data['gdpPercap_1952']`), but this is not recommended. Instead you should use the `iloc` and `loc` methods.

The `iloc` method enables us to access the DataFrame as we would a numpy array:
~~~
print(data.iloc[0,0])
~~~
{: .language-python}
while the `loc` method enables the same access using the index and column headers:
~~~
print(data.loc["Albania", "gdpPercap_1952"])
~~~
{: .language-python}

For both of these methods, we can leave out the column indexes, and these will all be returned for the specified index row:
~~~
print(data.loc["Albania"])
~~~
{: .language-python}
This will not work for column headings (in the inverse of the `data['gdpPercap_1952']` method) however. While it is quick to type, we recommend trying to avoid using this method of slicing the DataFrame, in favour of the methods described below.

For both of these methods we can use the `:` character to select all elements in a row or column. For example, to get all information for Albania:
~~~
print(data.loc["Albania", :])
~~~
{: .language-python}
or:
~~~
print(data.iloc[0, :])
~~~
{: .language-python}

The `:` character by itself is shorthand to indicate all elements across that indice, but it can also be combined with index values or column headers to specify a slice of the DataArray:
~~~
print(data.loc["Albania", 'gdpPercap_1962':'gdpPercap_1972'])
~~~
{: .language-python}
If either end of the slice definition is omitted, then the slice will run to the end of that indice (just as it does for `:` by itself):
~~~
print(data.loc["Albania", 'gdpPercap_1962':])
~~~
{: .language-python}

Slices can also be defined using a list of indexes or column headings:
~~~
year_list = ['gdpPercap_1952','gdpPercap_1967','gdpPercap_1982','gdpPercap_1997']
country_list = ['Albania','Belgium']
print(data.loc[country_list, year_list])
~~~
{: .language-python}

## Masking data


## Plotting from Pandas

Pandas is integrated with matplotlib, and so data can be plotted directly using the integrated `plot` method. For example, to plot the GDP for Sweden:
~~~
data.loc['Sweden',:].plot()
plt.xticks(rotation=90)
~~~
{: .language-python}
Note that we've had to rotate the xtick labels by 90 degrees, because they do not fit neatly under the x-axis. Later we will clean these up properly.

Note that, in the case above, we passed a single column of data to the `plot` method - which it automatically transposed in order to make sense of the plot request (because this method usually works on rows of data). If you want to plot more than one column of data you will need to explicitly transpose the DataFrame yourself.

For example, we will transpose the GDP data for the first 3 countries in our dataset:
~~~
print(data.iloc[0:3,:].T)
~~~
{: .language-python}
This data is now ready to be plotted as a histogram - first we set the style of the plot to match that of the ggplot package in R:
~~~
plt.style.use('ggplot')
data.iloc[0:3,:].T.plot(kind='bar')
plt.xticks(rotation=90)
plt.ylabel('GDP per capita')
~~~
{: .language-python}

> ## Changing Column Labels
> Note that the x-tick labels have been taken directly from the index values of the transposed DataFrame (which were the original column labels). These don't really need to be more than the year of the GDP values, so we could change the column labels to reflect this.
>
> First we make a new copy of the dataframe (in case anything goes wrong):
> ~~~
> gdpPercap = data.copy(deep=True)
> ~~~
> {: .language-python}
> This we have given a more appropriate name, replacing the information that will be removed from the column headers.
>
> Now we will use the inbuilt `str.strip` method to clean up our column labels for the new
> dataframe. Which of these commands is correct:
> 1. `gdpPercap.columns = data.columns.str.strip('gdpPercap_')`
> 2. `gdpPercap = data.columns.str.strip('gdpPercap_')`
{: .challenge}


> ## Plotting GDP increases with time
> Now that we've cleaned up the column labels, we now want to plot the GDP data for
> Sweden and Iceland from 1972 onwards. The code block we will be using is:
> ~~~
> gdp_percap<BLOCK>.T.plot(kind='line')
>
> # Create legend.
> plt.legend(loc='upper left')
> plt.xlabel('Year')
> plt.ylabel('GDP per capita ($)')
> ~~~
> {: .language-python}
>
> Which of the following blocks of code should replace the `<BLOCK>` in the code above?
> 1. `.loc[['Sweden','Iceland'],:]`
>
{: .challenge}



{% include links.md %}












