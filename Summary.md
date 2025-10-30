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

## 05.3 Advanced merging and concatenating

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

Concatenate tables vertically:

Combining tables and keeping track of the data: `pd.concat([inv_jan, inv_feb, inv_mar], ignore_index=False, keys=['jan','feb','mar'])`

Verifying integrity:

`.merge(validate=None)`
	`'one_to_one''one_to_many''many_to_one''many_to_many'`

overlapping integrity verification:
`.concat(verify_integrity=False`


## 05.4 Merging ordered and time series data:
When dealing with time series, using ordered is a good option, especially to deal with missing data.
`.merge_ordered()

forward fill:  `pd.merge_ordered(aapl, mcd, on='date', suffixes=('_aapl','_mcd'), fill_method='ffill')`

Merge using the nearest value (DataFrame must be sorted).
`merge_asof()`
- DataFrame must be sorted.
- similar to merge_ordered() left join
- match on the nearest key column and not exact matches - merged "on" columns must be sorted.
- The nearest forward or backward (Default) can be setted.
- 
`pd.merge_asof(visa, ibm, on=['date_time'], suffixes=('_visa','_ibm'), direction='forward')`

Selecting data with .query()

`stocks.query('nike > 96 or disney < 98')
`stocks_long.query('stock=="disney" or (stock=="nike" and close < 90)')`

Reshaping data with .melt() - unpivot table

Data wide format: pivoted table. Easier to read by people.
Data long format: unpivoted table. More acessable by computers.

`social_fin_tall = social_fin.melt(id_vars=['financial','company'], value_vars=['2018','2017'], var_name='year', value_name='dollars')

# 06. Introduction to statistics with Python

### 06.1 Summary statistics
Descriptive statistics: describe and summarize data.
Inferential statistics: use a sample to inference something about a population.

Data types:

Numeric (quantative):
	Continuous
	Discrete
Categorical (qualitative):
	Nominal (Unordered)
	Ordinal (Ordered)

Measures of center:
	Mean: works better for symmetrical data `np.mean()
	Median: works better for unsymmetrical data `np.median()
	Mode: most appearance `np.mode()

Measures of spread:
	Variance: average distance of each point to the mean `np.var( ddof=1) ` ddof=1 - for sample, hard to understand since the value is squared ^2
	Standard deviation: sqrt of the variance - the dimension is the same of the observations, easier to understand `np.std(ddof=1)` ddof=1 for sample
	Mean absolute deviation (MAD): absolute distance of each point to the mean and take the mean of these values. 
	SD vs MAD:
		SD. are more usual.
		SD: penalizes longer distances more than shorter ones.
		MAD: Penalizes each distance equally.

Quantiles (percentiles):
	Splits up the data on some number of equal parts. `np.quantile(df['column1], 0.5) `is equal to the median.
	Using `np.linspace()
	`np.quantile(df['column'], np.linspace(start, stop, num))

Interquartile range (IQR) - high of box on boxplot:
	Distance between the 25 and 75 percentiles.
	IQR = Q75 - Q25.
	`from scipy.stats import iqr 
	`iqr(df['column'])

Outliers:
	Definition: data < Q1 - 1.5 x IQR or data > Q3 + 1.5 IQR

All statistics in one go:
`df['column'].describe()'

### 06.2 Random numbers and probability

Sampling:
	setting a random seed - keeping the same result when running np.sample:
	`np.random.seed(10)`
	`sales_counts.sample()` gets the same result.

Probability distributions:

Discrete distributions:
	Law of large numbers: the mean of large samples tend to the expected value of theoretical probability distribution

Continuous distributions:
	continuous uniform distribution: 
		`from scipy.stats import uniform
		Probability P(waited value <=7): `uniform.cdf(waited value,min,max) 
		Generating random numbers according to uniform distribution: `uniform.rvs(min, max, size=10)
	Normal distribution:
	Exponential distribution:

Binomial distribution: 
	Creating observations binomial in Python:
	`from scipy.stats import binom
	`binom.rvs(# of coins (n), probability of heads/success (p), size=# of trials)
	ex.: `binom.rvs(3,0.5,size=10)
	Result: list of the sums of each head observation: array([0, 3, 2, 1, 0, 3, 2, 1,...])
	Probability of 7 heads? `binom.rvs(10, p=0.5, size=20)
	P(heads=7) = `binom.pmf(7,10,0.5)
	P(heads >7) = `binom.cdf(7,10,0.5)
	Expected value = `n x p

### 06.3 More distributions and the Central Limit Theorem

Normal distribution:
	Symmetrical, área=1, curve never hits 0.
	N(mean, sd).
	Standard normal distribution (0, 1): 68% 1sd, 95% 2sd, 99.7% 3sd.
	P(observation < 154 cm) = 
	`from scipy.stats import norm
	`norm.cdf(154, 161 - mean, 7 - sd)
	What height are 90% of women shorter than?
	`norm.ppf(0.9, 161, 7)
	Generating random numbers:
	`norm.rvs(161,7,size=10)

The central limit theorem:
	the sampling distribution will approach the normal distribution as the number of trials increases. Samples randomly and independently.

The Poisson distribution:
	Events appear to happen at a certain rate, but completely at random.
	Probability of some # of events occurring over a fixed period of time.
	Lambda: average number of events per time interval.
	Probability of a single value: if the average number of adoption per week is 8, what's the probability of 5 adoptions P(# adoptions = 5)?
	`from scipy.stats import poisson
	`poisson.pmf(5 -, 8 - lambda)
	At least 5 P(# <=5)?
	`poisson.cdf(5, 8)
	Sampling:
	`poisson.rvs(8, size=10)

Exponential distribution:
	Probability of time between Poisson events:
	Probability of >1 day between adoptions
	<10 minutos between restaurant arrivals
	6-8 months between earthquakes
	lambda(rate)
	Continuous
	Ex. on average, one customer service ticket is created every 2 minutes. So lambda = 0.5 customer service tickets created per minute.
	Expected value: 1/lambda.
	2 minutes in this case.
	Probability of P(wait<1 min):
	`expon.cdf(1, scale=2)

(Student's) t-distribution:
	Similar to Normal distribution
	Degrees of freedom - as the df increases, it approaches the normal distribution (lower, higher sd)

Log-normal distribution:
	Variable whose logarithm is normally distributed.
	Length of chess games
	Adult blood pressure.

### 06.4 Correlation and Experimental Design

Relationship between two variables:

Visualizing relationship with seaborn:
`import seaborn as sns
`sns.scatterplot(x='var1',y='var2',data=df, ci=None #trendline)
`plt.show()

Computing correlation (Pearson product-moment correlation (r) - most common):
`df[column1].corr(df[column2])
There's more than one way to calculate correlation. 

Correlation caveats:
	Correlation shouldn't be used blindly - visualize your data when possible (ex.: non-linear relationships have low linear correlation).
	Log transformation can be used to bring the correlation to become linear.
	sqrt (^1/2), reciprocal (1/x)...
	Always try to transform data so the correlation become linear, because there's a handful number of techniques for dealing with linear relationships.
	Correlation doesn't not imply causation.
	Confounding: a third variable (confounder) ex: correlation between coffee consumption and lung cancer. It seems there's a high correlation but no causation, only association. The causation is caused by a third variable (smoking) associated with coffee drinking and causating lung cancer.

 Design of experiments (DOE):
 
 Controlled experiments: 
	 Divide the participants randomly between Treatment group - apply changes and Control group - no changes. Compare between the effects to see if there's a causation to be inferred.
	Gold stardard of experiments will use:
		Randomized controlled trial.
		Placebo.
		Double-blind trial: person administering doesn't know which group is the control.

Observation study:
	Participants assign themselves.
	Studying lug cancer: you can not force people to start smoking.
	Can't establish causation, only association.

Longitudinal vs cross-sectional studies:
	Longitudinal study: participants are followed over a period of time to examine effect of treatment on response.
	Cross-sectional study: data on participants is collected from a single snapshot of time.
	