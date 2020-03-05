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



## How to highlight enumerate label and itemmized label
### enumerate
1. Defining the following command
```latex
\newcommand{\hlitem}{\stepcounter{enumi}\item[\hl{\theenumi}]}
```
Note that `\hl` is from `soul` package.

2. Using `\hlitem` in the enumerate rather than `item`. 
### itemized
1. Define
```latex
\newcommand{\hlitmizeditem}{\item[\hl{\labelitemi}]}
```
2. use `\hlitmizeditem` to replacd `\item`.

## Label styles of `enumerate` and `itemmize` environment
### `enumerate`
```latex
...
\usepackage{enumitem}
...
%Roman numbers
\begin{enumerate}[label=(\roman*)]
%...
% Arabic numbers
\begin{enumerate}[label=\arabic*)]
%...
% Alphabetical
\begin{enumerate}[label=\alph*)]
%...
```
### `itemize`
```latex
% choose whatever you like
\begin{itemize}[label=$\ast$]
```

## highlight with `soul` failed to highlight citations.
Try using `{}` to surround the `\cite{}` command or using `\mbox{}` to surround `\cite{}`.

## How to remove Reference section title
```latex
\renewcommand\refname{\vskip -1.5em}
```

## Quotation mark in LaTeX
We cannot use quotation mark directly from the keyboard. We should use the following symbols.
```latex
``quote''
```

## IEEETrans Template sometimes failed to meet the margin requirement in edas system.
e.g. top margin is less than the requriement, we can simply add more margins
```latex
\addtolength{\topmargin}{+0.1cm}
```

## How to eliminate the vertical space below algorithm
Add the following code snippet above `\begin{algorithm}[t]`.
```latex
\setlength{\textfloatsep}{0pt}
\begin{algorithm}[ht]
...
\end{algorithm}
```

## How to change the vertical space before or after section heading
```latex
% \titlespacing{command}{left spacing}{before spacing}{after spacing}[right]
% spacing: how to read {12pt plus 4pt minus 2pt}
%           12pt is what we would like the spacing to be
%           plus 4pt means that TeX can stretch it by at most 4pt
%           minus 2pt means that TeX can shrink it by at most 2pt
%       This is one example of the concept of, 'glue', in TeX

\usepackage{titlesec}

\titlespacing*{\section}{0pt}{1pt plus 1pt minus 1pt}{0pt plus 1pt minus 1pt}
\titlespacing*{\subsection}{0pt}{1pt plus 1pt minus 1pt}{0pt plus 1pt minus 1pt}
\titlespacing*{\subsubsection}{0pt}{1pt plus 1pt minus 1pt}{0pt plus 1pt minus 1pt}
```
Please note that `*` is used to guarantee the first paragraph do not have indentation.

## Removing vertical spaces before `enumerate` or `itemized` environment
use `topsep=0pt, noitemsep` options. as follows
```latex
relevant. 
\begin{enumerate}[wide, topsep=0pt, noitemsep,label=\textbf{(\arabic*)}]
\item ...
\end{enumerate}
```
## Avoid text wrap when using `\ref{}` and define reference command.
Sometimes, when refering figures, tables, and etc. in the text, we tend to use `Figure \ref{fig:fig1}`. However, this should be replaced by `Figure~\ref{fig:fig1}`. Because the TeX system will avoid wrap text if using the latter form. 

Another good practice is define our own-defined commands which help us quick adjust to the required format. E.g., some template requires Fig. rather than Figure we can simply modify our newly defined command rather than modify everywhere in the main text. Follows are the useful reference definitions.
```latex
\newcommand{\refs}[1]{Section~\ref{#1}}
\newcommand{\reff}[1]{Figure~\ref{#1}}
\newcommand{\reft}[1]{Table~\ref{#1}}
\newcommand{\refe}[1]{Equation~\ref{#1}}
\newcommand{\refa}[1]{Algorithm~\ref{#1}}
```

## How to remove page numbers on all pages?
Add `\pagenumbering{gobble}`.


## How to insert code to in LaTeX?

I firstly used `listings` to insert code sneppet. But I did dislike the default font used by the listing package. Back then, I did not know that the font can be changed. So I switched to `verbatim`, but `verbatim` is a simple dull tt font display, without any bold or emphsize on keywords, which does not satisfy me.

I researched the question online and got an answer that the default font of `listings` package can be changed. So I switch back to `listings`. Follows are the minimum working environment of `listings`.

```latex
\usepackage{listings}
\usepackage{courier} %% This package is used to change the default font to the courier font.

% listing settings, these setting works for my demands.
% Note that xleftmargin is used to solve the problem that 
% listing is too wide to expand outside the paragraph width (column width).
\lstset{basicstyle=\footnotesize\ttfamily,breaklines=true}
\lstset{language=C++,frame=single,numbers=left,xleftmargin=2em}  
\lstset{morekeywords={@decor,Packet,Interface,NATState,uint8_t}}

% floating behavior, caption, and label should be set up at the configuration list
\begin{lstlisting}[float={t},caption={Vanilla NAT implementation using \name{}.},label={list:gover_nat},captionpos=b]
// nat.gov
#include <stdio.h>
// main function
int main() {
  printf("Hello World!\n");
  return 0;
}
\end{lstlisting}


```
