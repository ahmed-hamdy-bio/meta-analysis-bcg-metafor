# Meta-analysis of BCG vaccine trials with metafor

Reproducible meta-analysis in R: effect sizes, random-effects model, forest and funnel
plots, heterogeneity, publication-bias tests, leave-one-out sensitivity analysis and
meta-regression.

**Question:** does BCG vaccination reduce the risk of tuberculosis, and does the effect differ between studies?

**Data:** the classic `dat.bcg` dataset from the `metafor` package (Colditz et al. 1994): 13 trials.
It is a well-known example dataset, so the project demonstrates the statistical workflow, not a new finding.
It covers the analysis step only, not literature search, screening or risk-of-bias assessment.

## Pipeline
1. Log risk ratios and sampling variances (`escalc`)
2. Random-effects model with REML (`rma`)
3. Forest plot
4. Heterogeneity: tau2, I2, Q, prediction interval
5. Publication bias: funnel plot, Egger-type test, trim-and-fill
6. Leave-one-out sensitivity analysis
7. Meta-regression on absolute latitude, and allocation method as a categorical moderator

## Run it
```r
install.packages(c("metafor", "rmarkdown"))
rmarkdown::render("analysis.Rmd")
```
The data comes with the package, so nothing needs to be downloaded.

## Results
- **Pooled effect (random-effects, REML, 13 trials):** risk ratio 0.49 (95% CI 0.34 to 0.70, p < 0.0001), meaning about half the tuberculosis risk among the vaccinated. The 95% prediction interval is wide (0.15 to 1.55), so the effect in a new setting could be small or null.
- **Heterogeneity:** very high. tau2 = 0.31, I2 = 92.2%, Q(12) = 152.2, p < 0.0001.
- **Publication bias:** no evidence of funnel-plot asymmetry (Egger-type test p = 0.42). Trim-and-fill added one study and moved the pooled risk ratio to about 0.52 (95% CI 0.36 to 0.74).
- **Sensitivity (leave-one-out):** the pooled risk ratio stayed between 0.45 and 0.53 whichever trial was dropped.
- **Moderators:** absolute latitude explained much of the heterogeneity (coefficient -0.029 log risk ratio per degree, p < 0.0001, R2 = 75.6%); the effect is stronger at higher latitudes. Residual heterogeneity remained (I2 = 68.4%). Allocation method was not significant (p = 0.41).

The full code, plots and session information are in `analysis.Rmd` and the rendered `analysis.html`.

## Limitations
- Only 13 trials, so heterogeneity, bias and moderator tests have limited power, and funnel-plot methods are unreliable with so few studies.
- Latitude is a study-level variable, so the meta-regression is an association across trials, not proof of a causal mechanism.
- The project does not cover the PRISMA steps (search, screening, risk of bias).
