# Restaurant-Tips-Analysis
This project aims to use the restaurant tips dataset to practice creating composition plots and visualizations. We will examine the relationship between different variables and the tips given.

The dataset consists of information from 244 restaurant bills, collected in the US in 1987.

It includes details about the tips given to restaurant staff, such as the total bill, tip amount, gender of the person paying, smoking status, day of the week, time of day, and party size.
## 📥 Data import
```python
import pandas as pd
import matplotlib.pyplot as plt
df = pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')
```
## 🔍 Data exploration
### Test sample
```python
df.head()
```
| id | total_bill | tip  | sex    | smoker | day | time   | size |
|----|------------|------|--------|--------|-----|--------|------|
| 0  | 16.99      | 1.01 | Female | No     | Sun | Dinner | 2    |
| 1  | 10.34      | 1.66 | Male   | No     | Sun | Dinner | 3    |
| 2  | 21.01      | 3.50 | Male   | No     | Sun | Dinner | 3    |
| 3  | 23.68      | 3.31 | Male   | No     | Sun | Dinner | 2    |
| 4  | 24.59      | 3.61 | Female | No     | Sun | Dinner | 4    |

As you can see each observation represents a customer who left a tip at a restaurant.

We can see information about:

- the day it occurred

- if it was at lunch or dinner

- the total bill

- the sex of the person

- if they were a smoker or not

- the size of the party

### Column types checking
```python
df.info()
```
```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 244 entries, 0 to 243
Data columns (total 8 columns):
 #   Column      Non-Null Count  Dtype  
---  ------      --------------  -----  
 0   id          244 non-null    int64  
 1   total_bill  244 non-null    float64
 2   tip         244 non-null    float64
 3   sex         244 non-null    object 
 4   smoker      244 non-null    object 
 5   day         244 non-null    object 
 6   time        244 non-null    object 
 7   size        244 non-null    int64  
dtypes: float64(2), int64(2), object(4)
memory usage: 15.4+ KB
```

We have string columns considered as objects.

```python
df=df.convert_dtypes()
```
Check again (output columns and their types):
```python
df.info()
```
```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 244 entries, 0 to 243
Data columns (total 8 columns):
 #   Column      Non-Null Count  Dtype  
---  ------      --------------  -----  
 0   id          244 non-null    Int64  
 1   total_bill  244 non-null    Float64
 2   tip         244 non-null    Float64
 3   sex         244 non-null    string 
 4   smoker      244 non-null    string 
 5   day         244 non-null    string 
 6   time        244 non-null    string 
 7   size        244 non-null    Int64  
dtypes: Float64(2), Int64(2), string(4)
memory usage: 16.3 KB
```
### Basic Descriptive Statistics

``` python
df.describe()
```
|       | id     | total_bill | tip     | size    |
|-------|--------|------------|---------|---------|
| count | 244.0  | 244.0      | 244.0   | 244.0   |
| mean  | 121.5  | 19.785943  | 2.998279| 2.569672|
| std   | 70.580923 | 8.902412 | 1.383638| 0.9511  |
| min   | 0.0    | 3.07       | 1.0     | 1.0     |
| 25%   | 60.75  | 13.3475    | 2.0     | 2.0     |
| 50%   | 121.5  | 17.795     | 2.9     | 2.0     |
| 75%   | 182.25 | 24.1275    | 3.5625  | 3.0     |
| max   | 243.0  | 50.81      | 10.0    | 6.0     |

➡️ Let's move forward!

# 💸 Tip value influencers
## 🚬 Do people who smoke give more tips?
### Separate smokers and non-smokers
``` python
smokers_df = df[df['smoker'] == 'Yes']
non_smokers_df = df.query('smoker == "No"')
```
### Compare their measures of central tendency
#### 🌏 Whole dataset
Calculate them for the 'tip' column through the whole dataset and save them into the following variables:
``` python
common_tip_min =df['tip'].min()
common_tip_max = df['tip'].max()
common_tip_mean = df['tip'].mean()
common_tip_median = df['tip'].median()
```
#### 🚬 Smokers
``` python 
smokers_tip_min = smokers_df['tip'].min()
smokers_tip_max = smokers_df['tip'].max()
smokers_tip_mean = smokers_df['tip'].mean()
smokers_tip_median = smokers_df['tip'].median()
```
#### 🚭 Non-smokers
``` python
non_smokers_tip_min = non_smokers_df['tip'].min()
non_smokers_tip_max = non_smokers_df['tip'].max()
non_smokers_tip_mean = non_smokers_df['tip'].mean()
non_smokers_tip_median = non_smokers_df['tip'].median()
```

Let's show the retrieved results together

``` python
all_vals_dict = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Smokers': {'min': smokers_tip_min, 'max': smokers_tip_max, 'mean': smokers_tip_mean, 'median': smokers_tip_median},
    'Non-smokers': {'min': non_smokers_tip_min, 'max': non_smokers_tip_max, 'mean': non_smokers_tip_mean, 'median': non_smokers_tip_median}
}
all_mct = pd.DataFrame(all_vals_dict)
all_mct
```
| Statistic | Common    | Smokers   | Non-smokers |
|-----------|-----------|-----------|--------------|
| min       | 1.000000  | 1.00000   | 1.000000     |
| max       | 10.000000 | 10.00000  | 9.000000     |
| mean      | 2.998279  | 3.00871   | 2.991854     |
| median    | 2.900000  | 3.00000   | 2.740000     |

##### Insights based on measures of central tendency comparison:
- Based on max Tips: Smokers give more tips than Non_smokers
- Based on median: Smokers give more tips than Common and Non_smokers
  
**General conclusion:** Smokers will give more tips than Common and Non_smokers
