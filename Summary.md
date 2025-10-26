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

Histograms: used to visualize the number of observations and distribution over a variable: `df.hist(), plt.show()`

Barplots: mostly used to compare values between variables `df.plot(kind='bar'), plt.show()`

Lineplots: mostly used to identify change of values over time `df.plot(kind='line'), plt.show()`

Scatterplot: mostly used to identify the relation between two variables `df.plot(kind='scatter')

Detecting missing values:
	`df.isna().any()`

Creating DataFrames:
	By list of dictionaries: creates row by row, ex.: `list_of_dic = [{'name':'Mateus','weight':'1.73'},...]
	By dictionary of lists: creates column by column, ex.: `dict_of_lists = {'name':'Mateus','Lucas','weight':'1.73','1.78'}

# 05. Joining data with Pandas

## 05.1 Basics
Setting suffixes: `wards_census = wards.merge(census, on='ward', suffixes=('_ward','_cen'))`

merging a table to itself: `original_sequels = sequels.merge(sequels, left_on='sequel', right_on='id', how='left', suffixes=('_org','_seq'))`
- hierarchical relationships
- sequential relationtships
- Graph data

Advanced merging and concatenating

Filtering joins: 

semi joins: 
- returns the intersection
- Returns rows only from the left table, not the right
- No duplicates

	Step 1: `genres_tracks = genres.merge(top_tracks, on='gid')`
	Step 2: `genres['gid'].isin(genres_tracks['gid'])`
	Step 3: `top_genres = genres[genres['gid'].isin(genres_tracks['gid'])]`

anti joins:
- returns the left table, excluding the intersection
- returns only from the left table, not the right

	Step 1: `genres_tracks = genres.merge(top_tracks, on='gid', how='left', indicator=True)`
	Step 2: `gid_list = genres_tracks.loc[genres_tracks['_merge'] == 'left_only', 'gid']`
	Step 3: `non_top_genres = genres[genres['gid'].isin(gid_list)]`
	


