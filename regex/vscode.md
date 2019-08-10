## Using vscode to make replacements with regular expression.
Use `()` to quote the pattern out and use $1, $2 to indicate the pattern in the replace string.
e.g. ` ([0-9]*\.)` repalce with `\n$1.` The `()` points out the changing pattern.
