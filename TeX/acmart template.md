## How to remvoe the acm reference format in the template?
Add the following command after `\documentClass{}`
```latex
\settopmatter{printacmref=False}
```

## How to remove the acm copyright information?
Add the following command after `\documentClass{}`
```latex
\renewcommand\footnotetextcopyrightpermission[1]{}
```
