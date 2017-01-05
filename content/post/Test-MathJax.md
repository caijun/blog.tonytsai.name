+++
title = "Test MathJax"
draft = false
date = "2017-01-05T23:44:46-04:00"
tags = ["Matrix", "Multi-line Equations", "Hugo"]
categories = ["MathJax"]
+++

The first test case is to create a matrix
$$
\begin{matrix}
  1 & x & x^2 \\
  1 & y & y^2 \\
  1 & z & z^2 \\
\end{matrix}
$$

but all elemments of the matrix are displayed in one line. Fortunately, this can be solved by wraping the math expression inside a `<div> </div>` block, which is hinted by the hugo documents of [MathJax Support](https://gohugo.io/tutorials/mathjax/). besides, the blank line before `<div> </div>` block is necessary, otherwise the matrix would not be displayed correctly.

<div>
$$
\begin{matrix}
  1 & x & x^2 \\
  1 & y & y^2 \\
  1 & z & z^2 \\
\end{matrix}
$$
</div>

Actually, what I would like to do is to display multi-line equations.Taking the example from the stackoverflow question [Mathjax multi-line equation rendering issue] (http://stackoverflow.com/questions/18860693/mathjax-multi-line-equation-rendering-issue), I input following MathJax expressions.

$$
\begin{eqnarray} 
y &=& x^4 + 4      \nonumber \\
  &=& (x^2+2)^2 -4x^2 \nonumber \\
  &\le&(x^2+2)^2    \nonumber
\end{eqnarray} 
$$

As you can see, the equations are displayed in one line. From the comments of the question, the solution is to use `\\\\\\` instead of `\\` as newline command, which also can be applied to the test example of matrix. But I think typing 6 `\` is quite tedious. Is there any concise solution?

$$
\begin{eqnarray} 
y &=& x^4 + 4      \nonumber \\\\\\
  &=& (x^2+2)^2 -4x^2 \nonumber \\\\\\
  &\le&(x^2+2)^2    \nonumber
\end{eqnarray} 
$$

Alternative way to display matrix correctly is to use `\\\\\\` instead of `\\` as newline command.
$$
\begin{matrix}
  1 & x & x^2 \\\\\\
  1 & y & y^2 \\\\\\
  1 & z & z^2 \\\\\\
\end{matrix}
$$