---
layout: post
title: Stratified Importance Sampling
categories: 
- R
tags:
- statistical computing
- variance reduction
- stratified importance sampling
---

The following `R` codes implement the **Example 5.13** in [Statistical Computing with R](http://www.amazon.com/Statistical-Computing-Chapman-Hall-Series/dp/1584885459), and compare the estimate $\hat{\theta}$ and $\hat{\sigma}$ from stratified importance sampling to the results from importance sampling. The example illustrates that stratification can reduce the varinace of importance sampling estimator.


```r
M <- 100000  # number of replicates
g <- function(x) {
  exp(-x - log(1 + x^2)) * (x > 0) * (x < 1)
}

# importance sampling
u <- runif(M)  # f3, inverse transform method
x <- -log(1 - u * (1 - exp(-1)))
fg <- g(x) / (exp(-x) / (1 - exp(-1)))
(theta.hat0 <- mean(fg))
```

```
## [1] 0.5253551
```

```r
(se0 <- sd(fg))
```

```
## [1] 0.09655589
```

```r
# stratified importance sampling
k <- 5  # number of strata
m <- M / k  # replicates per stratum
theta.hat <- se <- numeric(k)

for (j in 1:k) {
  u <- runif(m, (j - 1) / k, j / k)
  x <- -log(1 - u * (1 - exp(-1)))
  fg <- g(x) / (k * exp(-x) / (1 - exp(-1)))
  theta.hat[j] <- mean(fg)
  se[j] <- sd(fg)
}
sum(theta.hat)
```

```
## [1] 0.5248449
```

```r
sum(se)
```

```
## [1] 0.01832981
```

The estimate $\hat{\theta}$ is close, while the estimated standard error $\hat{\sigma}$ is reduced from 0.0965559 to 0.0183298 with stratification.