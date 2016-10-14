+++
title = "One-Liners in Python - Linear Regression Plot"
draft = false
date = "2016-10-07"
+++

When I was showing a friend the advantages of python over Excel/LibreCalc, I remembered
the truly beautiful [Seaborn Library](https://stanford.edu/~mwaskom/software/seaborn/index.html) which produces plots like the following.

![plot](/images/2016-10-07-linearregression.png)

As one can see, this a standard scatterplot of a two dimensional dataset together with a regression line fitted to it - the light blue shade indicates a 95% confidence interval btw. It turns out that creating this standard diagram is as easy as

```python
import pandas as pd
import seaborn as sns
...
#create/load your pandas dataframe here
sns.regplot(x="your_dataframe_column1", y="your_dataframe_column2",
            data=your_pandas_dataframe)
```

Of course, you might need to have a [10 minute look at Pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html#min), as well.
