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

### Look at histograms
``` python
plt.figure(figsize=(15, 5))

plt.subplot(1, 3, 1)
plt.hist(df['tip'], bins=10, color='#74b9ff')
median_all = df['tip'].median()
plt.axvline(median_all, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_all:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 2)
plt.hist(smokers_df['tip'], bins=10, color='#ff7675')
median_smokers = smokers_df['tip'].median()
plt.axvline(median_smokers, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_smokers:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Smokers tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 3)
plt.hist(non_smokers_df['tip'], bins=10, color='#55efc4')
median_non_smokers = non_smokers_df['tip'].median()
plt.axvline(median_non_smokers, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_non_smokers:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Non-smokers tip values')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```
<img width="791" alt="Ảnh màn hình 2025-05-20 lúc 21 51 04" src="https://github.com/user-attachments/assets/d06245de-596f-4f37-9f69-53e9f2412916" />

**Insight:** Based on Histogram, Smokers will give more tips than Non-smokers

### Mann-Whitney U test 

✅ Formulation of Hypotheses

Null Hypothesis (H₀):
The distribution of tip amounts for smokers is equal to or less than that of non-smokers.

Alternative Hypothesis (H₁):
The distribution of tip amounts for smokers is greater than that of non-smokers.
``` python
from scipy.stats import mannwhitneyu

u_stat, p_value = mannwhitneyu(smokers_df['tip'], non_smokers_df['tip'], alternative='greater')
print(f"Mann–Whitney U test: U = {u_stat:.2f}, p = {p_value:.4f}")
```
Mann–Whitney U test: U = 7163.00, p = 0.3960

**Final Conclusion:** p > 0.05, There is insufficient statistical evidence to conclude that smokers tip more than non-smokers.

## 👨👩 Do males give more tips?
### Separate Males and Females
``` python
males_df = df[df['sex'] == 'Male']
females_df = df[df['sex'] == 'Female']
```
### Compare their measures of central tendency
#### 👨 Male
``` python
males_tip_min = males_df['tip'].min()
males_tip_max = males_df['tip'].max()
males_tip_mean = males_df['tip'].mean()
males_tip_median = males_df['tip'].median()
```
#### 👩 Female
``` python
females_tip_min = females_df['tip'].min()
females_tip_max = females_df['tip'].max()
females_tip_mean = females_df['tip'].mean()
females_tip_median = females_df['tip'].median()
```
Let's show the retrieved results together
``` python
all_vals_dict1 = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Males': {'min': males_tip_min, 'max': males_tip_max, 'mean': males_tip_mean, 'median': males_tip_median},
    'Females': {'min': females_tip_min, 'max': females_tip_max, 'mean': females_tip_mean, 'median': females_tip_median}
}

all_mct1 = pd.DataFrame(all_vals_dict1)
all_mct1
```
| **Statistic** | **Common** | **Males ♂️** | **Females ♀️** |
|---------------|------------|--------------|----------------|
| **min**       | 1.000000   | 1.000000     | 1.000000       |
| **max**       | 10.000000  | 10.000000    | 6.500000       |
| **mean**      | 2.998279   | 3.089618     | 2.833448       |
| **median**    | 2.900000   | 3.000000     | 2.750000       |

- Based on max Tips: Males give more tips than Females
- Based on median: Males give more tips than Common and Females

**General conclusion:** Males will give more tips than Common and Females

### Look at histograms
``` python
plt.figure(figsize=(15, 5))

plt.subplot(1, 3, 1)
plt.hist(df['tip'], bins=10, color='#74b9ff')
median_all = df['tip'].median()
plt.axvline(median_all, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_all:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 2)
plt.hist(males_df['tip'], bins=10, color='#ff7675')
median_males = males_df['tip'].median()
plt.axvline(median_males, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_males:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Males tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 3)
plt.hist(females_df['tip'], bins=10, color='#55efc4')
median_females = females_df['tip'].median()
plt.axvline(median_females, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_females:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Females tip values')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```

<img width="794" alt="Ảnh màn hình 2025-05-20 lúc 23 52 03" src="https://github.com/user-attachments/assets/ad54db72-a0ea-4447-adb2-25ae395bd5b3" />

**Insight:** Based on Histogram, Smokers will give more tips than Non-smokers

### Mann-Whitney U test 

✅ Formulation of Hypotheses

Null Hypothesis (H₀):
The distribution of tip amounts for Male is equal to or less than that of Females.

Alternative Hypothesis (H₁):
The distribution of tip amounts for Male is greater than that of Females.
``` python
u_stat, p_value = mannwhitneyu(males_df['tip'], females_df['tip'], alternative='greater')
print(f"Mann–Whitney U test: U = {u_stat:.2f}, p = {p_value:.4f}")
```
Mann–Whitney U test: U = 7289.50, p = 0.1917

**Final Conclusion:** p > 0.05, There is insufficient statistical evidence to conclude that males tip more than females.

## 📆 Do weekends bring more tips?

### Separate Weekend and Non-Weekend
```python
weekends_df = df.query('day == "Sat" or day == "Sun"')
non_weekend_df = df.query('day != "Sat" and day != "Sun"')
```
### Compare their measures of central tendency
#### 📅 Weekend
``` python
weekends_tip_min = weekends_df['tip'].min()
weekends_tip_max = weekends_df['tip'].max()
weekends_tip_mean = weekends_df['tip'].mean()
weekends_tip_median = weekends_df['tip'].median()
```
#### 💼 Non-weekend

``` python
non_weekends_tip_min = non_weekend_df['tip'].min()
non_weekends_tip_max = non_weekend_df['tip'].max()
non_weekends_tip_mean = non_weekend_df['tip'].mean()
non_weekends_tip_median = non_weekend_df['tip'].median()
```
Let's show the retrieved results together
``` python
all_vals_dict2 = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Weekends': {'min': weekends_tip_min, 'max': weekends_tip_max, 'mean': weekends_tip_mean, 'median': weekends_tip_median},
    'Non-Weekends': {'min': non_weekends_tip_min, 'max': non_weekends_tip_max, 'mean': non_weekends_tip_mean, 'median': non_weekends_tip_median}
}

all_mct2 = pd.DataFrame(all_vals_dict2)
all_mct2
```
| Statistic | Common   | Weekends  | Non-Weekends |
|-----------|----------|-----------|---------------|
| Min       | 1.000000 | 1.000000  | 1.00000       |
| Max       | 10.000000| 10.000000 | 6.70000       |
| Mean      | 2.998279 | 3.115276  | 2.76284       |
| Median    | 2.900000 | 3.000000  | 2.50000       |

**Insights based on measures of central tendency comparison:**

- Based on max Tips: Weekends give more tips than Non_Weekends
- Based on median: Weekends give more tips than Common and Non_Weekends
**General conclusion: Weekends will give more tips than Common and Non_Weekends**

### Look at histograms
``` python
plt.figure(figsize=(15, 5))

plt.subplot(1, 3, 1)
plt.hist(df['tip'], bins=10, color='#74b9ff')
median_all = df['tip'].median()
plt.axvline(median_all, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_all:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 2)
plt.hist(weekends_df['tip'], bins=10, color='#ff7675')
median_weekends = weekends_df['tip'].median()
plt.axvline(median_weekends, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_weekends:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Weekends tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 3)
plt.hist(non_weekend_df['tip'], bins=10, color='#55efc4')
median_non_weekends = non_weekend_df['tip'].median()
plt.axvline(median_non_weekends, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_non_weekends:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Non-Weekends tip values')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```

<img width="790" alt="Ảnh màn hình 2025-05-22 lúc 10 28 31" src="https://github.com/user-attachments/assets/a3831424-f3f1-45f8-b491-63645f19bc65" />

**Insight:** Weekends give more tips than Non-Weekends based on Histogram

### Mann-Whitney U test 

✅ Formulation of Hypotheses

Null Hypothesis (H₀):
The distribution of tip amounts for Weekend is equal to or less than that of Non-Weekend.

Alternative Hypothesis (H₁):
The distribution of tip amounts for Weekend is greater than that of Non-Weekend.
``` python
u_stat, p_value = mannwhitneyu(weekends_df['tip'], non_weekend_df['tip'], alternative='greater')
print(f"Mann–Whitney U test: U = {u_stat:.2f}, p = {p_value:.4f}")
```
Mann–Whitney U test: U = 7619.50, p = 0.0248

**Final Conclusion:** p-value < 0.05, We conclude that Weekday receive more tips than Non-Weekends

## 🕑 Do dinners bring more tips?

### Separate Dinner and Non-Dinner
```python
dinner_df = df.query('time == "Dinner"')
non_dinner_df = df.query('time != "Dinner"')
```
### Compare their measures of central tendency
#### 🍽️ Dinner
``` python
dinner_tip_min = dinner_df['tip'].min()
dinner_tip_max = dinner_df['tip'].max()
dinner_tip_mean = dinner_df['tip'].mean()
dinner_tip_median = dinner_df['tip'].median()
```
#### 🥪 Non-Dinner 
```python
non_dinner_tip_min = non_dinner_df['tip'].min()
non_dinner_tip_max = non_dinner_df['tip'].max()
non_dinner_tip_mean = non_dinner_df['tip'].mean()
non_dinner_tip_median = non_dinner_df['tip'].median()
```
Let's show the retrieved results together
```python
all_vals_dict3 = {
    'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
    'Dinner': {'min': dinner_tip_min, 'max': dinner_tip_max, 'mean': dinner_tip_mean, 'median': dinner_tip_median},
    'Non-Dinner': {'min': non_dinner_tip_min, 'max': non_dinner_tip_max, 'mean': non_dinner_tip_mean, 'median': non_dinner_tip_median}
}

all_mct3 = pd.DataFrame(all_vals_dict3)
all_mct3
```

| Statistic | Common   | Dinner | Non-Dinner |
|-----------|----------|--------------|-----------------|
| Min       | 1.000000 | 1.00000      | 1.250000        |
| Max       | 10.000000| 10.00000     | 6.700000        |
| Mean      | 2.998279 | 3.10267      | 2.728088        |
| Median    | 2.900000 | 3.00000      | 2.250000        |

**Insights based on measures of central tendency comparison:**

- Based on max Tips: Dinner give more tips than Non_Dinner
- Based on median: Dinner give more tips than Common and Non_Dinner
**General conclusion: Dinner will give more tips than Common and Non_Dinner**

### Look at histograms
``` python
plt.subplot(1, 3, 1)
plt.hist(df['tip'], bins=10, color='#74b9ff')
median_all = df['tip'].median()
plt.axvline(median_all, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_all:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(True)
plt.legend()


plt.subplot(1, 3, 2)
plt.hist(dinner_df['tip'], bins=10, color='#ff7675')
median_dinner = dinner_df['tip'].median()
plt.axvline(median_dinner, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_dinner:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Dinner tip values')
plt.grid(True)
plt.legend()

plt.subplot(1, 3, 3)
plt.hist(non_dinner_df['tip'], bins=10, color='#55efc4')
median_non_dinner = non_dinner_df['tip'].median()
plt.axvline(median_non_dinner, color='black', linestyle='dashed', linewidth=2,
            label=f'Median = {median_non_dinner:.2f}')
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Non-Dinner tip values')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```
<img width="788" alt="Ảnh màn hình 2025-05-22 lúc 10 46 10" src="https://github.com/user-attachments/assets/58531245-c6eb-4948-8ef6-e391057162c0" />

**Insight:** Dinner give more tips than Non-Dinner based on Histogram

### Mann-Whitney U test 

✅ Formulation of Hypotheses

Null Hypothesis (H₀):
The distribution of tip amounts for Dinner is equal to or less than that of Non-Dinner.

Alternative Hypothesis (H₁):
The distribution of tip amounts for Dinner is greater than that of Non-Dinner.
``` python
u_stat, p_value = mannwhitneyu(dinner_df['tip'], non_dinner_df['tip'], alternative='greater')
print(f"Mann–Whitney U test: U = {u_stat:.2f}, p = {p_value:.4f}")
```
Mann–Whitney U test: U = 7063.00, p = 0.0144

**Final Conclusion:** p-value < 0.05, We conclude that Dinner will receive more tips than Non-Dinner

## **Summarize**
**- Both Dinner and Weekends are significantly associated with higher tip amounts, supported by higher means, medians, and significant p-values from Mann–Whitney U tests**

**- Although Males and Smokers appear to tip more than females and Non-smokers based on mean, median, and max values, the p - value > 0.05 shows that there is no statistical evidence to confirm that**

