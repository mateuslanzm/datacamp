# DataCamp Data Science Associate in python

Quick notes to remind me the most important topics of the course (using Obsidian).

## 01.Introdution to python - ignored

## 02.Python intermediate - ignored

## 03.(PROJECT) Investigating Netflix movies

## 04.Data manipulation with pandas
### 04.1. How to transform DataFrames

df.values - returns the DF into a list of lists with all values

df.index - returns the index of the DF
### 04.2. Aggregating with DataFrames
Statistics of DataFrame:
	df[['column1,column2']].agg([function1,function2,...]) - applies a function to the column.
	df[column].cumsum(), cummin(), cummax(), cumprod() - accumalative functions.
	.median(), .mode(), .mean(), max(), min(), .std()
	Dropping duplicate names: df.drop_duplicates(subset=['column_name1,column_name2,...'])
	Counting values: df['column'].value_counts()

Grouped summary statistics
	Multiple grouped, multiple summaries: df.groupby(["column","column2"])["column_value"].agg([func1,func2,func3])

Pivot tables
	Multiple statistics: dogs.pivot_table(values="weight_kg", index="color", aggfunc=["mean", "median"])
	Summing with pivot tables: dogs.pivot_table(values="weight_kg", index="color", columns="breed", fill_value=0, margins=True)

### 04.3. Slicing and indexing DataFrames
Indexing: to locate by index `df.loc[['value1,value2']]`

Subset inner levels with a list of tuples: `dogs_ind3.loc[[("Labrador", "Brown"), ("Chihuahua", "Tan")]]`

Should I index values?
Pros: sometimes it facilitates the data manipulation with pre-defined functions.
Cons: violate the "tidy data" principles (each variable forms a column, each observation forms a row, and each value is in its own cell)

### 04.3. Visualizating your DataFrame

Histograms: used to visualize the number of `df.hist(), plt.show()`

`