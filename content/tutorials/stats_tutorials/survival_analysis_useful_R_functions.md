---
title: "Useful R Functions"
linktitle: "Useful R Functions"
summary:
date: 2020-05-24T19:45:46-04:00
lastmod: 2020-05-24T19:45:46-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 10
menu:
  stats_tutorials:
    # name: "Useful Stuff"
    parent: Survival Analysis
    weight: 10
---

<!--
1. replace dirdir with page dir (e.g. r_resources)
2. replace ww with weight, or leave blank to sort pages alphabetically
-->

This is a Coursera course offered by Imperial College London on Survival Analysis using R.

## survival pkg commands
```r
library(survival)

fit = survfit(Surv(fu_time, death) ~ gender)
plot(fit, xlab = "time", ylab = "survival probability")

survdiff(Surv(fu_time, death) ~ gender, rho = 0)

# define significant vars as vector
# use formula as input for coxph()
fmla <- as.formula(paste("Surv(fu_time, death) ~ ", paste(vars_sig, collapse=" + "), sep = ""))
cox_vars_sig <- coxph(fmla, data = g2)
summary(cox_vars_sig)
```

## survminer pkg commands
- pretty graphs for KM plots and residual plots
```r
library(survminer)

fit = coxph(...)
zph = cox.zph(fit, transform = "km")
plot(zph)
OR
ggcoxzph(zph) # time transformed Schoenfeld residuals (PH assumption)

# influential data points
ggcoxdiagnostics(fit, type = "dfbeta", linear.prediction = FALSE, ggtheme = theme_bw())

# residual vs covariates
ggcoxdiagnostics(fit, type = "deviance", linear.prediction = FALSE, ggtheme = theme_bw())

# Martingale residuals (linear relationship btw continuous vars and outcome)
ggcoxfunctional(Surv(fu_time, death) ~ age + log(age) + sqrt(age), data = g)

```


## EOF