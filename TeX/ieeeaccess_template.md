## NOTE: IEEE ACCESS template does not support Tikz package. In this case, avoid using Tikz in all LaTeX projects.

## The bug of caption does not wrap in wide figures (cross column figures)
1. In the `ieeeaccess.cls` file, find the code block that defines IEEE Access style Caption.
2. Find the comment:
  ```tex
  % if caption is longer than a line, let it wrap around
  ```
  Comment all the statements that process the wide figure.
  ```tex
  \@IEEEfigurecaptionsepspace% V1.6 was a hard coded 5pt
  % 3/2001 use footnotesize, not small; use two nonbreaking spaces, not one
  \setbox\@tempboxa\hbox{\strut{\color{accessblue}\figcapheadfont #1. \ }\raggedright\figcapfont#2\strut}%
  %\ifdim \wd\@tempboxa >\columnwidth%
  %\ifdim \xfigwd >\columnwidth%
  %% if caption is longer than a line, let it wrap around
  %\setbox\@tempboxa\hbox{{\color{accessblue}\figcapheadfont #1. \ }}%
  %\mbox{}\hfill\begin{tabular}{@{}l@{}}\noindent\raggedright\unhbox\@tempboxa\figcapfont#2\end{tabular}\hfill\mbox{}%
  %% if caption is shorter than a line,
  %% allow user to control short figure caption justification (left or center)
  %\else% wendi
  %\ifcenterfigcaptions \hbox to\hsize{\footnotesize\hfil\box\@tempboxa\hfil}%
  %\else 
  \setbox\@tempboxa\hbox{{\color{accessblue}\figcapheadfont #1. \ }}%
  %\parbox[t]{\columnwidth}
  {\vss\raggedright\noindent\unhbox\@tempboxa\figcapfont#2\vss}%
  %\hbox to\hsize{\box\@tempboxa\hfil}%
  %\fi
  %\fiwendi
  ```
