# Bitly-Data-from-1.USA-project

#### It is a dataset that contains the name of the browser, city, timezone, country, and webpage visited by anonymous users who shorted links with .gov or .mil. In 2011.

- Browser's name
- city
- Timezone
- Country
- Web page visited

## what are the 10 time zones with the highest presence?

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

#### What are the time zones with the highest presence?

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
### Visualization
![image](https://github.com/EduardoJMR/Bitly-Data-from-1.USA-project/blob/master/images/Capture2.JPG)

































