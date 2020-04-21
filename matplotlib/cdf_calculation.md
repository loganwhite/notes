Calculating CDF using the following code snnipet:

```python3
def generate_cdf_xy(data):
    """
        data: a 1 dementional list
        return: x, y list.
    """
    x = sorted(data)
    len_data = len(x)
    
    y = []
    for i in range(len_data):
        y.append(float(i) / float(len_data))

    # lengthen the line
    scale = x[-1] - x[0]
    x_extrapoint = scale * 0.2 + x[-1]
    y_extrapoint = 1.0
    x.append(x_extrapoint)
    y.append(y_extrapoint)
    return x, y
```
