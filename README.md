# Restaurant Tips Analysis

This project aims to use the restaurant tips dataset to practice creating composition plots and visualizations. We will examine the relationship between different variables and the tips given.

The dataset consists of information from 244 restaurant bills, collected in the US in 1987.

It includes details about the tips given to restaurant staff, such as the total bill, tip amount, gender of the person paying, smoking status, day of the week, time of day, and party size.

# First Step
## Data Import

Trước khi import data, tôi sẽ tiến hành import thư việc Pandas và thư viện Matplotlib để phục vụ cho việc visualization thuận tiện cho phân tích

```python
import pandas AS pd
import matplotlib.pyplot AS plt
```
Then load data from the following link: https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv

```python
url = 'https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv'
df = pd.read_csv(url)
```
Sau khi import data thì chúng ta sẽ tiến hành khám phá dữ liệu

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
Trước khi chuyển đổi types

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

Có một vài cột có types là Objects nên tôi tiến hành chuyển đổi types và check again
```python
df = df.convert_dtypes()
print(df.info())
```
Kết quả sau khi chuyển đổi types của một vài cột trong data gốc

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

Từ các thông tin trên, tôi nhận thấy có thể tiếp tục tìm hiểu sâu hơn về các thông tin như smoker, time, size, tips, sex...
Let's see chúng ta có gì nào

## Analysis
Let's try to calculate measures of central tendency for the whole dataset first.
```python
#Define Values
common_tip_min = df.tip.min()
common_tip_max = df.tip.max()
common_tip_mean = df.tip.mean()
common_tip_median = df.tip.median()

# Make a list of values
common_values = [common_tip_min, common_tip_max, common_tip_mean, common_tip_median]
# Round all the values to 4 decimal places
common_values = map(lambda x: round(x, 4), common_values)

# Make a dataframe from the list
common_mct = pd.DataFrame(common_values, index=['min', 'max', 'mean', 'median'])
# Output the dataframe
print(common_mct)
```
|          |     0   |
|----------|--------:|
| **min**    | 1.0000  |
| **max**    | 10.0000 |
| mean     | 2.9983  |
| median   | 2.9000  |

Bên trên là whole dataset dưới dạng bảng, chúng ta sẽ đưa nó về dạng đồ thị histogram để có cái nhìn tổng quát hơn

```python
df.tip.hist(color= '#FF4500', bins=15, figsize=(15, 5), edgecolor='black', alpha=0.8)
plt.xlabel('Tip value')
plt.ylabel('Frequency')
plt.title('Whole dataset tip values')
plt.grid(axis='x')
plt.show()
```
<img width="1227" height="468" alt="output4" src="https://github.com/user-attachments/assets/98a3f30e-eb32-4ab2-a45a-f1fefcab8f4e" />

Có thể thấy rõ rằng phần lớn tiền tips nằm trong khoảng dưới $5, giá trị tip cao nhất là $10. Khách hàng chủ yếu tips $2 - $4 là nhiều nhất. 

### Sự khác biệt về giới tính

```python
#Tạo DataFrame
male_tip = df[df.sex == 'Male']
female_tip = df[df.sex == 'Female']

male_tip_min = male_tip.tip.min()
male_tip_max = male_tip.tip.max()
male_tip_mean = male_tip.tip.mean()
male_tip_median = male_tip.tip.median()

female_tip_min = female_tip.tip.min()
female_tip_max = female_tip.tip.max()
female_tip_mean = female_tip.tip.mean()
female_tip_median = female_tip.tip.median()

dataset = [male_tip, female_tip]
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
  axs[i].axvline(mean, color='red', linestyle='--', label=f'Mean: {mean:.2f}')
  axs[i].legend()


plt.tight_layout()
plt.show()
```
<img width="1490" height="490" alt="output" src="https://github.com/user-attachments/assets/6a5a0f66-8c44-4f9a-83b4-54a397743e32" />

Result: When comparing tipping behavior between males and females, the results show that males tend to tip more frequently than females. Additionally, the tip amounts given by males are generally higher than those given by females. This trend clearly highlights that males are significantly more likely to tip, and to tip higher amounts, compared to females.

### Sự khác biệt về tiền tips giữa Lunch và Dinner
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

dataset2 = [Lunch_tip, Dinner_tip]
titles = ['Lunch Tips', 'Dinner Tips']
colors = ['#b40426', '#3b4cc0']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()
plt.style.use('seaborn-v0_8')


# Make a Histogram
for i in range(2):
  axs[i].hist(dataset2[i].tip, color= colors[i], edgecolor='black', alpha=0.8)
  axs[i].set_xlabel('Tip value')
  axs[i].set_ylabel('Frequency')
  axs[i].set_title(titles[i])
  axs[i].grid(axis='x')
  axs[i].axvline(mean, color='black', linestyle='--', label=f'Mean: {mean:.2f}')
  axs[i].legend()


plt.tight_layout()
plt.show()
```
<img width="1490" height="490" alt="output3" src="https://github.com/user-attachments/assets/2a76443e-0908-4358-bf74-47b7ed9e642d" />

Result: A clear gap can be observed between the tipping behavior of dinner and lunch customers. Those who dine in the evening tend to tip more frequently, and the tip amounts are also higher compared to those who come for lunch.

### Sự khác biệt giữa tiền tip trong các bữa ăn weekday và weekend

```python
# Filter data by day: weekend and weekdays
weekend_tip = df[df.day.isin(['Sat', 'Sun'])]
weekday_tip = df[~df.day.isin(['Sat', 'Sun'])]

# Compute basic statistics for weekend tips
weekend_tip_min = weekend_tip.tip.min()
weekend_tip_max = weekend_tip.tip.max()
weekend_tip_mean = weekend_tip.tip.mean()
weekend_tip_median = weekend_tip.tip.median()

# Compute basic statistics for weekday tips
weekday_tip_min = weekday_tip.tip.min()
weekday_tip_max = weekday_tip.tip.max()
weekday_tip_mean = weekday_tip.tip.mean()
weekday_tip_median = weekday_tip.tip.median()

# Prepare datasets for plotting
dataset1 = [weekend_tip, weekday_tip]
titles = ['Weekend Tips', 'Weekday Tips']
colors = ['#e9a3c9', '#a1d76a']
fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)
mean = df.tip.mean()

# Plot histograms for each dataset
for i in range(2):
  axs[i].hist(dataset1[i].tip, bins=15, color= colors[i], edgecolor='black', alpha=0.8)
  axs[i].set_xlabel('Tip value')
  axs[i].set_ylabel('Frequency')
  axs[i].set_title(titles[i])
  axs[i].grid(axis='x')  # Display grid lines along x-axis
  axs[i].axvline(mean, color='red', linestyle='--', label=f'Mean: {mean:.2f}') # Draw vertical line for overall mean
  axs[i].legend()

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plot
```


<img width="1490" height="490" alt="output5" src="https://github.com/user-attachments/assets/3a261943-7595-4c47-b343-9383816b10b2" />

Result:
Kết quả hiển thị rõ rằng xu hướng tips của khách hàng sử dụng dịch vụ của nhà hàng vào weekend cao hơn weekday, giá trị tip cao nhất của weekend là $10 trong khi weekay chỉ có $6. 
Từ đó có thể tạm thời kết luận là khách hàng đến vào cuối tuần thường xuyên tip hơn và giá trị tip cũng cao hơn khách hàng weekday.

### Mối tương quan giữa smokers và non-smokers và tips

```python
# Filter data by smokers and non-smokers
smokers_tip = df[df.smoker == 'Yes']
non_smokers_tip = df[df.smoker == 'No']

# Compute basic statistics for smokers tips
smokers_tip_min = smokers_tip.tip.min()
smokers_tip_max = smokers_tip.tip.max()
smokers_tip_mean = smokers_tip.tip.mean()
smokers_tip_median = smokers_tip.tip.median()

# Compute basic statistics for non-smokers tips
non_smokers_tip_min = non_smokers_tip.tip.min()
non_smokers_tip_max = non_smokers_tip.tip.max()
non_smokers_tip_mean = non_smokers_tip.tip.mean()
non_smokers_tip_median = non_smokers_tip.tip.median()

# Prepare datasets for plotting
dataset3 = [smokers_tip, non_smokers_tip]
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
    axs[i].axvline(mean, color='orange', linestyle='--', label=f'Mean: {mean:.2f}') # Draw vertical line for overall mean
    axs[i].legend()

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plot
```
Result:
Nhóm non-smokers 

