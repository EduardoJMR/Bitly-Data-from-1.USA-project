# Bitly-Data-from-1.USA-project

#### It is a dataset that contains the name of the browser, city, timezone, country, and webpage visited by anonymous users who shorted links with .gov or .mil. In 2011.

- Browser's name
- city
- Timezone
- Country
- Web page visited

## What are the 10 time zones with the highest presence?

### Transforming the data

#### We decided to count the number of different time zones we have, using a function.

 ```python
def get_counts(sequence):
    counts = {}
    for x in sequence:
        if x in counts:
            counts[x]+=1
        else:
            counts[x]=1
    return counts 

## What are the time zones with the highest presence?

```python
from collections import defaultdict

def get_counts2(sequence):
    counts= defaultdict(int) #values will initiaze to 0
    for x in sequence:
        counts[x] += 1
    return counts
get_counts2(time_zones)

from collections import Counter
counts= Counter(time_zones)
counts.most_common(10)

from collections import Counter
counts= Counter(time_zones)
counts.most_common(10)
tz_counts = frame["tz"].value_counts()
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture.JPG)
```

#### Once the database has been converted to dataframe, knowing the number of times data from different time zones is even easier.

```python
frame = pd.DataFrame(records)
frame["tz"]

tz_counts = frame["tz"].value_counts()
```

### Cleaning the data

#### It can be seen that among the records, some of them do not have data on the time zone, being Nan. This means that we can clean the data by converting these missing values as "Unknow".

```python
clean_tz = frame["tz"].fillna("Missing")
clean_tz[clean_tz == ""] = "Unknown"
tz_counts=clean_tz.value_counts()

import seaborn as sns
subset = tz_counts.head()
sns.barplot(y=subset.index, x=subset.to_numpy())
```
### Visualizing the data

### Top time zones in the 1.usa.gov sample data

![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture2.JPG)

## What are the top 5 browsers more used?

### Cleaning the data

#### Parsing all of the interesting information in these “agent” strings may seem like a daunting task. One possible strategy is to split off the first token in the string (corresponding roughly to the browser capability) and make another summary of the user behavior:

```python
results= pd.Series([x.split()[0] for x in frame["a"].dropna()])
results.head(5)
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture4.JPG)
```python
results.value_counts().head(8)
```
### Visualizing the data

#### Top 5 browsers more used.

```python
import seaborn as sns
subset2 = results.value_counts().head()
sns.barplot(y=subset2.index, x=subset2.to_numpy())
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture5.JPG)

## Top time zones by Windows and non-Windows users?

### Transforming the data

#### We have noticed that another way to differentiate users is to see which ones use windows and which ones do not. To do this from column "a" we will look in each cell for the word windows.

```python
cframe= frame[frame["a"].notna()].copy()
cframe
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture3.JPG)

```python
cframe["os"]= np.where(cframe["a"].str.contains("Windows"), "Windows", "Not Windows")
cframe["os"].head(5)
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture6.JPG)

```python
by_tz_os = cframe.groupby(["tz" , "os"])
agg_counts= by_tz_os.size().unstack().fillna(0)
agg_counts.head(20)
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture7.JPG)

#### Once we knew the number of users who use windows by time zone, we decided to plot the ten time zones with the highest user traffic sorted by those who use windows and those who do not.
#### To do this, we first create an index to regroup the agg_cpunts table. This 'index' will take into account the total number of users using the sum of both windows and non-windows, by time zone, . sum("columns"), will reorder them according to their index using argsort().

```python
indexer= agg_counts.sum("columns").argsort()
count_subset = agg_counts.take(indexer[-10:])
count_subset
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture8.JPG)

```python
count_subset = count_subset.stack()
count_subset.name = "total"
count_subset = count_subset.reset_index()
count_subset.head(10)
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture9.JPG)

### Visualizing the data

#### Top time zones by Windows and non-Windows users

```python
sns.barplot(x="total",y="tz",hue="os", data= count_subset)
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture10.JPG)

### Transforming the data

#### Once again, once the number of users using windows according to time zone is known, it is recommended to normalise the data when plotting the graphs, obtaining in this case the proportion of users per time zone.

```python
def norm_total(group):
    group["normed_total"] = group["total"]/group["total"].sum()
    return group
results= count_subset.groupby("tz").apply(norm_total)
results
```
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture11.JPG)

```python
g= count_subset.groupby("tz")
result2= count_subset["total"]/g["total"].transform("sum")
count_subset["normed_total_2"]=result2

sns.barplot(x="normed_total", y="tz", hue="os", data=results)
```

### Visualizing the data

#### Top time zones by Windows and non-Windows users normalized 

![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture12.JPG)






