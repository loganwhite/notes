## Converting Matplotlib codes to LaTeX Tikz code.

1. Install the `matplotlib2tikz` python library

```bash
$ pip install matplotlib2tikz               # if default python environment is python3
$ python3 -m pip install matplotlib2tikz    # if default python env is python2
```


2. Simply add the following code before showing or generating the pdf output of the figure:

```python3
import matplotlib2tikz
matplotlib2tikz.save("test.tex")
```

3. Paste the generated code to LaTeX source code. **NOTE** that the following code snippets should be added in the main `.tex` file.

```tex
\usepackage[utf8]{inputenc}
\usepackage{fontspec}  % optional
\usepackage{pgfplots}
\pgfplotsset{compat=newest}
\usepgfplotslibrary{groupplots}
\usepgfplotslibrary{dateplot}
```
