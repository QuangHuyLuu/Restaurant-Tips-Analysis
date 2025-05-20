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
- **Rows:** 244
- **Columns:** 8
- **Memory usage:** 16.3 KB

| #  | Column      | Non-Null Count | Dtype    |
|----|-------------|----------------|----------|
| 0  | id          | 244 non-null   | Int64    |
| 1  | total_bill  | 244 non-null   | Float64  |
| 2  | tip         | 244 non-null   | Float64  |
| 3  | sex         | 244 non-null   | string   |
| 4  | smoker      | 244 non-null   | string   |
| 5  | day         | 244 non-null   | string   |
| 6  | time        | 244 non-null   | string   |
| 7  | size        | 244 non-null   | Int64    |

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
