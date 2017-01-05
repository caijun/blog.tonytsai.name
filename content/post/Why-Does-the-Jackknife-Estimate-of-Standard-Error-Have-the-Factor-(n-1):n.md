+++
title = "Why Does the Jackknife Estimate of Standard Error Have the Factor (n-1)/n?"
draft = false
date = "2015-04-23T23:44:46-04:00"
tags = ["Jackknife", "Standard Error", "Resampling"]
categories = ["Statistical Computing"]
+++

Today I read **7.2 The Jackknife** in [Stastical Computing with R](http://www.amazon.com/Statistical-Computing-Chapman-Hall-Series/dp/1584885459) and found the explanation for why the jackknife estimate of standard error have the factor $(n-1)/n$ is unclear. I refered to [An Introduction to the Bootstrap](http://www.amazon.com/Introduction-Bootstrap-Monographs-Statistics-Probability/dp/0412042312) by Bradley Efron and R. J. Tibshirani, and the [slides](https://www.scss.tcd.ie/Rozenn.Dahyot/453Bootstrap/04_Jackknife.pdf) of jackknife by Rozenn Dahyot to figure out the reason. Here is my understanding for the existence of factor $(n-1)/n$.

The jackknife samples are computed by leaving out one observation $x\_i$ from a sample $\mathbf{x} = (x\_1, x\_2, \cdots, x\_n)$ at a time:
$$
\mathbf{x}\_{(i)} = (x\_1, x\_2, \cdots, x\_{i-1}, x\_{i+1}, \cdots, x\_n)
$$
for $i = 1, 2, \cdots, n$. The $i$th jackknife replication $\hat{\theta}\_{(i)}$ of the statistic $\hat{\theta} = s(\mathbf{x})$ is
$$
\hat{\theta}\_{(i)} = s(\mathbf{x}\_{(i)}), \forall i = 1, 2, \cdots, n.
$$
Thus, for $\hat{\theta} = \bar{x}$, we have
$$
\begin{equation} \nonumber
\begin{aligned}
s(\mathbf{x}\_{(i)}) &= \bar{x}\_{(i)} \\\\\\
                    &= \frac{1}{n-1}\sum\_{j \ne i}{x_j} \\\\\\
                    &= \frac{n\bar{x} - x_i}{n-1}
\end{aligned}
\end{equation}
$$

The jackknife estimate of standard error is defined by
$$
\hat{se}\_{jack} = [\frac{n-1}{n}\sum\_{i=1}^n{(\hat{\theta}\_{(i)} - \hat{\theta}\_{(\cdot)})^2}]^{1/2}
$$
where $\hat{\theta}\_{(\cdot)} = \frac{1}{n}\sum\_{i=1}^n{\hat{\theta}_{(i)}}$.

The exact form of the factor $(n-1)/n$ in the above formula is derived by considering the special case $\hat{\theta} = \bar{x}$. For $\hat{\theta} = \bar{x}$, it is easy to show that
$$
\begin{equation} \nonumber
\begin{aligned}
\bar{x}\_{(\cdot)} &= \frac{1}{n}\sum\_{i=1}^n{\bar{x}\_{(i)}} \\\\\\
                  &= \frac{1}{n}\sum\_{i=1}^n{\frac{n\bar{x} - x\_i}{n-1}} \\\\\\
                  &= \frac{n}{n-1}\bar{x} - \frac{1}{n-1}\frac{1}{n}\sum\_{i=1}^n{x\_i} \\\\\\
                  &= \frac{n}{n-1}\bar{x} - \frac{1}{n-1}\bar{x} \\\\\\
                  &= \bar{x}
\end{aligned}
\end{equation}
$$
Therefore, 
$$
\begin{equation} \nonumber
\begin{aligned}
\hat{se}\_{jack} &= [\frac{n-1}{n}\sum\_{i=1}^n{(\hat{\theta}\_{(i)} - \hat{\theta}\_{(\cdot)})^2}]^{1/2} \\\\\\
                &= [\frac{n-1}{n}\sum\_{i=1}^n{(\bar{x}\_{(i)} - \bar{x}\_{(\cdot)})^2}]^{1/2} \\\\\\
                &= [\frac{n-1}{n}\sum\_{i=1}^n{(\frac{n\bar{x} - x\_i}{n-1} - \bar{x})^2}]^{1/2} \\\\\\
                &= [\frac{1}{n(n-1)}\sum\_{i=1}^n{(x\_i - \bar{x})^2}]^{1/2} \\\\\\
                &= \frac{[\frac{1}{n-1}\sum\_{i=1}^n{(x\_i - \bar{x})^2}]^{1/2}}{\sqrt{n}} \\\\\\
                &= \frac{\hat{\sigma}}{\sqrt{n}} \\\\\\
                &= \hat{se}(\bar{x})
\end{aligned}
\end{equation}
$$
where $\hat{se}(\bar{x})$ is the unbiased estimate of the standard error of the sample mean, and $\hat{\sigma}$ is unbiased estimate of the standard deviation of the population.

The derivation suggests that the factor $(n-1)/n$ os exactly what is needed to make $\hat{se}\_{jack}$ equal to the unbiased estimate of the standard error of the sample mean $\hat{se}(\bar{x})$. It is a somewhat arbitrary convention that $\hat{se}_{jack}$ uses the factor $(n-1)/n$, as in fact, the factor $(n-1)/n$ is derived by considering the special case $\hat{\theta} = \bar{x}$.
