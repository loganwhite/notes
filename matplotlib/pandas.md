## Pandas `DataFrame` define

```python
import pandas as pd

tips = pd.DataFrame({
	'day':[1,2,3,4,1,3,4,6],\
	'total_bill':[2,3,4,4,5,6,7,6]
})
```
The output will be

```
   day  total_bill
0    1           2
1    2           3
2    3           4
3    4           4
4    1           5
5    3           6
6    4           7
7    6           6
```
