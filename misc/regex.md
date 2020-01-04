## How to remove highlights in latex source file?
The key to removing highlight `\hl{}` commands in latex source files is finding the curly brackets and remove it. We can use the following commend in VSCode.
```regex
\\hl\{([^}]*?)\}
```
The key part here is `[^}]*?` which match All characters except `}` as few characters as possible. (lazy quantifier)
