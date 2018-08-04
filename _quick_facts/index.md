---
weight: 2
---

To suppress the index column when reading in a new .csv, try setting the index_col parameter to 0, like this:

```Python
pd.read_csv('./file.csv', index_col=0)
```
