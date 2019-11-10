## Using seaborn will boost the char creation, but seaborn accept pandas DataFrame data type data.

```python
import seaborn as sns
import pandas as pd
sns.set(style="ticks", palette="pastel")

tips = pd.DataFrame({
	'day':[1,2,3,4,1,3,4,6],\
	'total_bill':[2,3,4,4,5,6,7,6]
})
print(tips)

# Draw a nested boxplot to show bills by day and time
sns_plot = sns.boxplot(y="day", x="total_bill", data=tips)	   

sns_plot.figure.savefig("output.pdf")
```
