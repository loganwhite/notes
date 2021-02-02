## Using vscode to make replacements with regular expression.
Use `()` to quote the pattern out and use $1, $2 to indicate the pattern in the replace string.
e.g. ` ([0-9]*\.)` repalce with `\n$1.` The `()` points out the changing pattern.

## Using sublime to make batch replacement with regular expressions.
Use `()` to quote the pattern out and use $1, $2 to indicate the pattern in the replace string.
e.g. ` ([0-9]*\.)` repalce with `\n\1.` The `()` points out the changing pattern.

**NOTE** that in the replaced text, escape characters are needed. *e.g.*, replace `\ textbf {aaa}` with `\textbf{aaa}`. We can use the following code snippet.
```bash
# find
\\([\w]+)\{([.]+)\}
# replace
\\\1\{\2\}
```
