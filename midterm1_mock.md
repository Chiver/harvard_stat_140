# STAT 140: Design of Experiments — Midterm 1 Mock Exam

**Time: 75 minutes | Total: 100 points**
**Covers: Modules 1–6 (Potential Outcomes, Assignment Mechanisms, Fisher, Neyman, Bayesian Inference)**

---

## Part A: True or False (30 points, 5 points each)

**For each statement, state TRUE or FALSE and provide a brief justification (2–3 sentences). No credit without justification.**

**A1.** In the potential outcomes framework, for a given unit $i$, we can observe both $Y_i(1)$ and $Y_i(0)$ if we run the experiment twice. 

> F. We have no way to know the outcome of the counterpart if we give one treatment or control. 

**A2.** Under a completely randomized design (CRD) with $N = 8$ units and $N_t = 4$ treated, each unit has a probability of $4/8 = 0.5$ of being assigned to treatment, and the assignment of unit $i$ is independent of the assignment of unit $j$.

> T. 

**A3.** The sharp null hypothesis $H_0: Y_i(1) = Y_i(0)$ for all $i$ is a stronger assumption than the null hypothesis $H_0: \bar{Y}(1) - \bar{Y}(0) = 0$.

> F. They are the same. 

**A4.** In a Fisher randomization test with $N = 6$ and $N_t = 3$ under CRD, the minimum possible p-value is $1/20 = 0.05$.

> T. (6, 3) = 20, making the minimum 1/20 = 0.05. 

**A5.** Neyman's conservative variance estimator $\hat{V} = s_t^2/N_t + s_c^2/N_c$ always overestimates the true variance of $\hat{\tau}$, regardless of the data.

> F. If $\tau = 0$, then the variance is the same. 

**A6.** In the Bayesian approach to causal inference, single imputation (plugging in group means for missing potential outcomes) correctly captures the uncertainty about missing values.

> F. 

---

## Part B: Concepts and Short Answer (30 points)

**B1. (10 points)** Consider the following incomplete science table for an experiment with $N = 4$ units:

| Unit $i$ | $W_i$ | $Y_i^{\text{obs}}$ | $Y_i(0)$ | $Y_i(1)$ | $\tau_i$ |
|----------|--------|-------------------|-----------|-----------|----------|
| 1 | 1 | 50 | ? | ? | ? |
| 2 | 1 | 70 | ? | ? | ? |
| 3 | 0 | 40 | ? | ? | ? |
| 4 | 0 | 55 | ? | ? | ? |

**(a)** Fill in all values that can be determined from the observed data. Identify which values remain unknown and explain why.

**(b)** Now assume the sharp null hypothesis $H_0: Y_i(1) = Y_i(0)$ for all $i$. Fill in the complete science table.

**(c)** Now assume a constant treatment effect of $\tau_i = 10$ for all $i$. Fill in the complete science table. (Note: this will differ from part (b).)

---

**B2. (10 points)** A researcher wants to study the effect of a new tutoring program on test scores. She has 20 students. She considers three assignment mechanisms:

- (i) Completely Randomized Design (CRD) with $N_t = 10$
- (ii) Bernoulli trial with $p = 0.5$
- (iii) Block randomization: first divide students into 10 pairs by baseline GPA, then within each pair randomly assign one to treatment

**(a)** For each design, how many possible assignment vectors exist?

**(b)** For each design, state whether the number of treated units is fixed or random.

**(c)** The researcher plans to use a Fisher randomization test. For each design, briefly describe how the reference distribution would be constructed differently.

---

**B3. (10 points)** Explain the key philosophical differences between Fisher, Neyman, and Bayesian approaches to causal inference. For each approach, state:
- What is the primary goal?
- What does the approach treat as fixed vs. random?
- What is one advantage and one limitation?

---

## Part C: Fisher Inference (20 points)

**C1.** A completely randomized experiment tests whether a new fertilizer increases plant height (in cm). There are $N = 6$ plants, with $N_t = 3$ assigned to the new fertilizer and $N_c = 3$ assigned to no fertilizer. The observed data are:

| Plant | $W_i$ | $Y_i^{\text{obs}}$ (height in cm) |
|-------|--------|--------------------------------|
| 1 | 1 | 25 |
| 2 | 1 | 30 |
| 3 | 1 | 28 |
| 4 | 0 | 18 |
| 5 | 0 | 22 |
| 6 | 0 | 20 |

**(a)** (3 points) Compute the observed test statistic $T^{\text{obs}} = \bar{Y}_t - \bar{Y}_c$.

**(b)** (3 points) Write out the sharp null hypothesis. Under this hypothesis, fill in the complete science table.

**(c)** (8 points) Enumerate all $\binom{6}{3} = 20$ possible allocations under CRD. For each allocation, compute $T$. (Hint: you may organize this as a table.)

**(d)** (3 points) Compute the one-sided Fisher exact p-value, where "extreme" means $T \geq T^{\text{obs}}$.

**(e)** (3 points) At the $\alpha = 0.05$ significance level, what is your conclusion? Interpret in context.

---

## Part D: Neymanian Inference (10 points)

**D1.** In a randomized experiment studying the effect of a meditation app on anxiety scores (scale 0–100, lower is better), 60 participants are randomly assigned: 30 to the app (treatment) and 30 to a waitlist (control). The results are:

| Group | $N$ | $\bar{Y}$ | $s^2$ |
|-------|-----|-----------|-------|
| Treatment (App) | 30 | 42.5 | 120.3 |
| Control (Waitlist) | 30 | 51.8 | 135.7 |

**(a)** (2 points) Compute the estimated ATE $\hat{\tau}$.

**(b)** (3 points) Compute the conservative variance estimate $\hat{V}_{\text{Neyman}}$ and the standard error.

**(c)** (3 points) Construct a 95% confidence interval for $\tau$.

**(d)** (2 points) Based on the confidence interval, can we conclude that the meditation app reduces anxiety? Explain.

---

## Part E: Bayesian and Conceptual Integration (10 points)

**E1. (5 points)** Consider the following data from a small experiment:

| Unit | $W_i$ | $Y_i^{\text{obs}}$ |
|------|--------|-------------------|
| A | 1 | 80 |
| B | 1 | 90 |
| C | 0 | 65 |
| D | 0 | 70 |

A researcher uses the Bayesian Bootstrap to estimate the ATE.

**(a)** What are the two "sampling pools" for imputing missing potential outcomes? Explain why these pools are appropriate under CRD.

**(b)** Perform one round of Bayesian Bootstrap where: $Y_A(0)$ is drawn as 70, $Y_B(0)$ is drawn as 65, $Y_C(1)$ is drawn as 80, $Y_D(1)$ is drawn as 90. Compute the ATE for this imputation.

**(c)** Why is this approach better than simply plugging in the group means (single imputation)?

---

**E2. (5 points)** A clinical trial with $N = 200$ ($N_t = 100$, $N_c = 100$) yields:
- Fisher exact p-value (sharp null): $p = 0.03$
- Neyman 95% CI: $[0.5, 4.2]$
- Hodges-Lehmann estimate: $\hat{\tau}_{HL} = 2.1$

**(a)** Interpret each of these three results in one sentence.

**(b)** Are these three results consistent with each other? Why or why not?

**(c)** A colleague says "The Neyman CI is $[0.5, 4.2]$, which means there is a 95% probability that the true treatment effect is between 0.5 and 4.2." Is this statement correct? If not, provide the correct interpretation.

---

**END OF EXAM**
