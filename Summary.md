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
Counting values: df[']