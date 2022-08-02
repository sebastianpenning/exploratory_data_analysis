# exploratory_data_analysis
A project using Python in order to do exploratory data analysis on data regarding sales of commodity items

## Getting started


The minimal setup you need in order to further develop this is to have Python together with a IDE. 


## Project  

### Preparing data

First the neccessary libraries are loaded in for the project.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plot
pd.set_option('max_columns', None)
```

Together with the data itself. 

```python
df = pd.read_csv("Path/to/csv")
df = pd.DataFrame(df)
```

Now we will start to clean the data a bit. First of all, we'll drop some columns we don't need. 

```python
df = df.drop(df.columns[[2,4,5,7,8,9,13,14,15,20]], axis=1)
```

We'll create a new revenue column

```python
df['REVENUE'] = df.QUANTITY * df.UNIT_PRICE
```

And we'll reorder column to have a better overview

```python
df = df[['COMMODITY', 'COMMODITY_DESCRIPTION', 'QUANTITY', 'UNIT_PRICE', 'REVENUE', 'PURCHASE_ORDER', 'AWARD_DATE', 'VENDOR_CODE', 'CITY', 'ST', 'ZIP', 'CTRY']]
```

Finally, we drop the columns that have either 0 revenue or NaN values

```python
df = df.drop(df[df.REVENUE == 0].index)
df = df.dropna()
```
### Analysis

Now for the analysis we are interested to see which commodities are performing the best compared to those who are not performing well at all. 

```python 
df_com = df.groupby('COMMODITY').sum().reset_index()

results_high = df_com.query('REVENUE > 20000000')
results_low = df_com.query('REVENUE < 1')

results_high.plot.bar(x = 'COMMODITY', y  = 'REVENUE')
plot.show(block=True)
results_low.plot.bar(x = 'COMMODITY', y  = 'REVENUE')
plot.show(block=True)
```

Best: 

![rev_high](https://user-images.githubusercontent.com/88779306/182327188-7bef64e6-5a22-4927-9efa-8ebcd7303216.png)

Worst: 

![rev_low](https://user-images.githubusercontent.com/88779306/182327327-eb408b69-d7b9-4ff5-8cb8-18685e2304c9.png)

Now we'll look at which states perform the best and which the least. 

```python 
df_st = df.groupby('ST').sum().reset_index()

st_high = df_st.query('REVENUE > 10000000')
st_low = df_st.query('REVENUE < 100000')

st_high.plot.bar(x = 'ST', y = 'REVENUE')
plot.show(block=True)
st_low.plot.bar(x = 'ST', y = 'REVENUE')
plot.show(block=True)
```

Best: 

![st_high](https://user-images.githubusercontent.com/88779306/182327712-c29e5ac1-48a7-4e0f-a392-545c87133738.png)

Worst:

![st_low](https://user-images.githubusercontent.com/88779306/182327758-300fcc46-a7cd-476e-b6d3-699f58f7f6b1.png)

Then we want to see which product is performing the best in each state. 

```python 
df_comst = df.loc[df.groupby(['ST'], sort=False)['REVENUE'].idxmax()][['ST', 'COMMODITY', 'REVENUE']]
df_comst
```

|ST |	COMMODITY|	REVENUE|
|------|-----|-------|
|CO	|72543|	115000.00|
|TX	|72578|	7474357.86|
|MD	|65529|	230262.75|
|OH	|20811|	350000.00|
|GA	|0703620|	965159.40|
|CA	|0718055|	348101.70|
|NC	|45065	|1122304.20|
|AZ	|20554	|723679.92|
|IL	|72578|	7474357.86|
|FL	|28586|	6408900.00|
|NY	|0705930|	254197.00|
|TN	|755J516|	481188.25|
|MO	|84084280003|	386955.00|
|MS	|2856778|	1510476.00|
|LA	|28581|	150000.00|
|SC	|47584|	86235.00|
|AL	|2858660|	117600.00|
|NE	|2802939|	34401.72|
|VA	|20811|	219633.28|
|NJ	|55038|	310400.00|
|MN	|20868|	107893.50|
|IA	|55081260001|	200777.50|
|MI	|47010|	175624.20|
|WI	|07057|	711320.00|
|PA	|2851460|	212400.00|
|CT	|55670|	1100000.00|
|OK	|72556|	338581.12|
|MA	|07051|	270315.00|
|WA	|46514|	166650.00|
|KS	|45037|	161000.00|
|ID	|3054811|	15958.80|
|UT	|52580|	131472.00|
|IN	|0709374|	137116.00|
|NH	|68087|	17071.50|
|KY	|4757365|	21420.00|
|OR	|82068|	125700.00|
|ND	|7600417|	205306.92|
|DE	|49369|	143026.00|
|NV|	42040|	9872.60|
|SD	|02066|	99225.52|
|VT	|10007|	38487.00|
|AR	|02049|	31147.74|
|RI	|83833|	25200.00|
|NM	|20453|	33028.00|
|ME	|6551522|	25995.00|
|DC	|78544|	12000.00|
|MT	|2200414|	759.16|
|WV|	17542|	2044.57|



	



## Data

The data itself is data about commodity purchasing from the city of Austin since 2009. The link for the data can be found here: https://data.world/cityofaustin/3ebq-e9iz

## Licensing

The license used for this project is the MIT license which can be found in the files of this repository
