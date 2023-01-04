---
title: "Derivation of Number of Infected Individuals I(a)"
author: ''
date: '2017-04-22'
categories: ['Influenza']
tags: []
bibliography: ["bibliography.bib"]
link-citations: yes
draft: false
output:
  md_document:
    toc: false
    preserve_yaml: true
---

([Hens et al. 2012, 63:41](#ref-Hens.etal-Modelinginfectiousdisease-2012)) gives the formula for the total number of infective individuals `\(I(a)\)`:

$$
\begin{equation}
  I(a) = \frac{\lambda}{\lambda - \nu}N(0)l(a)\left[e^{-\nu a} - e^{-\lambda a}\right],
  \tag{1}
\end{equation} 
$$

which is obtained by integrating following differential equation with respect to age `\(a\)`:

$$
\begin{equation}
  \frac{dI(a)}{da}=\lambda S(a) - (\nu + \mu)I(a).
  \tag{2}
\end{equation}
$$

This is also equation (4.11) in ([Anderson and May 1992, 67](#ref-Anderson.May-InfectiousDiseasesHumans-1992)). Here I will present the details of integration.

Equation (2) can be written as follows:

$$
\begin{equation}
  \frac{dI(a)}{da} + (\nu + \mu)I(a)=\lambda S(a).
  \tag{3}
\end{equation}
$$

This equation can be solved using the technique of “integrating factors”[^1] by following the steps below:

*Step 1*. Multiply both sides of the equation by `\(e^{(\nu + \mu)a}\)`, to obtain the following:

$$
\begin{equation}
  \frac{dI(a)}{da}e^{(\nu + \mu)a} + I(a)(\nu + \mu)e^{(\nu + \mu)a}=\lambda S(a)e^{(\nu + \mu)a}.
  \tag{4}
\end{equation}
$$

Note that, according to the rules of differentiation, the left-hand side of this equation is equivalent to the derivative of `\(I(a)e^{(\nu + \mu)a}\)` with respect to `\(a\)`, and so the equation can be rewritten as follows:

$$
\begin{equation}
  \frac{d}{da}(I(a)e^{(\nu + \mu)a})=\lambda S(a)e^{(\nu + \mu)a}.
  \tag{5}
\end{equation}
$$

*Step 2*. We now integrate both sides of equation (5) between 0 and `\(a\)` to obtain the following:

$$
\begin{equation}
  \int_{0}^{a}\frac{d}{da}(I(a)e^{(\nu + \mu)a})da = \int_{0}^{a}\lambda S(a)e^{(\nu + \mu)a}da.
  \tag{6}
\end{equation}
$$

Since integration is the converse of differentiation, the left-hand side of equation (6) simplifies to:

$$
\begin{equation}
  \left[I(a)e^{(\nu + \mu)a} \right]_{0}^{a} = I(a)e^{(\nu + \mu)a} - I(0)
  \tag{7}
\end{equation}
$$

However, `\(I(0)=0\)` (since no individuals are infected at birth) and therefore the left-hand side of equation (6) simplifies to `\(I(a)e^{(\nu + \mu)a}\)`.

Since here we are discussiing the static model (3.8) ([Hens et al. 2012, 63:38](#ref-Hens.etal-Modelinginfectiousdisease-2012)) with demographic processes ($\mu$ is the constant mortality rate), the survival function `\(l(a) = e^{-\mu a}\)` and `\(S(a) = N(0)l(a)e^{-\lambda a} = N(0)e^{-(\mu + \lambda)a}\)`. The right hand side of equation (6) can be writtern as following:

$$
\begin{equation}
  \int_{0}^{a}\lambda S(a)e^{(\nu + \mu)a}da = \lambda N(0)\int_{0}^{a}e^{(\nu - \lambda)a}da.
  \tag{8}
\end{equation}
$$

By the rules of integration, equation (8) simplifies to the following:

$$
\begin{equation}
\begin{split}
  \lambda N(0)\int_{0}^{a}e^{(\nu - \lambda)a}da &= \frac{\lambda N(0)}{\nu - \lambda}\left[e^{(\nu - \lambda)a} \right]_{0}^{a} \\
  &= \frac{\lambda}{\lambda - \nu}N(0)\left[1 - e^{(\nu - \lambda)a} \right].
\end{split}
\tag{9}
\end{equation}
$$

*Step 3*. Equating the expressions obtained from integrating the left-hand and right-hand sides of equation (6) leads to the following:

$$
\begin{equation}
  I(a)e^{(\nu + \mu)a} = \frac{\lambda}{\lambda - \nu}N(0)\left[1 - e^{(\nu - \lambda)a} \right].
  \tag{10}
\end{equation}
$$

Dividing both sides of this equation by `\(e^{(\nu + \mu)a}\)` leads to the intended equation (1):

$$
\begin{equation*}
\begin{split}
  I(a) &= \frac{\lambda}{\lambda - \nu}N(0)\left[e^{(-\nu -\mu)a} - e^{(-\lambda -\mu)a} \right] \\\\
  &= \frac{\lambda}{\lambda - \nu}N(0)e^{-\mu a}\left[e^{-\nu a} - e^{-\lambda a} \right] \\\\
  &= \frac{\lambda}{\lambda - \nu}N(0)l(a)\left[e^{-\nu a} - e^{-\lambda a} \right].
\end{split}
\end{equation*}
$$

</br>

### PS

- blank line before `$$` is necessary for correctly displaying numbered equation.

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Anderson.May-InfectiousDiseasesHumans-1992" class="csl-entry">

Anderson, Roy M., and Robert M. May. 1992. *Infectious Diseases of Humans: Dynamics and Control*. Oxford University Press.

</div>

<div id="ref-Hens.etal-Modelinginfectiousdisease-2012" class="csl-entry">

Hens, Niel, Ziv Shkedy, Marc Aerts, Christel Faes, Pierre Van Damme, and Philippe Beutels. 2012. *Modeling Infectious Disease Parameters Based on Serological and Social Contact Data: A Modern Statistical Perspective*. Vol. 63. Springer Science & Business Media.

</div>

</div>

[^1]: http://mathworld.wolfram.com/IntegratingFactor.html
