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
