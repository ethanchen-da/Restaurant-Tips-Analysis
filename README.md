# Restaurant Tips Analysis

This project aims to use the restaurant tips dataset to practice creating composition plots and visualizations. We will examine the relationship between different variables and the tips given.

The dataset consists of information from 244 restaurant bills, collected in the US in 1987.

It includes details about the tips given to restaurant staff, such as the total bill, tip amount, gender of the person paying, smoking status, day of the week, time of day, and party size.

# First Step
## Data Import

Before importing the data, I will import the Pandas and Matplotlib libraries to facilitate visualization for easier analysis.


```python
import pandas AS pd
import matplotlib.pyplot AS plt
```
Then load data from the following link: https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv

```python
url = 'https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv'
df = pd.read_csv(url)
```
After importing the data, we will proceed to explore the dataset.

## Data Exploration
### Sample

Let's take a look at the first 10 rows to be sure, that data is loaded properly:

```python
print(df.head(10))
```
|    |   id |   total_bill |   tip | sex    | smoker   | day   | time   |   size |
|---:|-----:|-------------:|------:|:-------|:---------|:------|:-------|-------:|
|  0 |    0 |        16.99 |  1.01 | Female | No       | Sun   | Dinner |      2 |
|  1 |    1 |        10.34 |  1.66 | Male   | No       | Sun   | Dinner |      3 |
|  2 |    2 |        21.01 |  3.5  | Male   | No       | Sun   | Dinner |      3 |
|  3 |    3 |        23.68 |  3.31 | Male   | No       | Sun   | Dinner |      2 |
|  4 |    4 |        24.59 |  3.61 | Female | No       | Sun   | Dinner |      4 |
|  5 |    5 |        25.29 |  4.71 | Male   | No       | Sun   | Dinner |      4 |
|  6 |    6 |         8.77 |  2    | Male   | No       | Sun   | Dinner |      2 |
|  7 |    7 |        26.88 |  3.12 | Male   | No       | Sun   | Dinner |      4 |
|  8 |    8 |        15.04 |  1.96 | Male   | No       | Sun   | Dinner |      2 |
|  9 |    9 |        14.78 |  3.23 | Male   | No       | Sun   | Dinner |      2 |

### Column Types checking

```python
print(df.info())
```
Before converting the data types

| # | Column      | Non-Null Count | Dtype   |
|:-:|:-------------|----------------:|:--------|
| 0 | id           | 244 non-null    | int64   |
| 1 | total_bill   | 244 non-null    | float64 |
| 2 | tip          | 244 non-null    | float64 |
| 3 | sex          | 244 non-null    | object  |
| 4 | smoker       | 244 non-null    | object  |
| 5 | day          | 244 non-null    | object  |
| 6 | time         | 244 non-null    | object  |
| 7 | size         | 244 non-null    | int64   |

**dtypes:** float64(2), int64(2), object(4)  
**memory usage:** 15.4+ KB

Some columns have the data type `object`, so I will convert their types and check them again.

```python
df = df.convert_dtypes()
print(df.info())
```
The result after converting the data types of some columns in the original dataset

| # | Column      | Non-Null Count | Dtype   |
|:-:|:-------------|----------------:|:--------|
| 0 | id           | 244 non-null    | Int64   |
| 1 | total_bill   | 244 non-null    | Float64 |
| 2 | tip          | 244 non-null    | Float64 |
| 3 | sex          | 244 non-null    | string  |
| 4 | smoker       | 244 non-null    | string  |
| 5 | day          | 244 non-null    | string  |
| 6 | time         | 244 non-null    | string  |
| 7 | size         | 244 non-null    | Int64   |

**dtypes:** Float64(2), Int64(2), string(4)  
**memory usage:** 16.3 KB

### Basic Descriptive Statistics
```python
print(df.describe())
```
|       |        id |  total_bill |       tip |     size |
|:------|-----------:|-------------:|-----------:|----------:|
| count | 244.000000 | 244.000000  | 244.000000 | 244.000000 |
| mean  | 121.500000 | 19.785943   | 2.998279   | 2.569672 |
| std   | 70.580923  | 8.902412    | 1.383638   | 0.951100 |
| min   | 0.000000   | 3.070000    | 1.000000   | 1.000000 |
| 25%   | 60.750000  | 13.347500   | 2.000000   | 2.000000 |
| 50%   | 121.500000 | 17.795000   | 2.900000   | 2.000000 |
| 75%   | 182.250000 | 24.127500   | 3.562500   | 3.000000 |
| max   | 243.000000 | 50.810000   | 10.000000  | 6.000000 |

From the information above, I noticed that we can further explore details such as **smoker, time, size, tips, sex**, and so on.
Let’s see what insights we can uncover!

## Analysis
Let's try to calculate measures of central tendency for the whole dataset first.
```python
#Define Values
Common_tip_min = df.tip.min()
Common_tip_max = df.tip.max()
Common_tip_mean = df.tip.mean()
Common_tip_median = df.tip.median()

# Make a list of values
Common_values = [Common_tip_min, Common_tip_max, Common_tip_mean, Common_tip_median]
# Round all the values to 4 decimal places
Common_values = map(lambda x: round(x, 4), Common_values)

# Make a dataframe from the list
Common_mct = pd.DataFrame(Common_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
print(Common_mct)
```
|          |     0   |
|----------|--------:|
| **min**    | 1.0000  |
| **max**    | 10.0000 |
| mean     | 2.9983  |
| median   | 2.9000  |

Above is the whole dataset in tabular form; we will convert it into **histograms** to get a more comprehensive visual overview.

```python
df.tip.hist(color= '#FF4500', bins=15, figsize=(15, 5), edgecolor='black', alpha=0.8)
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(axis='x')
plt.show()
```
<img width="1227" height="468" alt="output4" src="https://github.com/user-attachments/assets/98a3f30e-eb32-4ab2-a45a-f1fefcab8f4e" />

It is clear that most tip amounts are below **$5**, with the highest tip being $10.
The majority of customers tend to tip $2–$4.

### The difference by gender

```python
#Tạo DataFrame
Male_tip = df[df.sex == 'Male']
Female_tip = df[df.sex == 'Female']

Male_tip_min = male_tip.tip.min()
Male_tip_max = male_tip.tip.max()
Male_tip_mean = male_tip.tip.mean()
Male_tip_median = male_tip.tip.median()

Female_tip_min = female_tip.tip.min()
Female_tip_max = female_tip.tip.max()
Female_tip_mean = female_tip.tip.mean()
Female_tip_median = female_tip.tip.median()

dataset = [Male_tip, Female_tip]
titles = ['Male Tips', 'Female Tips']
colors = ['#2166ac', '#b2182b']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()
plt.style.use('seaborn-v0_8')

for i in range(2):
  axs[i].hist(dataset[i].tip, color= colors[i], edgecolor='black', alpha=0.8)
  axs[i].set_ylim(0, 55)
  axs[i].set_xlabel('Tip value')
  axs[i].set_ylabel('Frequency')
  axs[i].set_title(titles[i])
  axs[i].grid(axis='x')
  mean = dataset[i].tip.mean()     #Mean for each group
  axs[i].axvline(mean, color='red', linestyle='--', label=f'Mean: {mean:.2f}')
  axs[i].legend()


plt.tight_layout()
plt.show()
```
<img width="1490" height="490" alt="SEX" src="https://github.com/user-attachments/assets/4b066a89-7606-44b5-b5fd-3dfcffefbbc0" />


Result: When comparing tipping behavior between males and females, the results show that males tend to tip more frequently than females. Additionally, the tip amounts given by males are generally higher than those given by females. This trend clearly highlights that males are significantly more likely to tip, and to tip higher amounts, compared to females.

###The difference in tip amounts between Lunch and Dinner

```python
# Define Values
Lunch_tip = df[df.time == 'Lunch']
Dinner_tip = df[df.time == 'Dinner']

Lunch_tip_min = Lunch_tip.tip.min()
Lunch_tip_max = Lunch_tip.tip.max()
Lunch_tip_mean = Lunch_tip.tip.mean()
Lunch_tip_median = Lunch_tip.tip.median()

Dinner_tip_min = Dinner_tip.tip.min()
Dinner_tip_max = Dinner_tip.tip.max()
Dinner_tip_mean = Dinner_tip.tip.mean()
Dinner_tip_median = Dinner_tip.tip.median()

dataset1 = [Lunch_tip, Dinner_tip]
titles = ['Lunch Tips', 'Dinner Tips']
colors = ['#b40426', '#3b4cc0']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()
plt.style.use('seaborn-v0_8')


# Make a Histogram
for i in range(2):
  axs[i].hist(dataset1[i].tip, color= colors[i], edgecolor='black', alpha=0.8)
  axs[i].set_xlabel('Tip value')
  axs[i].set_ylabel('Frequency')
  axs[i].set_title(titles[i])
  axs[i].grid(axis='x')
  mean = dataset1[i].tip.mean()
  axs[i].axvline(mean, color='black', linestyle='--', label=f'Mean: {mean:.2f}')
  axs[i].legend()


plt.tight_layout()
plt.show()
```
<img width="1490" height="490" alt="output3" src="https://github.com/user-attachments/assets/2a76443e-0908-4358-bf74-47b7ed9e642d" />

Result: A clear gap can be observed between the tipping behavior of dinner and lunch customers. Those who dine in the evening tend to tip more frequently, and the tip amounts are also higher compared to those who come for lunch.

### The difference in tip amounts between weekday and weekend meals

```python
# Filter data by day: weekend and weekdays
Weekend_tip = df[df.day.isin(['Sat', 'Sun'])]
Weekday_tip = df[~df.day.isin(['Sat', 'Sun'])]

# Compute basic statistics for weekend tips
Weekend_tip_min = weekend_tip.tip.min()
Weekend_tip_max = weekend_tip.tip.max()
Weekend_tip_mean = weekend_tip.tip.mean()
Weekend_tip_median = weekend_tip.tip.median()

# Compute basic statistics for weekday tips
Weekday_tip_min = weekday_tip.tip.min()
Weekday_tip_max = weekday_tip.tip.max()
Weekday_tip_mean = weekday_tip.tip.mean()
Weekday_tip_median = weekday_tip.tip.median()

# Prepare datasets for plotting
dataset2 = [weekend_tip, weekday_tip]
titles = ['Weekend Tips', 'Weekday Tips']
colors = ['#e9a3c9', '#a1d76a']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()

# Plot histograms for each dataset
for i in range(2):
  axs[i].hist(dataset2[i].tip, bins=15, color= colors[i], edgecolor='black', alpha=0.8)
  axs[i].set_xlabel('Tip value')
  axs[i].set_ylabel('Frequency')
  axs[i].set_title(titles[i])
  axs[i].grid(axis='x')  # Display grid lines along x-axis
  mean = dataset2[i].tip.mean()
  axs[i].axvline(mean, color='red', linestyle='--', label=f'Mean: {mean:.2f}') # Draw vertical line for overall mean
  axs[i].legend()

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plot
```

<img width="1490" height="490" alt="Weekeand" src="https://github.com/user-attachments/assets/f95baecb-8288-415b-a9cf-3fc4a8e0774e" />


Weekday:
The distribution is wider, but high tip values are less frequent compared to the weekend group.
Most tips range from $2 to $5, with none exceeding $6.
The average tip is $2.76, indicating that tip amounts on weekdays are generally lower than on weekends.
Overall, tipping activity is also less frequent during weekdays.

Weekend:
The distribution is slightly skewed, with most tips concentrated between $2 and $4.
There are several higher tips ranging from $6 to $10, showing that customers tend to give larger tips on weekends.
The average tip is $3.12, suggesting that customers are more generous during the weekend.

Overview:
Customers tend to tip more often and give higher tip amounts during weekends than on weekdays.
During weekdays, customers tip less frequently and with smaller amounts.
Although the difference in average tip value is not large (around $0.36), the trend clearly shows that tips are higher on weekends than on weekdays.

### The correlation between smokers, non-smokers, and tips

```python
# Filter data by smokers and non-smokers
Smokers_tip = df[df.smoker == 'Yes']
Non_smokers_tip = df[df.smoker == 'No']

# Compute basic statistics for smokers tips
Smokers_tip_min = Smokers_tip.tip.min()
Smokers_tip_max = Smokers_tip.tip.max()
Smokers_tip_mean = Smokers_tip.tip.mean()
Smokers_tip_median = Smokers_tip.tip.median()

# Compute basic statistics for non-smokers tips
Non_smokers_tip_min = Non_smokers_tip.tip.min()
Non_smokers_tip_min = Non_smokers_tip.tip.max()
Non_smokers_tip_mean = Non_smokers_tip.tip.mean()
Non_smokers_tip_median = Non_smokers_tip.tip.median()

# Prepare datasets for plotting
dataset3 = [Smokers_tip, Non_smokers_tip]
titles = ['Smokers Tips', 'Non-Smokers Tips']
colors = ['#2166ac', '#b2182b']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()
plt.style.use('seaborn-v0_8')

# Plot histograms for each dataset
for i in range(2):
    axs[i].hist(dataset3[i].tip, color= colors[i], edgecolor='black', alpha=0.8)
    axs[i].set_xlabel('Tip value')
    axs[i].set_ylabel('Frequency')
    axs[i].set_title(titles[i])
    axs[i].grid(axis='x')
    mean = dataset3[i].tip.mean()
    axs[i].axvline(mean, color='orange', linestyle='--', label=f'Mean: {mean:.2f}') # Draw vertical line for overall mean
    axs[i].legend()

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plot
```
<img width="1490" height="490" alt="Smokers!" src="https://github.com/user-attachments/assets/af8f6367-3fa3-438d-b4b8-3ca2bad40999" />



Smokers:
From the chart, it can be seen that the distribution is left-skewed, with most tip amounts ranging from $2 to $4.
There are a few higher values between $6 and $10, but they occur less frequently.
The average tip is around $3.
There are also a few small outliers (around $7–$10), indicating that some customers gave unusually high tips.

Non-Smokers:
The distribution is more concentrated around $2–$4, with fewer outliers compared to the smokers group.
It also shows a more symmetrical pattern.

Preliminary Conclusion:
Smokers tend to have more diverse tipping behavior (greater variation), while non-smokers tend to have more consistent tip amounts (around the average).
However, the average tip amount between the two groups is not significantly different.


## Summary
From the analysis, we can see clear patterns in tipping behavior. Most tips fall below $5, with the majority around $2–$4, and the maximum tip recorded is $10. Customers tend to tip more on weekends compared to weekdays, and Dinner generally receives higher tips than Lunch. Differences are also observed between smokers and non-smokers, as well as between genders, although average tip values are often similar. Overall, these insights provide a better understanding of customer tipping habits and can help guide service and business strategies.
