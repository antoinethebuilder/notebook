---
description: This section covers a bunch of things related to pandas 1.1 +
---

# Working with Pandas

## Conditional Statements

### Boolean Indexing

#### Series

```python
In[1]: pd.Series(range(-3, 4))
In[2]: s[(s < -1) | (s > 0.5)]
Out[3]:
0   -3
1   -2
4    1
5    2
6    3
dtype: int64
```

#### Dataframes

You may select rows from a DataFrame using a boolean vector the same length as the DataFrame’s index

```text
In [1]: df[df['A'] > 0]
Out[2]: 
                   A         B         C         D   E   0
2000-01-01  0.469112 -0.282863 -1.509059 -1.135632 NaN NaN
2000-01-02  1.212112 -0.173215  0.119209 -1.044236 NaN NaN
2000-01-04  7.000000 -0.706771 -1.039575  0.271860 NaN NaN
2000-01-07  0.404705  0.577046 -1.715002 -1.039268 NaN NaN
```

{% hint style="info" %}
The operators are: `|` for `or`, `&` for `and`, and `~` for `not`.  
These **must** be grouped by using parentheses.
{% endhint %}

### Replace value based on multiple conditional statements

```python
df = pd.DataFrame({'Type':list('ABBC'), 'Set':list('ZZXY')})

conditions = [
    (df['Set'] == 'Z') & (df['Type'] == 'A'),
    (df['Set'] == 'Z') & (df['Type'] == 'B'),
    (df['Type'] == 'B')]

values = ['yellow', 'blue', 'purple']
df['color'] = np.select(conditions, values, default='black')
print(df)
```

