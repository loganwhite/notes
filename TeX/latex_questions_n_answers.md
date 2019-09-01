## When using a inline graphics, the image may be higher than thought. The following command can be leveraged to lower it.

```latex
\raisebox{-0.4\height}{\includegraphics[width=1em]{fig/fail_emoj}}
```

## How to insert a figure inside a caption in LaTeX?
Sometimes you want to place a figure inside a caption of either a table or a figure. There are two way to do that,
1. Place some other words in the small caption text field of the `\caption` command.
  ```latex
  \caption[short form]{Motivation examples demonstration. 
  \raisebox{-0.4\height}{\includegraphics[width=1em]{fig/fail_emoj}} represents the device is compromised}
  ```
  In this example, short form is shown in the table of figure (like the table of the content) and the text quoted with curly bracket is the one shown aside the table or figure.
  
2. Use `\protect` command to protect the image in the caption. However, this will makes the table of figure also contains the inserted figure.
  ```latex
  \caption{Motivation examples demonstration. 
  \raisebox{-0.4\height}{\protect\includegraphics[width=1em]{fig/fail_emoj}} represents the device is compromised}
  ```
## Defining macro in LaTeX (`\newcommand`)
Some times, when defining new commands with `\newcommand`, it act like macro where it just replace the newly defined command word with the one it stands for. E.g. If a new macro is defined like:
```latex
\newcommand{\probname}{Problem name}
```
Then in the text:
```latex
This is \probname ok?
```
after compilation will be like:

This is Problem nameok?

Note that the space after `\probname` is only used for identifying the command. If we want to add space between "name" and "ok", we have to add a space inside the `\probname` defination.
```latex
\newcommand{\probname}{Problem name }
```
**NOTE: using the above method may cause problems, the correct way is using `\␣`**

```latex
% Macro definition
\newcommand{\probname}{Problem name }
% Macro invokation
This is \probname\ ok?
```

## How to remove indent in the `enumerate` environment
use wide in the enumerate parameter list:
```latex
\begin{enumerate}[wide]
\item ...
\end{enumerate}
```


## Highlight text
Using the `soul` package to highlight the text. Generally, if using `\hl` command directly, the text will be underlined. If adding `color` package, the text will be highlighted with yellow in the background.
```latex
\documentclass{article}
\usepackage{color}
\usepackage{soul}
...
\begin{document}
...
\hl{This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text. This is the highlighted text.} This is normal text.This is normal text.This is normal text.This is normal text.This is normal text.This is normal text.This is normal text.
...
\end{document}
```
