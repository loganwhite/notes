## `add_subfig`
We can see a lot of codes which use `fig.add_subplot()` function with a numeric parameter passed into. the meaning of the parameter is:

These are subplot grid parameters encoded as a single integer. For example, "111" means "1x1 grid, first subplot" and "234" means "2x3 grid, 4th subplot".

Alternative form for `add_subplot(111)` is `add_subplot(1, 1, 1)`.


## `gca()`
Each figure has its subplots. For a figure without explicitly creating subplots, we can use `gca()` function to get the current Axes.
