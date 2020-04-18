The most easiest way to put the legent outside the plot is use the `bbox_to_anchor=(0.2, 1)`, shown as follows.

```python3
plt.gca().legend(bbox_to_anchor=(0.2, 1),ncol=5, title=None)
plt.tight_layout(True)
```

NOTE that `plt.tight_layout(True)` have to be applied after use `bbox_to_anchor`. It's a good habbit to always put `plt.tight_layout(True)` at the very end.
