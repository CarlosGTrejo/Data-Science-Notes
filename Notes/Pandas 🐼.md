# TOC
* [[#Making Data Frames]]
* [[#Inspecting]]
* [[#Sorting]]
* [[#Filter and Subsetting]]
* [[#Statistics]]
* [[#Missing Values]]
****

## Making  Data Frames
```py
df = pd.read_csv('path/to/file.csv', index_col=0)  # file.csv will be created as a dataframe and column 0 will be the index column
```

## Inspecting
```py
df.head()  # Shows first few rows of the data frame
df.info()  # Shows information about each column (eg. data type, # of missing values)
df.shape  # Gives the number of rows and columns of the data frame
df.describe()  # Shows summary statistics for each column (eg. median, std, min, max, etc.)

df.values  # Returns a 2D NumPy array of values
df.columns  # Returns an "Index" object of the column names
df.index  # Returns an "Index" object for the rows (ie. row numbers or row names)

df['col1'].value_counts(sort=True)  # Shows a counter of the values in 'col1', sort=True puts the largest counts on top
df['col1'].value_counts(normalize=True)  # Counts the values in 'col1' BUT displays the number as a proportion of the total.

df.sample(n)  # Print a random sample of n rows
```
****

## Sorting
```py
df.sort_values('col_name')  # Sorts the data in ascending order based on the given column name (use ascending=False to sort the data in descending order).
df.sort_values(['col1', 'col2'])  # Sorts the data by multiple values (eg. col1 and col2)
df.sort_values(['col1', 'col2'], ascending=[True, False])  # Sorts the data by multiple values and multiple directions depending on the list passed to ascending (col1 will be in asc. order and col2 will be in desc order)
```
****

## Filter and Subsetting
```py
df['col_name']  # Returns a single column and it's values as a pandas series
df[['col1', 'col2']]  # Returns a pandas data frame containing col1 and col2

df[df['col1' > 100]]  # Returns all values where "col1" is greater than 100
df[df['color' == 'blue']]  # Returns all values where 'color' is equal to blue
df[df['DOB'] > '2000-01-01']  # Returns all values where the "DOB" is after 2000

# Subsetting based on multiple conditions
is_blue = df['color'] == 'blue'
is_unique = df['unique'] == True
df[is_blue & is_unique]


# Or alternatively...
df[(df['color'] =='blue') & (df['unique'] == True)]

is_blue_or_red = df['color'].isin(['blue', 'red'])  # Returns a Pandas Series with Boolean values that satisfy the condition (ie. if color is equal to blue or red)
df[is_blue_or_red]  # Returns a pandas data frame where the color column has a value of blue or red

df.drop_duplicates(subset='col_name')  # Removes rows so that 'col_name' only shows once
df.drop_duplicates(subset=['col1', 'col2'])  # Removes rows so that col1 and col2 values only appear once in the data frame
```
****

## Statistics
```python
df['col1'].mean()
df['col1'].mode()

df['col1'].min()
df['col1'].max()

df['col1'].var()
df['col1'].std()

df['col1'].sum()
df['col1'].quantile()

df['col1'].cumsum()
df['col1'].cummax()
df['col1'].cummin()
df['col1'].cumprod()

df['col1'].agg([func1, func2])  # Apply custom function to one or more columns

dogs.groupby('color')['weight_kg'].mean()  # Shows the average weight for each dog color
dogs.groupby(['color', 'breed'])['weight_kg'].mean()  # Shows the average weight for dogs grouped by multiple columns (eg. black labradors and brown labradors)
dogs.groupby(['color', 'breed'])[['weight_kg', 'height_cm']].mean()  # Groups by multiple columns and shows the mean of multiple columns (eg. weight and height) (see )

dogs.groupby('color')['weight_kg'].agg([min, max, sum])  # Shows min, max, and the sum for each dog color

```
****

## Missing Values
```python
df.isna()  # Shows which values are NaN with booleans
df.isna().any()  # Shows if there is at least one missing value in each column
df.isna().sum()  # Shows how many missing values are in each column

df.isna().sum().plot(kind="bar")  # Plots a histogram of the missing values in each column

df.dropna()  # Removes rows that have a missing value, might not be useful if too many rows have missing values, and if the missing values are not random, then it might introduce bias

df.fillna(0)  # Replaces missing values with 0

df.notnull()  # The non-missing values in the dataframe will show True, all missing values will show False