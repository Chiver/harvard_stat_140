# STAT 140: Midterm 1 Mock Exam — Answer Key

---

## Part A: True or False (30 points)

### A1. FALSE

In the potential outcomes framework, the **Fundamental Problem of Causal Inference** states that for a given unit, we can only ever observe **one** potential outcome. Even if we run the experiment twice, the unit is no longer the same — time has passed, conditions have changed. The second observation is a different "unit-time" combination. $Y_i(1)$ and $Y_i(0)$ refer to the **same moment** in time under two different hypothetical states, and we can never simultaneously realize both.

---

### A2. FALSE

The first part is correct: under CRD, each unit has probability $N_t/N = 4/8 = 0.5$ of being treated. However, the assignments are **NOT independent**. In CRD, the total number of treated units is fixed at $N_t = 4$. So if unit 1 is assigned to treatment, the probability that unit 2 is also treated decreases to $3/7$, not $4/8$. Independence of assignments is a property of **Bernoulli trials**, not CRD.

More formally, under CRD:
$$P(W_i = 1) = \frac{N_t}{N} = 0.5$$
$$P(W_j = 1 \mid W_i = 1) = \frac{N_t - 1}{N - 1} = \frac{3}{7} \neq 0.5$$

---

### A3. TRUE

The sharp null $H_0: Y_i(1) = Y_i(0)$ for all $i$ says that the treatment effect is exactly zero for **every single unit**. This implies $\bar{Y}(1) - \bar{Y}(0) = 0$, but the reverse is not true. The weaker null $H_0: \bar{Y}(1) - \bar{Y}(0) = 0$ only says the **average** effect is zero — some units could have positive effects and others negative, as long as they cancel out. So the sharp null is strictly stronger (more restrictive) than the average null.

---

### A4. TRUE

Under CRD with $N = 6$ and $N_t = 3$, there are $\binom{6}{3} = 20$ equally likely allocations. The most extreme case is when only the observed allocation (and possibly its mirror) produces a test statistic as extreme as the observed one. The minimum p-value is:

$$p_{\min} = \frac{1}{\binom{6}{3}} = \frac{1}{20} = 0.05$$

This means that with only 6 units and 3 treated, we can **barely** achieve significance at $\alpha = 0.05$, and only if the observed allocation is uniquely the most extreme.

---

### A5. FALSE

The statement is almost right but technically imprecise. In **expectation**, Neyman's variance estimator overestimates the true variance:

$$E[\hat{V}] = \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} \geq \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} - \frac{S^2(\tau)}{N} = \text{Var}(\hat{\tau})$$

However, this is true **in expectation** (on average across repeated experiments). For any **particular** dataset, the realized $\hat{V}$ could happen to be smaller than the true variance due to sampling variability. "Conservative" refers to the expected value, not a guarantee for every single realization.

Additionally, if the treatment effect is constant ($\tau_i = \tau$ for all $i$), then $S^2(\tau) = 0$ and the estimator is **exact**, not conservative.

---

### A6. FALSE

Single imputation **underestimates** uncertainty. When you plug in a single value (e.g., the group mean) for each missing potential outcome, you treat that imputed value as if it were truly observed. This ignores the fact that the imputed value is itself uncertain — the true missing value could be higher or lower. The result is confidence intervals that are too narrow and an ATE estimate that appears more precise than it actually is.

The correct approach is **multiple imputation**: draw many different plausible values for each missing outcome from a posterior distribution, compute ATE for each imputation, and combine the results. This properly reflects both the uncertainty about the treatment effect and the uncertainty about the missing values.

---

## Part B: Concepts and Short Answer (30 points)

### B1. Science Table (10 points)

**(a)** From observed data:

| Unit $i$ | $W_i$ | $Y_i^{\text{obs}}$ | $Y_i(0)$ | $Y_i(1)$ | $\tau_i$ |
|----------|--------|-------------------|-----------|-----------|----------|
| 1 | 1 | 50 | **?** | **50** | ? |
| 2 | 1 | 70 | **?** | **70** | ? |
| 3 | 0 | 40 | **40** | **?** | ? |
| 4 | 0 | 55 | **55** | **?** | ? |

**Reasoning:**
- Unit 1 is treated ($W_1 = 1$), so we observe $Y_1(1) = 50$. We cannot observe $Y_1(0)$.
- Unit 3 is control ($W_3 = 0$), so we observe $Y_3(0) = 40$. We cannot observe $Y_3(1)$.
- All $\tau_i$ are unknown because we never observe both potential outcomes for any unit.

This is the **Fundamental Problem of Causal Inference**: each "?" represents a counterfactual outcome that can never be directly observed.

---

**(b)** Under sharp null $H_0: Y_i(1) = Y_i(0)$ for all $i$:

| Unit $i$ | $W_i$ | $Y_i^{\text{obs}}$ | $Y_i(0)$ | $Y_i(1)$ | $\tau_i$ |
|----------|--------|-------------------|-----------|-----------|----------|
| 1 | 1 | 50 | **50** | 50 | **0** |
| 2 | 1 | 70 | **70** | 70 | **0** |
| 3 | 0 | 40 | 40 | **40** | **0** |
| 4 | 0 | 55 | 55 | **55** | **0** |

Under the sharp null, treatment has no effect on anyone, so $Y_i(1) = Y_i(0) = Y_i^{\text{obs}}$ for all units.

---

**(c)** Under constant treatment effect $\tau_i = 10$ for all $i$:

For treated units: $Y_i(0) = Y_i(1) - 10 = Y_i^{\text{obs}} - 10$
For control units: $Y_i(1) = Y_i(0) + 10 = Y_i^{\text{obs}} + 10$

| Unit $i$ | $W_i$ | $Y_i^{\text{obs}}$ | $Y_i(0)$ | $Y_i(1)$ | $\tau_i$ |
|----------|--------|-------------------|-----------|-----------|----------|
| 1 | 1 | 50 | **40** | 50 | **10** |
| 2 | 1 | 70 | **60** | 70 | **10** |
| 3 | 0 | 40 | 40 | **50** | **10** |
| 4 | 0 | 55 | 55 | **65** | **10** |

---

### B2. Assignment Mechanisms (10 points)

**(a)** Number of possible assignment vectors:

- **(i) CRD**: $\binom{20}{10} = 184{,}756$ possible allocations (choosing which 10 of 20 get treated).
- **(ii) Bernoulli**: $2^{20} = 1{,}048{,}576$ possible allocations (each unit independently assigned with $p = 0.5$).
- **(iii) Block (paired)**: Each of the 10 pairs has 2 possible assignments (either member gets treatment). So $2^{10} = 1{,}024$ possible allocations.

**(b)** Fixed or random number of treated units:

- **(i) CRD**: **Fixed** at $N_t = 10$. This is by definition.
- **(ii) Bernoulli**: **Random**. Could be anywhere from 0 to 20. Expected value is 10, but not guaranteed.
- **(iii) Block**: **Fixed** at $N_t = 10$ (exactly one treated per pair, 10 pairs).

**(c)** Fisher reference distribution construction:

- **(i) CRD**: Enumerate all $\binom{20}{10} = 184{,}756$ allocations, each with equal probability $1/184{,}756$. Compute $T$ for each. (In practice, use Monte Carlo approximation.)
- **(ii) Bernoulli**: Enumerate all $2^{20} = 1{,}048{,}576$ allocations, each with probability $(0.5)^{20}$. This includes allocations with different numbers of treated units. Each allocation gets equal weight.
- **(iii) Block**: Enumerate all $2^{10} = 1{,}024$ within-pair allocations, each with equal probability $1/1{,}024$. The randomization is restricted to within-pair swaps only.

---

### B3. Three Approaches Compared (10 points)

| | Fisher | Neyman | Bayesian |
|---|---|---|---|
| **Primary goal** | Test whether there is **any** effect (hypothesis testing) | **Estimate** the average treatment effect and give a confidence interval | Obtain the full **posterior distribution** of the treatment effect |
| **What is fixed** | Potential outcomes are fixed; only the assignment vector varies | Same as Fisher: potential outcomes fixed, assignment varies | Assignment and observed data are fixed; missing potential outcomes are treated as **random variables** |
| **What is random** | The assignment vector $\mathbf{W}$ | The assignment vector $\mathbf{W}$ | The missing potential outcomes, governed by a prior + likelihood |
| **One advantage** | Exact for any sample size; no distributional assumptions | Simple closed-form formula; allows heterogeneous treatment effects | Gives a complete probability distribution; naturally handles uncertainty about missing data |
| **One limitation** | Requires sharp null (or constant effect for CI); cannot directly estimate ATE | Requires large sample for CLT approximation; variance estimator is conservative | Results depend on prior specification and modeling assumptions |

---

## Part C: Fisher Inference (20 points)

### C1(a): Observed test statistic (3 points)

$$\bar{Y}_t = \frac{25 + 30 + 28}{3} = \frac{83}{3} \approx 27.67$$

$$\bar{Y}_c = \frac{18 + 22 + 20}{3} = \frac{60}{3} = 20.00$$

$$T^{\text{obs}} = 27.67 - 20.00 = 7.67$$

---

### C1(b): Sharp null and complete science table (3 points)

**Sharp null**: $H_0: Y_i(1) = Y_i(0)$ for all $i = 1, \ldots, 6$.

Under this hypothesis, treatment has no effect on any plant:

| Plant | $W_i$ | $Y_i^{\text{obs}}$ | $Y_i(0)$ | $Y_i(1)$ |
|-------|--------|-------------------|-----------|-----------|
| 1 | 1 | 25 | 25 | 25 |
| 2 | 1 | 30 | 30 | 30 |
| 3 | 1 | 28 | 28 | 28 |
| 4 | 0 | 18 | 18 | 18 |
| 5 | 0 | 22 | 22 | 22 |
| 6 | 0 | 20 | 20 | 20 |

---

### C1(c): All 20 allocations and their test statistics (8 points)

Under the sharp null, outcomes are fixed: {25, 30, 28, 18, 22, 20} for units {1,2,3,4,5,6}.

For each allocation, the treated units' outcomes are averaged and the control units' outcomes are averaged, then $T = \bar{Y}_t - \bar{Y}_c$.

| # | Treated Units | $\bar{Y}_t$ | $\bar{Y}_c$ | $T$ |
|---|---------------|------------|------------|-----|
| 1 | {1,2,3} | 27.67 | 20.00 | **7.67** ← observed |
| 2 | {1,2,4} | 24.33 | 23.33 | 1.00 |
| 3 | {1,2,5} | 25.67 | 22.00 | 3.67 |
| 4 | {1,2,6} | 25.00 | 22.67 | 2.33 |
| 5 | {1,3,4} | 23.67 | 24.00 | −0.33 |
| 6 | {1,3,5} | 25.00 | 22.67 | 2.33 |
| 7 | {1,3,6} | 24.33 | 23.33 | 1.00 |
| 8 | {1,4,5} | 21.67 | 26.00 | −4.33 |
| 9 | {1,4,6} | 21.00 | 26.67 | −5.67 |
| 10 | {1,5,6} | 22.33 | 25.33 | −3.00 |
| 11 | {2,3,4} | 25.33 | 22.33 | 3.00 |
| 12 | {2,3,5} | 26.67 | 21.00 | 5.67 |
| 13 | {2,3,6} | 26.00 | 21.67 | 4.33 |
| 14 | {2,4,5} | 23.33 | 24.33 | −1.00 |
| 15 | {2,4,6} | 22.67 | 25.00 | −2.33 |
| 16 | {2,5,6} | 24.00 | 23.67 | 0.33 |
| 17 | {3,4,5} | 22.67 | 25.00 | −2.33 |
| 18 | {3,4,6} | 22.00 | 25.67 | −3.67 |
| 19 | {3,5,6} | 23.33 | 24.33 | −1.00 |
| 20 | {4,5,6} | 20.00 | 27.67 | **−7.67** |

---

### C1(d): One-sided Fisher exact p-value (3 points)

"Extreme" = $T \geq T^{\text{obs}} = 7.67$.

Looking at the table, only **one** allocation achieves $T \geq 7.67$:
- #1: $T = 7.67$ ✓

$$p\text{-value} = \frac{1}{20} = 0.05$$

---

### C1(e): Conclusion (3 points)

At $\alpha = 0.05$, the p-value = 0.05 is **right at the boundary**. Depending on convention:
- If using strict inequality ($p < 0.05$), we **fail to reject** $H_0$.
- If using $p \leq 0.05$, we **reject** $H_0$.

In most conventions, we would say the result is **marginally significant**. In context: there is suggestive but not strong evidence that the new fertilizer increases plant height. The observed difference of 7.67 cm is the most extreme possible under this experimental design, which means we have reached the **resolution limit** of this experiment — with only 6 plants, the Fisher test simply cannot produce a p-value smaller than 0.05.

**Important note**: This illustrates a key limitation — with $\binom{6}{3} = 20$ allocations, the smallest achievable p-value is exactly 1/20 = 0.05. To get more statistical power, we would need more experimental units.

---

## Part D: Neymanian Inference (10 points)

### D1(a): Estimated ATE (2 points)

$$\hat{\tau} = \bar{Y}_t - \bar{Y}_c = 42.5 - 51.8 = -9.3$$

The meditation app group scored 9.3 points lower (less anxious) on average.

---

### D1(b): Conservative variance and SE (3 points)

$$\hat{V}_{\text{Neyman}} = \frac{s_t^2}{N_t} + \frac{s_c^2}{N_c} = \frac{120.3}{30} + \frac{135.7}{30} = 4.01 + 4.523 = 8.533$$

$$SE = \sqrt{\hat{V}} = \sqrt{8.533} = 2.921$$

---

### D1(c): 95% Confidence Interval (3 points)

$$CI = \hat{\tau} \pm 1.96 \times SE = -9.3 \pm 1.96 \times 2.921 = -9.3 \pm 5.725$$

$$CI = [-15.025, \ -3.575]$$

---

### D1(d): Conclusion (2 points)

Yes, we can conclude the app reduces anxiety. The 95% CI is $[-15.025, -3.575]$, which lies **entirely below zero**. This means:

1. At the 95% confidence level, we can rule out "no effect" ($\tau = 0$) — 0 is not in the interval.
2. The estimated reduction is between 3.6 and 15.0 points on a 100-point scale.
3. The effect is both **statistically significant** (CI excludes 0) and potentially **practically meaningful** (a reduction of at least 3.6 points in anxiety).

---

## Part E: Bayesian and Conceptual Integration (10 points)

### E1(a): Sampling pools (2 points)

- **Pool for missing $Y_i(0)$**: {65, 70} — the observed outcomes of the control group (C and D).
- **Pool for missing $Y_i(1)$**: {80, 90} — the observed outcomes of the treatment group (A and B).

These pools are appropriate under CRD because randomization ensures that treatment and control groups are drawn from the **same underlying population**. So the observed control outcomes are representative of what treated units would have scored without treatment, and vice versa.

---

### E1(b): One round of Bayesian Bootstrap (2 points)

Given draws: $Y_A(0) = 70$, $Y_B(0) = 65$, $Y_C(1) = 80$, $Y_D(1) = 90$.

| Unit | $Y_i(1)$ | $Y_i(0)$ | $\tau_i$ |
|------|----------|----------|----------|
| A | 80 | 70 | 10 |
| B | 90 | 65 | 25 |
| C | 80 | 65 | 15 |
| D | 90 | 70 | 20 |

$$\hat{\tau} = \frac{10 + 25 + 15 + 20}{4} = \frac{70}{4} = 17.5$$

---

### E1(c): Why better than single imputation (1 point)

Single imputation would plug in $\bar{Y}_c = 67.5$ for all missing $Y_i(0)$ and $\bar{Y}_t = 85$ for all missing $Y_i(1)$. This gives **one** ATE estimate and treats it as certain.

The Bayesian Bootstrap **repeats the random drawing many times**, producing a **distribution** of ATE estimates. Each draw gives different imputed values, so the ATE varies. This variation captures the **uncertainty we have about the missing values**. Single imputation pretends we know the missing values exactly; multiple imputation honestly admits we don't.

---

### E2(a): Interpreting three results (2 points)

- **Fisher p = 0.03**: Under the sharp null (no effect on anyone), only 3% of all possible random allocations would produce a test statistic as extreme as observed. This is evidence against the null.
- **Neyman 95% CI = [0.5, 4.2]**: Using the difference-in-means estimator with a conservative variance estimate, we are 95% confident that the interval [0.5, 4.2] contains the true ATE. The procedure that generated this interval captures the true ATE 95% of the time.
- **HL estimate = 2.1**: Among all possible constant treatment effect values, $\tau_0 = 2.1$ is the most compatible with the observed data (maximizes the Fisher p-value). This is our best point estimate of the treatment effect under a constant effect assumption.

---

### E2(b): Consistency (1.5 points)

Yes, they are consistent:
- Fisher rejects the sharp null ($p = 0.03 < 0.05$), meaning there is evidence of **some** effect.
- The Neyman CI $[0.5, 4.2]$ does **not contain 0**, which is consistent with Fisher's rejection.
- The HL estimate of 2.1 falls inside the Neyman CI, which is expected — the point estimate should lie within the confidence interval.

All three methods point in the same direction: there is a positive treatment effect, estimated around 2.1, and we can be fairly confident it is between 0.5 and 4.2.

---

### E2(c): Correcting the CI interpretation (1.5 points)

The colleague's statement is **incorrect**. The true treatment effect $\tau$ is a **fixed, unknown constant** — it does not have a probability of being anywhere. Either it is in $[0.5, 4.2]$ or it is not.

**Correct interpretation**: "If we repeated this experiment many times, each time computing a 95% CI using the same procedure, approximately 95% of those intervals would contain the true $\tau$. The interval $[0.5, 4.2]$ is one realization of this procedure."

The 95% refers to the **reliability of the method** (the interval-constructing procedure), not to the probability that $\tau$ lies in any particular interval.

---

**END OF ANSWER KEY**
