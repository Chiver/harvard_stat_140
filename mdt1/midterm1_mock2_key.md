# STAT 140: Midterm 1 Mock Exam #2 — Answer Key

---

## Part A: True or False (40 points)

### A1. FALSE
The individual treatment effect $\tau_i = Y_i(1) - Y_i(0)$ requires observing **both** potential outcomes for the same unit. Due to the Fundamental Problem of Causal Inference, we can only ever observe one. Even in a randomized experiment, for every unit, one potential outcome is always missing. We can estimate the **average** treatment effect, but never the individual effect.

### A2. TRUE
Each unit has two potential outcomes: $Y_i(0)$ and $Y_i(1)$. We observe exactly one per unit. So for $N = 10$ units, we observe 10 outcomes and miss 10 outcomes. The science table has $2 \times 10 = 20$ cells total, of which 10 are observed and 10 are missing.

### A3. TRUE
This is exactly the definition of SUTVA (Stable Unit Treatment Value Assumption). It has two components: (1) **no interference** — my outcome doesn't depend on your treatment status, and (2) **no hidden variations of treatment** — there is only one version of "treatment" and one version of "control." Together, these allow us to write $Y_i(W_i)$ as a function of only unit $i$'s own assignment.

### A4. TRUE
This "switching equation" relies on SUTVA. If SUTVA is violated (e.g., unit $i$'s outcome depends on whether unit $j$ is also treated), then $Y_i$ is not just a function of $W_i$ but also of $W_j$, $W_k$, etc. The simple two-argument potential outcome notation $Y_i(0), Y_i(1)$ would no longer be well-defined, and the switching equation breaks down.

### A5. TRUE
The ATE is an **average**: $\tau = \frac{1}{N}\sum \tau_i$. Individual effects can be positive for some units and negative for others, and they can cancel out to give $\tau = 0$. For example: $\tau_1 = +5$, $\tau_2 = -5$ → ATE $= 0$, but both individuals have non-zero effects. This is called **treatment effect heterogeneity**.

### A6. TRUE
Under Bernoulli randomization, each unit is independently assigned with probability $p = 0.5$. The probability that all 10 units are treated is $(0.5)^{10} = 1/1024 \approx 0.001$. It's unlikely, but possible. This is actually a **disadvantage** of Bernoulli trials — you could end up with severely imbalanced groups. CRD avoids this by fixing $N_t$.

### A7. TRUE
Under CRD:
- **Individualistic**: The probability that unit $i$ is assigned to treatment depends only on the properties of the assignment mechanism (i.e., $N$, $N_t$), not on the potential outcomes or covariates of any unit. Formally, $P(W_i = 1 | \mathbf{Y}(0), \mathbf{Y}(1), \mathbf{X}) = N_t / N$.
- **Unconfounded**: Treatment assignment is independent of potential outcomes. $P(\mathbf{W} | \mathbf{Y}(0), \mathbf{Y}(1)) = P(\mathbf{W})$. Randomization guarantees this.

### A8. FALSE
Block randomization generally improves precision **in expectation** over CRD when the blocking variable is correlated with the outcome. However, "always" is too strong. If the blocking variable is unrelated to the outcome, blocking provides no benefit and could even slightly reduce degrees of freedom. The improvement depends on how well the blocks capture outcome-relevant variation.

### A9. FALSE
In a paired experiment with 10 pairs, within each pair one unit is randomly assigned to treatment. Each pair has $2$ possible assignments (unit A treated or unit B treated), and the pairs are independent. So the total number of possible assignment vectors is $2^{10} = 1{,}024$, **not** $\binom{20}{10} = 184{,}756$. The latter would be for CRD with 20 units. Pairing **restricts** the randomization, dramatically reducing the number of possible allocations.

### A10. TRUE
Rerandomization works by: (1) generate a random allocation, (2) check if it satisfies a balance criterion (e.g., covariate balance), (3) if yes, use it; if no, discard and draw again. Among the **accepted** allocations, each is equally likely because the original CRD gives equal probability to all allocations, and the acceptance criterion is applied symmetrically. The reference distribution for Fisher-style inference must then be computed over the accepted allocations only.

### A11. FALSE
The Fisher randomization test makes **no distributional assumptions** about potential outcomes. It does not assume normality, equal variances, or any parametric distribution. The p-value is derived entirely from the randomization distribution — the set of all possible treatment allocations under CRD. This is why it's called an "exact" test: it uses the known randomization mechanism, not an assumed probability model.

### A12. TRUE
This is a critical conceptual point. Under the sharp null, all potential outcomes are fixed constants (we know every cell in the science table). The only source of randomness is **which allocation was chosen** by the randomization mechanism. So $T$ varies across allocations not because outcomes are random, but because different allocations put different units in the treatment vs. control groups. This is "randomization-based" inference, as opposed to "sampling-based" inference.

### A13. FALSE
This is a very common misinterpretation. The p-value is **not** the probability that the null is true. The correct interpretation: "**If** the sharp null were true (treatment has no effect on anyone), the probability of observing a test statistic as extreme as or more extreme than $T^{\text{obs}}$ is 0.03." The p-value is a statement about the data given the null, not a statement about the null given the data. To get $P(H_0 | \text{data})$, you would need Bayes' theorem and a prior on $H_0$.

### A14. TRUE
Fisher's framework is extremely flexible in this regard. You can use any function of the data as a test statistic: difference in means, difference in medians, Wilcoxon rank sum, Kolmogorov-Smirnov statistic, Welch $t$-statistic, etc. The choice of test statistic affects **power** (ability to detect a real effect) but not the **validity** of the test. The p-value is always computed by comparing the observed statistic to its randomization distribution.

### A15. TRUE
This is the definition of the Hodges-Lehmann estimator. We test $H_0: \tau_i = \tau_0$ for many candidate values of $\tau_0$. For each, we perform a Fisher randomization test and record the p-value $p(\tau_0)$. The HL estimate is the $\tau_0$ that produces the **largest** p-value — i.e., the value most compatible with the observed data. Computationally, it can also be found as the **median of all pairwise differences** between treated and control units.

### A16. TRUE
Under CRD, $E[\hat{\tau}] = \tau$ regardless of whether treatment effects are constant or heterogeneous. The proof only relies on the fact that each unit has equal probability $N_t/N$ of being treated, which gives $E[\bar{Y}_t^{\text{obs}}] = \bar{Y}(1)$ and $E[\bar{Y}_c^{\text{obs}}] = \bar{Y}(0)$. Heterogeneous treatment effects affect the **variance** of the estimator (through the $S^2(\tau)/N$ term), but not its unbiasedness.

### A17. TRUE
The true variance of $\hat{\tau}$ is:
$$\text{Var}(\hat{\tau}) = \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} - \frac{S^2(\tau)}{N}$$
When $\tau_i = \tau$ for all $i$, the variance of individual effects $S^2(\tau) = 0$, so the third term vanishes. Neyman's estimator $\hat{V} = s_t^2/N_t + s_c^2/N_c$ targets the first two terms, which now equals the true variance exactly (in expectation). So the estimator is exact, not conservative.

### A18. FALSE
Neyman's CI $\hat{\tau} \pm 1.96 \times SE$ relies on the **Central Limit Theorem** (CLT), which guarantees that $\hat{\tau}$ is approximately normally distributed **for large samples**. For small samples, the normal approximation may be poor, and the CI may not achieve its nominal 95% coverage. Fisher's confidence interval (via test inversion) is exact for any sample size, but Neyman's is approximate.

### A19. FALSE
Single imputation **underestimates** uncertainty. When you plug in the group mean for every missing value, you treat those imputed values as if they were truly observed. The resulting ATE estimate appears more precise than it actually is — the confidence intervals are too narrow. The correct approach is **multiple imputation**, which draws many plausible values for each missing outcome, producing a distribution of ATE estimates that properly reflects uncertainty.

### A20. FALSE
The Bayesian Bootstrap is **non-parametric**. It does not assume any parametric distribution (like normal). Instead, it imputes missing $Y_i(0)$ by randomly drawing from the **empirical distribution** of observed control outcomes, and missing $Y_i(1)$ by drawing from observed treatment outcomes. This is simpler and makes fewer assumptions than parametric imputation — no need to specify means, variances, or distributional shape.

---

## Part B: Concept Explanations (30 points)

### B1. Fundamental Problem of Causal Inference (5 points)

The Fundamental Problem of Causal Inference (Holland, 1986) states that for any unit $i$, we can observe **at most one** of the two potential outcomes $Y_i(1)$ and $Y_i(0)$ — never both simultaneously. This is because a unit can only be in one state at a time: either treated or not treated. The unobserved potential outcome is called the **counterfactual**.

This means we can never directly compute the individual causal effect $\tau_i = Y_i(1) - Y_i(0)$ for any unit.

**Randomization helps** by ensuring that the treatment and control groups are, on average, comparable. Under CRD, $E[\bar{Y}_t^{\text{obs}}] = \bar{Y}(1)$ and $E[\bar{Y}_c^{\text{obs}}] = \bar{Y}(0)$, so the difference in group means is an unbiased estimator of the **average** treatment effect. Randomization doesn't let us observe individual counterfactuals, but it allows us to estimate population-level causal quantities without bias.

---

### B2. SUTVA (5 points)

SUTVA (Stable Unit Treatment Value Assumption) has two parts:

1. **No interference**: Unit $i$'s potential outcomes depend **only** on $i$'s own treatment assignment, not on the assignments of other units. Formally: $Y_i(\mathbf{W}) = Y_i(W_i)$.
2. **No hidden variations of treatment**: There is only one "version" of treatment and one "version" of control.

**Example of violation**: In a study of a **vaccine** in a school, vaccinating student A not only protects A but also reduces the chance that A transmits the disease to student B (herd immunity). So B's health outcome $Y_B$ depends not just on $W_B$ but also on $W_A$. If many of B's classmates are vaccinated, B's outcome improves even if B is in the control group.

**Why it matters**: If SUTVA is violated, the simple notation $Y_i(0), Y_i(1)$ is no longer well-defined — we would need $Y_i(\mathbf{W})$, a function of the **entire** assignment vector. The science table would have $2^N$ columns per unit instead of 2, making causal inference far more complex. Estimates of ATE from simple difference-in-means would be biased.

---

### B3. Deeper Role of Randomization (5 points)

The student's explanation is incomplete. Making groups "look similar" on observed covariates is a consequence of randomization, but not its fundamental role.

The deeper role is that randomization makes the treatment assignment **independent of potential outcomes**:

$$P(\mathbf{W} | \mathbf{Y}(0), \mathbf{Y}(1)) = P(\mathbf{W})$$

This means the assignment mechanism is **unconfounded** — there is no systematic relationship between who gets treated and how they would respond. This holds for **all** variables, including unobserved ones we haven't measured and don't even know about.

Matching on covariates or "balancing" groups can only control for **observed** confounders. Randomization controls for **everything** — observed and unobserved — because the assignment is determined purely by chance. This is why randomized experiments are the "gold standard" for causal inference.

Additionally, randomization provides the **reference distribution** for Fisher's test. Because we know exactly how the assignment was generated, we can compute exactly how extreme the observed result is under the null.

---

### B4. Sharp Null vs. Weak Null (5 points)

**Sharp null** (Fisher's null): $H_0: Y_i(1) = Y_i(0)$ for **every** unit $i$. This says the treatment has **exactly zero effect on each individual**. It's "sharp" because it fully specifies all missing potential outcomes. Under this null, if unit $i$ is treated and we observe $Y_i^{\text{obs}} = 5$, then $Y_i(0) = Y_i(1) = 5$. Every cell in the science table is filled in, which is why we can enumerate all possible test statistics.

**Weak null** (Neyman's null): $H_0: \bar{Y}(1) - \bar{Y}(0) = 0$, i.e., the **average** treatment effect is zero. This allows individual effects to be non-zero as long as they cancel out (e.g., $\tau_1 = +5, \tau_2 = -5$). Under this null, we **cannot** fill in the science table because we don't know each unit's individual effect — only that they average to zero.

This is why Fisher's test requires the sharp null: it needs the complete science table to enumerate all possible test statistics under every allocation. Neyman's approach sidesteps this by working with the estimator $\hat{\tau}$ and its sampling distribution directly, without needing individual-level counterfactuals.

---

### B5. Conservative Variance Estimator (5 points)

The true variance of $\hat{\tau}$ under CRD is:
$$\text{Var}(\hat{\tau}) = \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} - \frac{S^2(\tau)}{N}$$

The third term $S^2(\tau)/N$ is the variance of individual treatment effects, and it is **always non-negative**. Neyman's estimator ignores this term:

$$\hat{V}_{\text{Neyman}} = \frac{s_t^2}{N_t} + \frac{s_c^2}{N_c}$$

In expectation, $E[\hat{V}]$ targets the first two terms but **not** the subtracted third term. So $E[\hat{V}] \geq \text{Var}(\hat{\tau})$, meaning it **overestimates** the variance on average. "Conservative" means the resulting confidence intervals tend to be **wider** than necessary — we are being cautious, which is safe (coverage is at least 95%) but less efficient.

When the treatment effect is **constant** ($\tau_i = \tau$ for all $i$), every unit has the same effect, so the variance of effects $S^2(\tau) = 0$. The third term disappears, and $E[\hat{V}]$ equals the true variance exactly. Intuitively: if the treatment adds exactly the same amount to everyone, knowing $Y_i(1)$ tells you exactly what $Y_i(0)$ is (just subtract $\tau$), so there's no "extra" uncertainty from treatment effect heterogeneity.

---

### B6. Fisher CI vs. Neyman CI (5 points)

**Fisher CI** (Test Inversion):
- **Construction**: For each candidate $\tau_0$, assume $H_0: \tau_i = \tau_0$ (constant treatment effect), fill in the science table, run a Fisher randomization test, record the p-value. Collect all $\tau_0$ where $p(\tau_0) > \alpha$. This set is the $(1-\alpha)$ CI.
- **Properties**: **Exact** for any sample size. Does not require CLT. But assumes constant treatment effect.

**Neyman CI**:
- **Construction**: Compute $\hat{\tau} \pm z_{\alpha/2} \times SE$ using the conservative variance estimator.
- **Properties**: **Approximate**, relies on CLT (large sample). Allows heterogeneous treatment effects. Computationally trivial — just a formula.

| | Fisher CI | Neyman CI |
|---|---|---|
| Sample size | Any (exact) | Large (needs CLT) |
| Treatment effect | Must be constant | Can be heterogeneous |
| Computation | Heavy (enumerate allocations for each $\tau_0$) | Simple formula |
| Variance | From randomization distribution | Conservative estimator |

**When to prefer which**: Use Fisher CI with small samples where CLT may not hold, or when you want exact coverage. Use Neyman CI with large samples when you want a quick answer and can tolerate the conservative approximation.

---

## Part C: Calculation Problems (30 points)

### C1: Fisher Analysis of Music Experiment (15 points)

#### C1(a): Observed test statistic (2 points)

$$\bar{Y}_t = \frac{88 + 92}{2} = 90$$

$$\bar{Y}_c = \frac{80 + 84}{2} = 82$$

$$T^{\text{obs}} = 90 - 82 = 8$$

---

#### C1(b): All possible allocations (2 points)

$\binom{4}{2} = 6$ possible allocations:

| # | Treated Units |
|---|---------------|
| 1 | {1, 2} ← observed |
| 2 | {1, 3} |
| 3 | {1, 4} |
| 4 | {2, 3} |
| 5 | {2, 4} |
| 6 | {3, 4} |

---

#### C1(c): Sharp null and complete science table (2 points)

$H_0: Y_i(1) = Y_i(0)$ for all $i$.

Under this hypothesis, treatment has no effect:

| Student | $Y_i(0)$ | $Y_i(1)$ |
|---------|-----------|-----------|
| 1 | 88 | 88 |
| 2 | 92 | 92 |
| 3 | 80 | 80 |
| 4 | 84 | 84 |

---

#### C1(d): Test statistics for all allocations (4 points)

Under sharp null, outcomes are fixed at {88, 92, 80, 84}. For each allocation:

| # | Treated | $\bar{Y}_t$ | $\bar{Y}_c$ | $T$ |
|---|---------|------------|------------|-----|
| 1 | {1,2} | (88+92)/2 = 90 | (80+84)/2 = 82 | **8** ← observed |
| 2 | {1,3} | (88+80)/2 = 84 | (92+84)/2 = 88 | −4 |
| 3 | {1,4} | (88+84)/2 = 86 | (92+80)/2 = 86 | 0 |
| 4 | {2,3} | (92+80)/2 = 86 | (88+84)/2 = 86 | 0 |
| 5 | {2,4} | (92+84)/2 = 88 | (88+80)/2 = 84 | 4 |
| 6 | {3,4} | (80+84)/2 = 82 | (88+92)/2 = 90 | **−8** |

---

#### C1(e): Two-sided Fisher p-value (2 points)

$|T| \geq |T^{\text{obs}}| = 8$:

- #1: $|T| = 8$ ✓
- #6: $|T| = 8$ ✓
- All others: $|T| < 8$ ✗

$$p = \frac{2}{6} = \frac{1}{3} \approx 0.333$$

**Conclusion**: At $\alpha = 0.05$, we **fail to reject** the sharp null. There is insufficient evidence that music affects exam scores. Note: with only 4 students and 6 possible allocations, the smallest achievable p-value is $1/6 \approx 0.167$, so we **can never** reach significance at $\alpha = 0.05$ with this design.

---

#### C1(f): Hodges-Lehmann estimate (3 points)

Pairwise differences (each treated unit minus each control unit):

| | Student 3 ($Y = 80$) | Student 4 ($Y = 84$) |
|---|---|---|
| Student 1 ($Y = 88$) | $88 - 80 = 8$ | $88 - 84 = 4$ |
| Student 2 ($Y = 92$) | $92 - 80 = 12$ | $92 - 84 = 8$ |

All pairwise differences sorted: **4, 8, 8, 12**

Median = $\frac{8 + 8}{2} = 8$

$$\hat{\tau}_{HL} = 8$$

**Interpretation**: Our best estimate is that music improves exam scores by 8 points. This equals $T^{\text{obs}}$ in this symmetric example.

---

### C2: Neymanian Analysis of Teaching Method (10 points)

#### C2(a): ATE estimate (2 points)

$$\hat{\tau} = \bar{Y}_t - \bar{Y}_c = 78.4 - 73.1 = 5.3$$

The new teaching method improves scores by an estimated 5.3 points.

---

#### C2(b): Conservative variance and SE (2 points)

$$\hat{V}_{\text{Neyman}} = \frac{s_t^2}{N_t} + \frac{s_c^2}{N_c} = \frac{156.25}{100} + \frac{182.49}{100} = 1.5625 + 1.8249 = 3.3874$$

$$SE = \sqrt{3.3874} = 1.8405$$

---

#### C2(c): 95% Confidence Interval (2 points)

$$CI_{95\%} = \hat{\tau} \pm 1.96 \times SE = 5.3 \pm 1.96 \times 1.8405 = 5.3 \pm 3.607$$

$$CI_{95\%} = [1.693, \ 8.907]$$

---

#### C2(d): 99% Confidence Interval (2 points)

$$CI_{99\%} = \hat{\tau} \pm 2.576 \times SE = 5.3 \pm 2.576 \times 1.8405 = 5.3 \pm 4.741$$

$$CI_{99\%} = [0.559, \ 10.041]$$

---

#### C2(e): Responding to the skeptic (2 points)

**Against 95% CI**: The 95% CI is $[1.693, 8.907]$, which does **not contain 0**. We can reject the claim that the new method has no effect at the 5% significance level. The data provides statistically significant evidence that the new method improves scores by somewhere between 1.7 and 8.9 points.

**Against 99% CI**: The 99% CI is $[0.559, 10.041]$, which also does **not contain 0**. Even at the more stringent 99% confidence level, we can still reject the skeptic's claim. However, the lower bound (0.559) is very close to zero, suggesting that while the evidence supports a positive effect, the effect could be quite small.

**Note on widths**: The 99% CI is wider than the 95% CI ($\pm 4.741$ vs. $\pm 3.607$). This illustrates the tradeoff: higher confidence requires a wider interval. We are "more sure" the interval contains $\tau$, but the range of plausible values is broader.

---

### C3: Bayesian Bootstrap of Music Experiment (5 points)

#### C3(a): Sampling pools (1 point)

- **Pool for missing $Y_i(0)$**: {80, 84} — observed outcomes of control students (3 and 4)
- **Pool for missing $Y_i(1)$**: {88, 92} — observed outcomes of treated students (1 and 2)

---

#### C3(b): All 16 imputations (2 points)

Four missing values: $Y_1(0)$, $Y_2(0)$ ∈ {80, 84} and $Y_3(1)$, $Y_4(1)$ ∈ {88, 92}.

Each has 2 choices → $2^4 = 16$ combinations.

For each combination, we compute:
$$\hat{\tau} = \frac{1}{4}\sum_{i=1}^{4}[Y_i(1) - Y_i(0)]$$

| # | $Y_1(0)$ | $Y_2(0)$ | $Y_3(1)$ | $Y_4(1)$ | $\tau_1$ | $\tau_2$ | $\tau_3$ | $\tau_4$ | ATE |
|---|----------|----------|----------|----------|----------|----------|----------|----------|-----|
| 1 | 80 | 80 | 88 | 88 | 8 | 12 | 8 | 4 | **8.0** |
| 2 | 80 | 80 | 88 | 92 | 8 | 12 | 8 | 8 | **9.0** |
| 3 | 80 | 80 | 92 | 88 | 8 | 12 | 12 | 4 | **9.0** |
| 4 | 80 | 80 | 92 | 92 | 8 | 12 | 12 | 8 | **10.0** |
| 5 | 80 | 84 | 88 | 88 | 8 | 8 | 8 | 4 | **7.0** |
| 6 | 80 | 84 | 88 | 92 | 8 | 8 | 8 | 8 | **8.0** |
| 7 | 80 | 84 | 92 | 88 | 8 | 8 | 12 | 4 | **8.0** |
| 8 | 80 | 84 | 92 | 92 | 8 | 8 | 12 | 8 | **9.0** |
| 9 | 84 | 80 | 88 | 88 | 4 | 12 | 8 | 4 | **7.0** |
| 10 | 84 | 80 | 88 | 92 | 4 | 12 | 8 | 8 | **8.0** |
| 11 | 84 | 80 | 92 | 88 | 4 | 12 | 12 | 4 | **8.0** |
| 12 | 84 | 80 | 92 | 92 | 4 | 12 | 12 | 8 | **9.0** |
| 13 | 84 | 84 | 88 | 88 | 4 | 8 | 8 | 4 | **6.0** |
| 14 | 84 | 84 | 88 | 92 | 4 | 8 | 8 | 8 | **7.0** |
| 15 | 84 | 84 | 92 | 88 | 4 | 8 | 12 | 4 | **7.0** |
| 16 | 84 | 84 | 92 | 92 | 4 | 8 | 12 | 8 | **8.0** |

---

#### C3(c): Mean and range (2 points)

All 16 ATE values: 6.0, 7.0, 7.0, 7.0, 8.0, 8.0, 8.0, 8.0, 8.0, 8.0, 9.0, 9.0, 9.0, 9.0, 10.0

Wait, let me recount: 8.0, 9.0, 9.0, 10.0, 7.0, 8.0, 8.0, 9.0, 7.0, 8.0, 8.0, 9.0, 6.0, 7.0, 7.0, 8.0

**Sum** = 8+9+9+10+7+8+8+9+7+8+8+9+6+7+7+8 = **128**

**Mean** = 128/16 = **8.0**

**Range** = [6.0, 10.0]

**Comparison**: The mean of the Bayesian Bootstrap ATE (8.0) exactly equals $T^{\text{obs}} = 8$ from C1(a). This is not a coincidence — the bootstrap is centered on the observed difference in means. But the bootstrap gives us **more than just a point estimate**: it shows that the ATE could plausibly range from 6 to 10, capturing the **uncertainty** we have about missing potential outcomes. The single number $T^{\text{obs}} = 8$ hides this uncertainty.

---

**END OF ANSWER KEY**
