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

Có một vài cột có types là Objects nên tôi tiến hành bước chuyển đổi type và check again
```python
df.df.convert_dtypes()
print(df.info())
```
### Basic Descriptive Statistics
```python
print(df.describe())
```

