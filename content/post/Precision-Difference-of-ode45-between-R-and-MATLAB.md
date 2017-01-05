+++
title = "Precision Difference of ode45 between R and MATLAB"
draft = false
date = "2014-11-24T23:44:46-04:00"
tags = ["ode45", "R", "MATLAB"]
categories = ["Numerical Computation"]
+++

`ode45` solvers from both `R` and `MATLAB` are used to run simulations of the [spread of Hong Kong flu in New York city](https://www.math.duke.edu//education/ccp/materials/diffcalc/sir/sir1.html) with different initial susceptible individuals $S\_0 =$ 790, 7900, 79000, 790000, and 7900000. The maximum infected people $I_{max}$ under each condition is recorded for comparison. The following codes draw a figure showing the precision difference of `ode45` solvers between `R` and `MATLAB`.

``` r
Imax <- data.frame(S0 = 79 * 10 ^ (1 : 5), 
                   R = c(57.12598, 504.9923, 4984.303, 49749.60, 497474.2), 
                   MATLAB = c(57.14987, 505.3385, 4987.447, 49774.26, 497552.2))
Imax$`MATLAB - R` <- Imax$MATLAB - Imax$R
# plot delta Imax VS. S0
Imax.plot <- melt(Imax, id = "S0", measure = c("R", "MATLAB", "MATLAB - R"))
library(scales)
p <- ggplot(Imax.plot, aes(x = S0, y = value, group = variable, color = variable))
p + geom_line() + 
  geom_point() + 
  scale_x_log10(name = expression(S[0]), breaks = 79 * 10 ^ (1 : 5), labels = comma) + 
  scale_y_continuous(name = "Imax", labels = comma) + 
  facet_wrap(~ variable, scales = "free") + 
  ggtitle("Precision Diffference of ode45 Solver between R and MATLAB") + 
  theme_bw(base_size = 14)
```

![](http://tonytsai.name/materials/RK4SIR_files/figure-html/unnamed-chunk-21-1.png) 

As shown in the right panel, the difference of $I_{max}$ increases with the initial susceptible number $S_0$ getting larger. When $S_0 < 8,000$, the difference is no more than 1. When $S_0$ is very large, the absolute precision difference also becomes larger; however, the relative accuracy keeps and it is accepted in this case.