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

## How to group reference numbers in the main body?
Add the following package.
```latex
\usepackage[noadjust]{cite}
```

## How to display check and cross

```latex
\usepackage{pifont}

% define check and cross with command
\newcommand{\cmark}{\ding{51}}%
\newcommand{\xmark}{\ding{55}}%
```

## `IEEETran` incompatible with `hyperref`
For some reasons, \maketitle gets redefined twice, and the second time \HyOrg@maketitle (what hyperref uses to store the old meaning of \maketitle) points to itself, thus creating a loop.

We can use the following trick to solve the problem.
```latex
\let\keptmaketitle\maketitle
\usepackage{hyperref}
\let\maketitle\keptmaketitle
```

## Using `aligned` environment can bring errors saying that `aligned` environment cannot be used outside math mode.
Error message: '\begin{aligned} allowed only in math mode.'

To solve this problem, we can simply put the `aligned` environment into a `math` environment
```latex
\begin{math}
\begin{aligned}
...
\end{aligned}
\end{math}
```

## How to represent matrix or vector transpose in LaTeX?
```latex
\mathbf{a} = \left\{a_1, a_2, \dots, a_n\right\}^\interca
```

## How to change the arabic reference number to the corresponding Chinese character when invoking \ref{} in a xeCJK Chinese document?
Use the code snippet
```latex
\usepackage{refcount}
\usepackage{CJKnumb}

......

\newcommand{\refch}[1]{第\CJKnumber{\getrefnumber{#1}}章}
```

## How to refer the name of a section or a chapter?
Use the `nameref` package
```latex
\usepackage{hyperref}
...
\section{Section Name}
\label{sec:example}
...
\nameref{sec:example} % Here will show "Section Name" after compilation.
...
```

## How to Change the algorithm name (reference name, e.g., Algorithm 1, Algorithm 2 to 算法 1, 算法 2), and change the Input Output？
```latex
\SetAlgorithmName{算法}{算法}{算法列表}
\begin{algorithm}[t]
 \caption{This is a algorithm caption.}
 \label{al:alg}
\SetAlgoVlined
\SetKwInput{KwIn}{输入}
\SetKwInput{KwOut}{输出}
\SetKw{Continue}{continue}

\KwIn{$A$}
\KwOut{$B$}
...
\end{algorithm}
```

## How to modify the default title page elements style?
Make modification similar to the code snippet below. NOTE that `\makeatletter` and `\makeatother` are required in the preamble.

```latex
\makeatletter
\newcommand\department[1]{\renewcommand\@department{#1}}
\newcommand\@department{}

\renewcommand{\maketitle}{\bgroup\setlength{\parindent}{0pt}
\begin{flushleft}

  {\vspace{18pt}\fontsize{16}{32}\selectfont\textbf{\@title}\par}
  \vspace{16pt}
  {\fontsize{14}{14}\selectfont\textbf{\@author}\par}
  \vspace{10pt}
  {\fontsize{12}{12}\setlength{\baselineskip}{24pt}\@department\par}
  \vspace{14pt}
\end{flushleft}\egroup
}
\makeatother
```

## How to reduce the unexpected vertical spaces between paragraphs?
Place `\parskip=0pt` followed by `\begin{documnet}`.

## How to reduce vertical spaces between subfloat rows?

Place a `[negativespace]` like `[-2cm]` at the end of subfloat in the current line.

```latex
\documentclass{article}
\usepackage{subfig}
\usepackage{graphicx}
\begin{document}
\begin{figure}[htb]
 \subfloat[Some figure]{\includegraphics[width=0.48\textwidth]{example-image-a}}\hfill
 \subfloat[Some other figure]{\includegraphics[width=0.48\textwidth]{example-image-b}}\\[-2cm]  %%<-- in this line
 \subfloat[Some more]{\includegraphics[width=0.48\textwidth]{example-image-c}}\hfill
 \subfloat[Some less]{\includegraphics[width=0.48\textwidth]{example-image}}
 \caption{There are example figures}
\end{figure}
\end{document}
```

## Reduce space between footnotes and the main text
Put `\setlength{\skip\footins}{5pt}` right behind `\begin{document}` or right before the footnote you want to reduce space. Note that all succeeding footnote-maintext spaces are reduced.

## How to change the number of the caption of an Algorithm listing?
Place `\setcounter{algocf}{1}` at the beginning of the algorithm environment.

## How to change font size in a group (ie, a small number of place)
```latex
% set fontsize to 22pt with line space 22pt
{\fontsize{16pt}{22pt}\selectfont Font Size Sixteen} vs normal font size.
```
## How to shrink the space between characters in xeCJK environment?
```latex
\xeCJKsetup{
  CJKglue={\hskip -0.08em plus 0.08\baselineskip}
  % default value is
  % CJKglue={\hskip 0pt plus 0.08\baselineskip}
}
```

## How to break line in a url?
Change the `url` package to `xurl`.


## How to change the font of url?
Internally `\url` uses `\UrlFont`. You can change it with `\urlstyle` (see the documentation of url in url.sty) or by redefining `\UrlFont`
```latex
\renewcommand\UrlFont{\rmfamily}
```

## How to use square brackets in the enumerate environment
```latex
\documentclass{article}
\usepackage{enumitem}% http://ctan.org/pkg/enumitem
\begin{document}
\begin{enumerate}[label={[\arabic*]}]
  \item First item
  \item Second item
  \item \ldots
  \item Last item
\end{enumerate}
\end{document}
```

## How to reduce spaces between words in math environment.
Use `\!` in the space you want to shrink.
```latex
$$w\!=\!a$$
```
## Limiting the number of authors in the references with IEEEtran
First place the following code snippet into the head of your `.bib` file
```bibtex
@IEEEtranBSTCTL{IEEEexample:BSTcontrol,
CTLuse_forced_etal       = "yes",
CTLmax_names_forced_etal = "3",
CTLnames_show_etal       = "2" }
```
This will limit author lists more than 3 by printing only the first 2 authors followed by et al.

Next, you should place `\bstctlcite{IEEEexample:BSTcontrol}` at the beginning of the `document` environment.

## Highlight commands using `\hl{}` results in errors.
Register the command using `\soulregister{}7`. For example:
```latex
\soulregister{\cite}7
\soulregister{\citep}7
\soulregister{\citet}7
\soulregister{\ref}7
\soulregister{\solution}7
\soulregister{\refs}7
```

## How to highlight section titles using `soul`.
Replace `\section{this is title}` with
```latex
\section{\texorpdfstring{\hl{this is section}}{this is section}}
\subsection{\texorpdfstring{\hl{this is subsection}}{this is subsection}}
```


## How to highlight the equation environment.
See  the following example.
```latex
\documentclass{article}
\usepackage{xcolor}
\usepackage{soul}
\newcommand{\mathcolorbox}[2]{\colorbox{#1}{$\displaystyle #2$}}

\begin{document}
For inline math, one can simply do \hl{colored $a=b$ math}.For display math, the following works:
\begin{equation}
\mathcolorbox{red}{y=\frac{x^2}{q}}+z
\end{equation}
\end{document} 
```

## How to adjust section title spaces for the `acmart` template?
The `titlesec` package has conflict behavior to the original `acmart` template, exhibiting missing of `.` in subsubsection titles. Hence, we should change the title behavior defined in the `acmart` template. Following are the latex codes and should be placed in preamble.
```latex
\makeatletter
    \renewcommand\section{\@startsection{section}{1}{\z@}%
      {-.1\baselineskip \@plus -.1\p@ \@minus -.1\p@}% % Space before section title
      {.1\baselineskip}% % Space after section title
      {\ACM@NRadjust\@secfont}}
    \renewcommand\subsection{\@startsection{subsection}{2}{\z@}%
      {-.1\baselineskip \@plus -.1\p@ \@minus -.1\p@}% % Space before subsection title
      {.1\baselineskip}% % Space after subsection title
      {\ACM@NRadjust\@subsecfont}}
    \renewcommand\subsubsection{\@startsection{subsubsection}{3}{10pt}%
  {-.1\baselineskip \@plus -.1\p@ \@minus -.1\p@}%
  {-3.5\p@}%
  {\@subsubsecfont\@adddotafter}}
\makeatother
```
