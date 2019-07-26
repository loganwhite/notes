## Sometime the minor ticks doesn't shows.

The following code will help generate minor ticks in 'log' scale. NOTE that some case might not working.

```python3
locmaj = matplotlib.ticker.LogLocator(base=10,numticks=8) 
ax.xaxis.set_major_locator(locmaj)

# this statement is used to add some minor ticks.
ax.xaxis.set_minor_formatter(matplotlib.ticker.NullFormatter())
```
