# STAT 140 Midterm 2 · Mock Exam v2

**Instructions.** 7 problems, 90 minutes. Calculator allowed. Use $\pm 2\cdot SE$ for 95% CI. Show all steps, circle final answers. Problems cover: CRE · Matched Pair · Multi-Treatment CRE · Crossover · Block · Factorial $2^K$ (lex order + contrasts) · Fractional Factorial $2^{4-1}$ + Fisherian test statistics.

**说明**：每题都配了 **先英文后中文** 的双语详解。先自己做，做完再对答案。

---

# Problem 1 · Completely Randomized Experiment

A plant biologist runs a CRE with $N=10$ tomato plants to test a new growth hormone. $N_T=5$ plants are randomly assigned to treatment, $N_C=5$ to control. Observed yields (kg):

$$Y_T^{obs}=\{12,\ 15,\ 14,\ 13,\ 11\},\quad Y_C^{obs}=\{9,\ 10,\ 8,\ 11,\ 7\}.$$

**(a)** State the size of the randomization set $|W^+|$ and $P(W_i=1)$ for any single plant.

**(b)** State the sharp null hypothesis in words and in symbols.

**(c)** Compute $\hat\tau$ and the conservative 95% CI.

**(d)** Explain in one or two sentences why the variance estimator you used in (c) is called **conservative**.

**(e)** A TA claims: "Since $|W^+|=252$ is small, we can compute an exact Fisherian $p$-value." Describe the procedure (do not compute the final number). Could we do this for a $N=40, N_T=20$ experiment? Why or why not?

---

## ✅ Solution 1

### (a) Randomization set

**English.** $|W^+|=\binom{N}{N_T}=\binom{10}{5}=252$. Each assignment is equally likely, so $P(W)=1/252$. Marginally, $P(W_i=1)=N_T/N=5/10=1/2$ for any individual plant.

**中文。** $|W^+|=\binom{10}{5}=252$ 种合法分配，每种等概率。对单个植物而言 $P(W_i=1)=5/10=1/2$。

### (b) Sharp null

**English.** $H_0: Y_i(1)=Y_i(0)$ for every plant $i$ — the new hormone produces exactly the same yield as the control condition for **every individual plant** (not merely on average).

**中文。** $H_0: Y_i(1)=Y_i(0)$ 对每一棵植物都成立 —— 新激素对**每一棵植物个体**产量效果恰好为零（不是"平均为零"）。

### (c) Estimate and 95% CI

**English.**
- $\bar Y_T^{obs}=(12+15+14+13+11)/5=65/5=13$.
- $\bar Y_C^{obs}=(9+10+8+11+7)/5=45/5=9$.
- $\hat\tau=13-9=\boxed{4}$.

Sample variances ($N_j-1=4$):
- Treatment deviations from 13: $\{-1,2,1,0,-2\}$; squared sum $=1+4+1+0+4=10$. $s_1^2=10/4=2.5$.
- Control deviations from 9: $\{0,1,-1,2,-2\}$; squared sum $=0+1+1+4+4=10$. $s_0^2=10/4=2.5$.

Conservative variance:
$$\widehat{\mathrm{Var}}(\hat\tau)=\frac{s_1^2}{N_T}+\frac{s_0^2}{N_C}=\frac{2.5}{5}+\frac{2.5}{5}=0.5+0.5=1.0.$$
$SE=\sqrt{1.0}=1.0$. 95% CI: $4\pm 2(1.0)=\boxed{(2.0,\ 6.0)}$.

**中文。** $\bar Y_T=13,\ \bar Y_C=9,\ \hat\tau=4$。样本方差分母 $N_j-1=4$：两组平方和均为 10，所以 $s_1^2=s_0^2=2.5$。保守方差 $=2.5/5+2.5/5=1.0$，$SE=1.0$。95\% CI: $4\pm 2=(2,6)$。

### (d) Why conservative?

**English.** The **true** variance of $\hat\tau$ is $S_1^2/N_T+S_0^2/N_C-S_\tau^2/N$, where $S_\tau^2$ is the finite-population variance of the **unit-level treatment effects** $\tau_i=Y_i(1)-Y_i(0)$. Estimating $S_\tau^2$ requires knowing **both** potential outcomes for the same plant, which is impossible (each plant is observed under only one condition). We drop this non-negative term, so the reported variance is an **upper bound** on the true variance, giving CIs that are always at-least as wide as they should be (guarantees $\ge 95\%$ coverage).

**中文。** 真实方差 $=S_1^2/N_T+S_0^2/N_C-S_\tau^2/N$，其中 $S_\tau^2$ 是单元级效应 $\tau_i$ 的有限总体方差。要估计它需要**同一棵植物**在两种处理下的潜在结果 —— 根本无法观测。于是我们丢掉这一项（$\ge 0$），得到的方差是上界 $\Rightarrow$ CI 偏宽，保证覆盖率 $\ge 95\%$。

### (e) Fisherian procedure & scalability

**English.** Procedure for $N=10$: (1) State sharp null — under it, the 10 observed numbers $\{12,15,14,13,11,9,10,8,11,7\}$ are **fixed**. (2) Enumerate all $\binom{10}{5}=252$ ways to label 5 plants as "T" and 5 as "C". (3) For each labeling, compute $T(W)=\bar Y_T-\bar Y_C$. (4) The $p$-value is $\#\{|T(W)|\ge|T^{obs}|=4\}/252$.

For $N=40, N_T=20$: $\binom{40}{20}\approx 1.38\times 10^{11}$ — infeasible to enumerate. Use **Monte Carlo**: draw $B=10{,}000$ random assignments, compute $T^{rand}$ for each, approximate $p\approx\#\{|T^{rand}|\ge|T^{obs}|\}/B$.

**中文。** $N=10$ 过程：(1) 锐零下 10 个观测数字固定。(2) 枚举 $\binom{10}{5}=252$ 种"5T+5C"标签。(3) 每次重算 $T=\bar Y_T-\bar Y_C$。(4) $p=\#\{|T|\ge 4\}/252$。

$N=40$ 时 $\binom{40}{20}\approx 1.38\times 10^{11}$，无法枚举 $\Rightarrow$ **Monte Carlo**：随机抽 $B=10{,}000$ 种分配近似 $p$ 值。

---

# Problem 2 · Matched Pair Design

A nutritionist runs a matched-pair trial with $P=6$ pairs of twins. One twin gets a probiotic (T), the other placebo (C). Within-pair differences in gut-health score (T $-$ C) are:

$$d=\{2,\ 5,\ -1,\ 4,\ 3,\ 3\}.$$

**(a)** Describe the re-randomization procedure that defines $|W^+|$, and state $|W^+|$.

**(b)** Compute $\hat\tau$, $s_d^2$, and $\widehat{\mathrm{Var}}(\hat\tau)$. Give the 95% CI.

**(c)** Is the variance in (b) **exact** or **conservative**? Explain why, in 1–2 sentences, with reference to what is and is not observable.

**(d)** For the Fisherian test with statistic $T=\bar d$, compute $T^{obs}$, describe the reference set explicitly (size and how elements are generated), and state how you would compute the $p$-value.

**(e)** A student proposes using $\binom{12}{6}$ as the Fisherian reference set. Explain in one sentence why this is wrong.

---

## ✅ Solution 2

### (a) Re-randomization

**English.** Within each of the 6 pairs independently, flip a fair coin: one twin becomes T, the other C. Twins **cannot** be swapped across pairs. So $|W^+|=2^P=2^6=64$ possible assignments.

**中文。** 每对 6 组内部独立抛硬币，决定双胞胎哪一个作 T、哪一个作 C。双胞胎不能跨对互换。$|W^+|=2^6=64$ 种。

### (b) Estimate, variance, CI

**English.**
$\bar d=(2+5-1+4+3+3)/6=16/6=2.667.$

Deviations from $\bar d\approx 2.667$: $\{-0.667, 2.333, -3.667, 1.333, 0.333, 0.333\}$.
Squared: $\{0.444, 5.444, 13.444, 1.778, 0.111, 0.111\}$; sum $=21.333$.

$s_d^2=21.333/(P-1)=21.333/5\approx 4.267$.

$\widehat{\mathrm{Var}}(\hat\tau)=s_d^2/P=4.267/6\approx 0.711$. $SE\approx\sqrt{0.711}\approx 0.843$.

95\% CI: $2.667\pm 2(0.843)=\boxed{(0.98,\ 4.35)}$.

**中文。** $\bar d=16/6\approx 2.667$。偏差平方和 $\approx 21.33$，$s_d^2\approx 4.267$。$\widehat{\mathrm{Var}}\approx 0.711$，$SE\approx 0.843$。95\% CI: $2.667\pm 1.69\approx(0.98,4.35)$。

### (c) Exact or conservative?

**English.** **Exact.** The within-pair difference $d_p$ is **directly observable** for every pair (we literally measure it). Therefore $s_d^2$ is a legitimate sample variance of the $d_p$'s with nothing unobservable dropped, and $s_d^2/P$ is an unbiased estimate of $\mathrm{Var}(\hat\tau)$. Contrast with CRE, where $S_\tau^2/N$ must be dropped because both potential outcomes per unit are never observed.

**中文。** **精确**。$d_p$ 对每一对都直接观测到，所以 $s_d^2$ 是这些 $d_p$ 的合法样本方差，没有任何未观测量被丢掉。$s_d^2/P$ 是 $\mathrm{Var}(\hat\tau)$ 的无偏估计。与 CRE 对比：那里的 $S_\tau^2/N$ 因无法观测必须丢掉 $\Rightarrow$ 只能保守。

### (d) Fisherian test

**English.** $T^{obs}=\bar d=2.667$.

Reference set size = $2^P=64$. Each element is an **independent sign-flip pattern** $\epsilon=(\epsilon_1,\dots,\epsilon_6)\in\{-1,+1\}^6$. For a given $\epsilon$, compute $d_p^*=\epsilon_p|d_p|$ and $\bar d^*=\frac{1}{6}\sum d_p^*$. (This corresponds to "what if in pair $p$, the other twin had been labeled T?")

$p$-value: $p=\#\{|\bar d^*|\ge 2.667\}/64$. Since 64 is tiny, compute by exhaustive enumeration (a short Python/R loop). **No Monte Carlo needed.**

**中文。** $T^{obs}=\bar d=2.667$。参照集大小 $2^6=64$。每个元素是独立符号模式 $\epsilon\in\{-1,+1\}^6$；给定 $\epsilon$，$d_p^*=\epsilon_p|d_p|$，重算 $\bar d^*$。$p=\#\{|\bar d^*|\ge 2.667\}/64$。64 很小，直接枚举即可。

### (e) Why $\binom{12}{6}$ is wrong

**English.** $\binom{12}{6}=924$ is the CRE reference set — it would allow labeling any 6 of the 12 twins as "T", including swaps **across pairs**. But the matched-pair design explicitly forbids cross-pair swaps; only within-pair sign flips are valid. Using $\binom{12}{6}$ would produce the wrong null distribution and wrong $p$-value.

**中文。** $\binom{12}{6}=924$ 是 CRE 的参照集，会允许任意 6 人作 T（包括跨对互换）。但配对设计禁止跨对互换，只允许对内翻符号。用 $\binom{12}{6}$ 会给出错误的零分布和错误的 $p$ 值。

---

# Problem 3 · Multi-Treatment CRE with ANOVA

A tech company tests $J=3$ landing-page designs (A, B, C) on $N=12$ users (4 per arm). Observed time-on-page (seconds):

$$\text{A}:\{20,22,24,22\},\quad\text{B}:\{26,30,28,28\},\quad\text{C}:\{35,33,34,34\}.$$

**(a)** Compute $|W^+|$.

**(b)** Build the full ANOVA table (Source · df · SS · MS) and compute $F$.

**(c)** Compute $\hat\tau(C,A)=\bar Y_C-\bar Y_A$ and its conservative 95% CI.

**(d)** Explain, in 2–3 sentences, why we might prefer to gate on the $F$-test before computing pairwise CIs.

---

## ✅ Solution 3

### (a) Randomization set

**English.** $|W^+|=\dfrac{12!}{4!\,4!\,4!}=\dfrac{479{,}001{,}600}{24\cdot 24\cdot 24}=\dfrac{479{,}001{,}600}{13{,}824}=34{,}650.$

**中文。** $|W^+|=\dfrac{12!}{4!\cdot 4!\cdot 4!}=34{,}650$ 种分组。

### (b) ANOVA table

**English.**
Arm means:
- $\bar Y_A=(20+22+24+22)/4=88/4=22$
- $\bar Y_B=(26+30+28+28)/4=112/4=28$
- $\bar Y_C=(35+33+34+34)/4=136/4=34$

Grand mean $\bar Y=(22+28+34)/3=28$ (because $N_j$ are equal).

$SS_{Treat}=\sum_j N_j(\bar Y_j-\bar Y)^2=4[(22-28)^2+(28-28)^2+(34-28)^2]=4[36+0+36]=288$. df $=J-1=2$.

$SS_{Res}$ (within-arm):
- A: dev $\{-2,0,2,0\}\Rightarrow$ sum $=4+0+4+0=8$.
- B: dev $\{-2,2,0,0\}\Rightarrow 4+4+0+0=8$.
- C: dev $\{1,-1,0,0\}\Rightarrow 1+1+0+0=2$.
- Total $SS_{Res}=8+8+2=18$. df $=N-J=9$.

| Source | df | SS | MS |
|---|---|---|---|
| Treatments | 2 | 288 | 144 |
| Residuals | 9 | 18 | 2 |
| Total | 11 | 306 | — |

$F=MS_{Treat}/MS_{Res}=144/2=\boxed{72}$.

**中文。** $\bar Y_A=22,\bar Y_B=28,\bar Y_C=34$，总均值 $28$。$SS_{Treat}=4\cdot 72=288$，df $=2$。$SS_{Res}$：A 组 $=8$，B 组 $=8$，C 组 $=2$，合计 $=18$，df $=9$。$MS_{Treat}=144,MS_{Res}=2,F=72$。

### (c) Pairwise contrast

**English.**
$\hat\tau(C,A)=\bar Y_C-\bar Y_A=34-22=12.$

Within-arm sample variances (divide by $N_j-1=3$):
- $s_A^2=8/3\approx 2.667$
- $s_C^2=2/3\approx 0.667$

$\widehat{\mathrm{Var}}(\hat\tau(C,A))=s_C^2/N_C+s_A^2/N_A=0.667/4+2.667/4=0.167+0.667=0.833.$
$SE\approx\sqrt{0.833}\approx 0.913$. 95\% CI: $12\pm 2(0.913)=\boxed{(10.17,\ 13.83)}$.

**中文。** $\hat\tau(C,A)=34-22=12$。$s_A^2=8/3,s_C^2=2/3$。方差 $=2/3\cdot 1/4+8/3\cdot 1/4=10/12\approx 0.833$，$SE\approx 0.913$。95\% CI: $12\pm 1.83=(10.17,13.83)$。

### (d) Why gate on $F$?

**English.** With $J=3$ arms there are $\binom{3}{2}=3$ possible pairwise tests. If we perform all 3 at $\alpha=0.05$ independently, the family-wise Type I error inflates to $1-(1-0.05)^3\approx 0.143$. The $F$-test provides a single **global** check at level $\alpha$: only if $F$ rejects do we drill into pairwise comparisons. This controls the family-wise error and prevents "mining" for significant pairs by chance.

**中文。** $J=3$ 有 3 对成比较。若直接用 $\alpha=0.05$ 做 3 次独立检验，族错误率膨胀到 $1-(1-0.05)^3\approx 0.14$。$F$ 检验先给一个全局检验：只有 $F$ 拒绝才进一步做两两比较 $\Rightarrow$ 控制族错误率，避免因偶然性"挖"出显著对。

---

# Problem 4 · Crossover Design

A sleep-lab trial tests a new relaxation app. $N=5$ subjects each spend two nights in the lab, one with the app (T) and one without (C), with the order chosen by an independent coin flip for each subject. Observed hours of sleep:

| Subject $i$ | $Y_i^T$ | $Y_i^C$ |
|---|---|---|
| 1 | 7.5 | 6.0 |
| 2 | 6.8 | 5.5 |
| 3 | 7.0 | 6.5 |
| 4 | 8.0 | 7.0 |
| 5 | 6.5 | 6.0 |

**(a)** State the two substantive assumptions required for this design to be valid.

**(b)** Compute $d_i$ for each subject, then $\bar d$, $s_D^2$, and the Fisherian test statistic $T_{paired}=\bar d/(s_D/\sqrt N)$.

**(c)** Explain in words what the sharp null means in this context and, under it, why the sign of each $d_i$ is uniformly $\pm 1$.

**(d)** Explain why in a crossover design the unit-level effect $\tau_i$ is estimable, and contrast with the CRE where it is not.

**(e)** Describe precisely how to compute a two-sided Fisherian $p$-value using $T_{paired}$ (size of reference set, enumeration vs Monte Carlo).

---

## ✅ Solution 4

### (a) Assumptions

**English.**
1. **No carryover effect.** Using the app in period 1 does not affect the sleep outcome in period 2 (e.g., the app's calming effect wears off overnight). If violated, the "no-app night" observation is still partly under the app's influence, so $d_i$ no longer isolates the treatment effect.
2. **No time / period effect.** The two nights are otherwise exchangeable — no learning, adaptation, weekday/weekend difference, or order-specific trend. If period 2 is systematically better/worse for unrelated reasons (e.g., subjects sleep better on the second night of a lab stay), $d_i$ is contaminated.

**中文。**
1. **无延滞效应 (no carryover)**：第 1 晚的 app 不影响第 2 晚的睡眠。若违反，"无 app" 的那晚其实仍部分受 app 影响，$d_i$ 不再是纯处理效应。
2. **无时段效应 (no time/period effect)**：两晚除顺序外可互换（无学习、适应、工作日/周末差异等）。若第 2 晚系统性更好/更差，$d_i$ 被污染。

### (b) Computations

**English.**
| $i$ | $Y_i^T$ | $Y_i^C$ | $d_i=Y_i^T-Y_i^C$ |
|---|---|---|---|
| 1 | 7.5 | 6.0 | 1.5 |
| 2 | 6.8 | 5.5 | 1.3 |
| 3 | 7.0 | 6.5 | 0.5 |
| 4 | 8.0 | 7.0 | 1.0 |
| 5 | 6.5 | 6.0 | 0.5 |

$\bar d=(1.5+1.3+0.5+1.0+0.5)/5=4.8/5=\boxed{0.96}$.

Deviations from $\bar d=0.96$: $\{0.54, 0.34, -0.46, 0.04, -0.46\}$.
Squared: $\{0.2916, 0.1156, 0.2116, 0.0016, 0.2116\}$; sum $=0.832$.

$s_D^2=0.832/(N-1)=0.832/4=0.208$. $s_D\approx 0.456$.

$s_D/\sqrt N=0.456/\sqrt 5=0.456/2.236\approx 0.204$.

$T_{paired}=0.96/0.204\approx\boxed{4.71}$.

**中文。** $d=\{1.5,1.3,0.5,1.0,0.5\}$，$\bar d=0.96$。偏差平方和 $=0.832$，$s_D^2=0.208$，$s_D\approx 0.456$。$s_D/\sqrt 5\approx 0.204$。$T_{paired}\approx 0.96/0.204\approx 4.71$。

### (c) Sharp null meaning

**English.** Sharp null: $Y_{i,j}(1)=Y_{i,j}(0)$ for every subject $i$ and every night $j\in\{1,2\}$ — the app has **exactly zero effect** on sleep hours for every subject on every night (not merely in aggregate). Under this null, whichever night is labeled "T" is irrelevant: the two observed numbers for each subject are fixed, and $d_i=($night labeled T$)-($night labeled C$)$ depends only on the random coin flip. Since the flip is 50/50, $d_i$ takes values $+|d_i|$ and $-|d_i|$ with equal probability — i.e., the **sign** of $d_i$ is a fair coin flip.

**中文。** 锐零 $Y_{i,j}(1)=Y_{i,j}(0)$ 对每人每晚都成立 —— app 对每人每晚的睡眠时长影响恰好为零。锐零下，哪晚标 T 哪晚标 C 无关紧要：两个观测数字固定，$d_i$ 的符号只取决于那次 50/50 的硬币。因此 $d_i$ 的**符号**是公平硬币。

### (d) Unit-level effect

**English.** In a crossover, each subject is observed under **both** treatment conditions (one per night, assuming the two assumptions hold). So the within-subject difference $d_i=Y_i^T-Y_i^C$ directly gives the unit-level effect $\tau_i$ for that individual — no missing potential outcome to impute. In a CRE, each subject is observed under only **one** condition, so one of $\{Y_i(1), Y_i(0)\}$ is forever missing and $\tau_i$ can never be estimated for a specific individual.

**中文。** 交叉设计中每人观测了两种条件（前提是无延滞/无时段），所以 $d_i=Y_i^T-Y_i^C$ 直接给出 $\tau_i$，无缺失潜在结果。CRE 中每人只观测一种条件，$\{Y_i(1),Y_i(0)\}$ 必有一个永远缺失，$\tau_i$ 永远估不出。

### (e) Fisherian $p$-value

**English.** Reference set has $|W^+|=2^N=2^5=32$ sign-flip patterns. Procedure:
1. For each $\epsilon=(\epsilon_1,\dots,\epsilon_5)\in\{-1,+1\}^5$, form $d_i^*=\epsilon_i|d_i|$.
2. Compute $\bar d^*$, $s_D^*$, then $T_{paired}^{rand}=\bar d^*/(s_D^*/\sqrt 5)$.
3. Two-sided $p$-value: $p=\#\{|T_{paired}^{rand}|\ge|T_{paired}^{obs}|=4.71\}/32$.

Since 32 is tiny, **exact enumeration** is easy — no Monte Carlo needed.

**中文。** 参照集 $2^5=32$ 个符号模式。对每个 $\epsilon$：$d_i^*=\epsilon_i|d_i|$；重算 $\bar d^*, s_D^*, T^{rand}$。双侧 $p=\#\{|T^{rand}|\ge 4.71\}/32$。32 小，直接枚举。

---

# Problem 5 · Block Design with Effect Modification

An education researcher tests a new math app on $N=20$ students in $B=2$ blocks by age group: **younger** $N(y)=12$ (8 T, 4 C), **older** $N(o)=8$ (4 T, 4 C). Observed mean post-test scores and within-arm sample variances:

| Block | $\bar Y_T^{obs}(k)$ | $\bar Y_C^{obs}(k)$ | $s_T^2(k)$ | $s_C^2(k)$ |
|---|---|---|---|---|
| Younger ($y$) | 72 | 65 | 16 | 16 |
| Older ($o$) | 80 | 78 | 9 | 9 |

**(a)** Compute $|W^+|$ for this blocked design.

**(b)** Compute the population-weighted estimator $\hat\tau$ and its conservative 95% CI.

**(c)** A TA claims the reference set for a Fisherian test is $\binom{20}{12}$. Explain in 2 sentences why this is wrong and give the correct size.

**(d)** Test for **effect modification**: compute $\hat\Delta=\hat\tau(y)-\hat\tau(o)$, its conservative variance, and its 95% CI. Based on the CI, is there statistical evidence that the app works differently for the two age groups?

**(e)** A colleague asks: "If we just care about the overall effect (part b), why bother testing effect modification?" Answer in 1–2 sentences.

---

## ✅ Solution 5

### (a) Randomization set

**English.** The design is two independent CREs, one per block.
$$|W^+|=\binom{N(y)}{N_t(y)}\binom{N(o)}{N_t(o)}=\binom{12}{8}\binom{8}{4}=495\cdot 70=\boxed{34{,}650}.$$

**中文。** 两个独立的块内 CRE。$|W^+|=\binom{12}{8}\binom{8}{4}=495\cdot 70=34{,}650$ 种。

### (b) Overall ATE

**English.**
Block effects:
- $\hat\tau(y)=72-65=7$
- $\hat\tau(o)=80-78=2$

Weights:
- $\lambda_y=N(y)/N=12/20=0.6$
- $\lambda_o=N(o)/N=8/20=0.4$

$\hat\tau=0.6(7)+0.4(2)=4.2+0.8=\boxed{5.0}.$

Variance contributions:
- Younger: $\lambda_y^2[s_T^2/N_t+s_C^2/N_c]=0.36[16/8+16/4]=0.36[2+4]=0.36\cdot 6=2.16$.
- Older: $\lambda_o^2[9/4+9/4]=0.16[2.25+2.25]=0.16\cdot 4.5=0.72$.

$\widehat{\mathrm{Var}}(\hat\tau)=2.16+0.72=2.88$. $SE\approx\sqrt{2.88}\approx 1.697$.

95\% CI: $5.0\pm 2(1.697)=\boxed{(1.61,\ 8.39)}$.

**中文。** $\hat\tau(y)=7,\hat\tau(o)=2$，$\lambda_y=0.6,\lambda_o=0.4$。$\hat\tau=0.6(7)+0.4(2)=5.0$。方差：年轻块贡献 $(0.6)^2(2+4)=2.16$；年长块贡献 $(0.4)^2(2.25+2.25)=0.72$。合计 $2.88$，$SE\approx 1.70$，CI $=5.0\pm 3.39=(1.61,8.39)$。

### (c) Wrong reference set

**English.** $\binom{20}{12}$ treats the 20 students as one big CRE pool, allowing a younger student's label to swap to an older student — but the blocked design forbids such cross-block swaps. The correct reference set re-randomizes **only within each block**, giving $\binom{12}{8}\binom{8}{4}=34{,}650$ (much smaller than $\binom{20}{12}=125{,}970$).

**中文。** $\binom{20}{12}$ 是把 20 人当成一个大 CRE，会允许年轻人的标签被换到年长人，违反分块设计。正确参照集是**块内重随机化**：$\binom{12}{8}\binom{8}{4}=34{,}650$，远小于 $\binom{20}{12}=125{,}970$。

### (d) Effect modification

**English.**
$\hat\Delta=\hat\tau(y)-\hat\tau(o)=7-2=\boxed{5}.$

Since blocks are independent, variances **add directly** (no $\lambda^2$ weighting, because we're estimating a difference of block effects, not a weighted average):
$$\widehat{\mathrm{Var}}(\hat\Delta)=\left[\tfrac{s_T^2(y)}{N_t(y)}+\tfrac{s_C^2(y)}{N_c(y)}\right]+\left[\tfrac{s_T^2(o)}{N_t(o)}+\tfrac{s_C^2(o)}{N_c(o)}\right]=[2+4]+[2.25+2.25]=6+4.5=10.5.$$

$SE\approx\sqrt{10.5}\approx 3.240$. 95\% CI: $5\pm 2(3.240)=\boxed{(-1.48,\ 11.48)}$.

The CI **includes 0** $\Rightarrow$ at the 95% level, we have **no statistical evidence** that the effect differs across age groups (even though the point estimate $\hat\Delta=5$ is sizable — the data are too noisy / sample is too small to be confident).

**中文。** $\hat\Delta=7-2=5$。块独立 $\Rightarrow$ 方差直接相加（没有 $\lambda^2$，因为这是两个块效应之差，不是加权平均）：$\widehat{\mathrm{Var}}=6+4.5=10.5$，$SE\approx 3.24$，CI $=5\pm 6.48=(-1.48,11.48)$。**CI 含 0** $\Rightarrow$ 95\% 水平下没有证据说效应随年龄组变化（尽管点估计 5 不小，但样本噪声太大，不够显著）。

### (e) Why bother testing effect modification?

**English.** The overall ATE gives a **population-level summary** but hides heterogeneity — e.g., a treatment that works wonderfully for younger students but is useless for older students could still produce a moderate "average" effect. Effect-modification analysis diagnoses such heterogeneity, which matters for **policy and targeting**: if the effect is very different across groups, a one-size-fits-all recommendation is misleading.

**中文。** 总体 ATE 只给群体层面的汇总，掩盖异质性 —— 一个对年轻人非常有效、对年长人完全无效的干预，整体平均下来可能仍是中等正效应。测效应修饰能诊断这种异质性，对**政策与精准投放**至关重要：若效应在组间差异巨大，"一刀切"的推荐就具有误导性。

---

# Problem 6 · Factorial $2^3$ Design

A materials engineer runs a $2^3$ factorial: $F_1$ = cure temperature, $F_2$ = pressure, $F_3$ = additive type, each at two levels coded $\{-1,+1\}$. Each of the 8 treatment combinations is replicated $n=3$ times ($N=24$ total) and mean hardness scores (in lex order) are:

$$\bar Y^{obs}=(30,\ 32,\ 34,\ 40,\ 36,\ 42,\ 46,\ 56).$$

The pooled within-cell sample variance is $\hat S^2=6$.

**(a)** Write the lex-order design matrix and write out $g_{F_1}, g_{F_2}, g_{F_3}$.

**(b)** Using the product rule, derive $g_{F_1F_2}$ and $g_{F_1F_2F_3}$, and verify one of them has exactly 4 pluses and 4 minuses.

**(c)** Compute $\hat\tau_{F_1}$, $\hat\tau_{F_2}$, $\hat\tau_{F_3}$, and the two-way interaction $\hat\tau_{F_1F_2}$.

**(d)** Compute the Neymanian conservative 95% CI for $\tau_{F_1F_2}$.

**(e)** A student asks: "What does $\hat\tau_{F_1F_2}$ represent in plain language, and why is interpreting $\hat\tau_{F_1}$ tricky if $\hat\tau_{F_1F_2}$ is large?" Answer in 2–3 sentences.

**(f)** Describe how to obtain a Fisherian $p$-value for $H_0:\tau_{F_1}=0$ using $\hat\tau_{F_1}$ as the test statistic. Why is Monte Carlo required rather than full enumeration?

---

## ✅ Solution 6

### (a) Lex-order matrix & single-factor contrasts

**English.**
| Run | $F_1$ | $F_2$ | $F_3$ |
|---|---|---|---|
| 1 | $-$ | $-$ | $-$ |
| 2 | $-$ | $-$ | $+$ |
| 3 | $-$ | $+$ | $-$ |
| 4 | $-$ | $+$ | $+$ |
| 5 | $+$ | $-$ | $-$ |
| 6 | $+$ | $-$ | $+$ |
| 7 | $+$ | $+$ | $-$ |
| 8 | $+$ | $+$ | $+$ |

$g_{F_1}=(-,-,-,-,+,+,+,+),\ g_{F_2}=(-,-,+,+,-,-,+,+),\ g_{F_3}=(-,+,-,+,-,+,-,+).$

**中文。** 字典序矩阵如上。主效应对比向量直接抄列：$g_{F_1},g_{F_2},g_{F_3}$ 如上。

### (b) Interactions

**English.** Hadamard product (elementwise, "same sign $\to +$, different sign $\to -$"):
$g_{F_1F_2}=g_{F_1}\odot g_{F_2}=(+,+,-,-,-,-,+,+).$
$g_{F_1F_2F_3}=g_{F_1}\odot g_{F_2}\odot g_{F_3}=(-,+,+,-,+,-,-,+).$

Verify $g_{F_1F_2}$: count $+$ signs = 4 (rows 1,2,7,8), count $-$ = 4 (rows 3,4,5,6). ✓

**中文。** Hadamard 乘积（同号得 +，异号得 −）：$g_{F_1F_2}=(+,+,-,-,-,-,+,+)$，$g_{F_1F_2F_3}=(-,+,+,-,+,-,-,+)$。$g_{F_1F_2}$ 有 4 个 + 和 4 个 −，检验通过。

### (c) Effect estimates

**English.** Divisor $2^{K-1}=2^2=4$.

- $g_{F_1}^\top\bar Y=-30-32-34-40+36+42+46+56=44\Rightarrow \hat\tau_{F_1}=44/4=\boxed{11}$.
- $g_{F_2}^\top\bar Y=-30-32+34+40-36-42+46+56=36\Rightarrow \hat\tau_{F_2}=36/4=\boxed{9}$.
- $g_{F_3}^\top\bar Y=-30+32-34+40-36+42-46+56=24\Rightarrow \hat\tau_{F_3}=24/4=\boxed{6}$.
- $g_{F_1F_2}^\top\bar Y=+30+32-34-40-36-42+46+56=12\Rightarrow \hat\tau_{F_1F_2}=12/4=\boxed{3}$.

**中文。** 除数 $=4$。
$\hat\tau_{F_1}=44/4=11$，$\hat\tau_{F_2}=36/4=9$，$\hat\tau_{F_3}=24/4=6$，$\hat\tau_{F_1F_2}=12/4=3$。

### (d) 95% CI for $\tau_{F_1F_2}$

**English.** The Neymanian conservative variance is the same for every effect:
$$\widehat{\mathrm{Var}}(\hat\tau_F)=\tfrac{2^K}{N}\hat S^2=\tfrac{8}{24}\cdot 6=\tfrac{48}{24}=2.$$
$SE=\sqrt 2\approx 1.414$.
95\% CI for $\tau_{F_1F_2}$: $3\pm 2(1.414)=\boxed{(0.17,\ 5.83)}$.

**中文。** $\widehat{\mathrm{Var}}(\hat\tau_F)=\tfrac{2^K}{N}\hat S^2=\tfrac{8}{24}\cdot 6=2$，$SE\approx 1.414$。$\tau_{F_1F_2}$ 的 95\% CI: $3\pm 2\cdot 1.414=(0.17,5.83)$（**不含 0**，有显著交互证据）。

### (e) Interpretation of interaction & caveat for main effect

**English.** $\hat\tau_{F_1F_2}=3$ means the effect of $F_1$ (temperature) **depends on** the level of $F_2$ (pressure): specifically, the temperature effect is about $2\cdot\hat\tau_{F_1F_2}=6$ units larger at high pressure than at low pressure (equivalently, the pressure effect is 6 units larger at high temperature). When the interaction is large, the "main effect" $\hat\tau_{F_1}=11$ is only an **average over the two pressure levels** — at low pressure the true temperature effect is $\hat\tau_{F_1}-\hat\tau_{F_1F_2}=8$, while at high pressure it's $\hat\tau_{F_1}+\hat\tau_{F_1F_2}=14$. So a single "main-effect of temperature = 11" oversimplifies the real structure.

**中文。** $\hat\tau_{F_1F_2}=3$ 意味着温度的效应**依赖于**压力水平：高压下的温度效应比低压下高约 $2\cdot 3=6$ 单位（等价地：高温下的压力效应比低温下高 6 单位）。交互大的时候，主效应 $\hat\tau_{F_1}=11$ 只是**在两个压力水平上的平均**：低压下真实温度效应 $=11-3=8$，高压下 $=11+3=14$。用单一数字"温度主效应 11"会掩盖真实结构。

### (f) Fisherian $p$-value

**English.** Under the sharp null $H_0:\tau_{iF}=0$ for all units and all factor effects, each of the $N=24$ units has identical potential outcomes across the 8 cells; the 24 observed numbers are **fixed**, only their cell labels are random. Procedure:
1. Keep the 24 observed hardness values fixed.
2. Repeat $B=10{,}000$ times: randomly re-assign the 24 units to the 8 cells (3 units per cell); recompute the 8 cell means $\bar Y^{rand}$; compute $\hat\tau_{F_1}^{rand}=\tfrac{1}{4}g_{F_1}^\top\bar Y^{rand}$.
3. Two-sided $p$-value: $p=\#\{|\hat\tau_{F_1}^{rand}|\ge|\hat\tau_{F_1}^{obs}|=11\}/B$.

Full enumeration requires $\dfrac{24!}{(3!)^8}\approx 3.2\times 10^{17}$ assignments — astronomically infeasible. Monte Carlo with $B=10{,}000$ gives a perfectly usable approximation.

**中文。** 锐零下每个单元在 8 个 cell 下潜在结果相同；24 个观测数字固定，cell 标签随机。步骤：(1) 保持 24 数不变。(2) 重复 $B=10{,}000$ 次：随机把 24 单元分到 8 cell（每 cell 3 个），重算 8 个 cell 均值，再算 $\hat\tau_{F_1}^{rand}=g_{F_1}^\top\bar Y^{rand}/4$。(3) 双侧 $p=\#\{|\hat\tau_{F_1}^{rand}|\ge 11\}/B$。完整空间 $24!/(3!)^8\approx 3.2\times 10^{17}$ 无法枚举，故用 MC。

---

# Problem 7 · Fractional Factorial $2^{4-1}$

A biotech company screens 4 two-level factors $A, B, C, D$ on protein yield. A full $2^4=16$-run experiment is too expensive, so they use the half-fraction $2^{4-1}$ with generator $D=ABC$ (i.e., defining relation $I=ABCD$).

**(a)** Write the 8-run design matrix in lex order on $(A,B,C)$ with column $D$ generated.

**(b)** Using $I=ABCD$, derive the alias of $AB$. What is the resolution of this design, and what is its practical implication for interpreting main effects?

**(c)** The observed responses in the order of part (a) are
$$\bar Y^{obs}=(50,\ 54,\ 55,\ 61,\ 52,\ 56,\ 60,\ 68).$$
Compute $\hat\tau_A$. If we assume the 3-way interaction $BCD$ is negligible, what does $\hat\tau_A$ approximately represent?

**(d)** A colleague proposes using generator $D=AB$ instead (defining relation $I=ABD$). What resolution would that be, and why is it a worse choice?

---

## ✅ Solution 7

### (a) Design matrix

**English.**
| # | $A$ | $B$ | $C$ | $D=ABC$ |
|---|---|---|---|---|
| 1 | $-$ | $-$ | $-$ | $-$ |
| 2 | $-$ | $-$ | $+$ | $+$ |
| 3 | $-$ | $+$ | $-$ | $+$ |
| 4 | $-$ | $+$ | $+$ | $-$ |
| 5 | $+$ | $-$ | $-$ | $+$ |
| 6 | $+$ | $-$ | $+$ | $-$ |
| 7 | $+$ | $+$ | $-$ | $-$ |
| 8 | $+$ | $+$ | $+$ | $+$ |

Check: $I=ABCD$ means every row's 4-column product equals $+1$. Row 2: $(-)(-)(+)(+)=+1$ ✓. Row 7: $(+)(+)(-)(-)=+1$ ✓.

**中文。** 按 $A,B,C$ 字典序写，$D=A\odot B\odot C$。每行 $ABCD$ 之积 $=+1$ 是一致性检查。

### (b) Alias of $AB$ and resolution

**English.** From $I=ABCD$, multiply both sides by $AB$ (using $A^2=B^2=I$):
$$AB\cdot I=AB\cdot ABCD=A^2B^2CD=CD.$$
So $\boxed{AB\equiv CD}$ — the 2-way interaction $AB$ is indistinguishable from the 2-way interaction $CD$ in this fraction.

The defining relation $I=ABCD$ has word length 4, so the design is **Resolution IV**. Practical implication:
- Main effects are **clear** of all 2-factor interactions (e.g., $A\equiv BCD$, a 3fi, typically negligible).
- Main effects are confounded with 3-factor interactions (OK under the usual sparsity-of-effects assumption).
- 2-factor interactions are confounded **with each other** ($AB\equiv CD$, $AC\equiv BD$, $AD\equiv BC$) — we cannot separate these pairs from 8 runs alone, which is the price of halving the experiment.

**中文。** $I=ABCD$ 两边乘 $AB$：$AB\cdot I=AB\cdot ABCD=CD$（用 $A^2=B^2=I$）$\Rightarrow AB\equiv CD$。
分辨度：定义关系最短词长 $=4 \Rightarrow$ **Resolution IV**。含义：主效应与任何 2fi 不混淆（$A\equiv BCD$，3fi 通常可忽略）；但 2fi 之间互相混淆（$AB\equiv CD$ 等），8 次运行无法分离 —— 这是半分式的代价。

### (c) Estimating $\hat\tau_A$

**English.** Treat the 8-run design as a full $2^3$ in $(A,B,C)$ — divisor is $2^{(K-1)-1}$ where we treat this as a $2^3$ on 8 runs, so divisor $=2^{3-1}=4$. $g_A=(-,-,-,-,+,+,+,+)$:

$g_A^\top\bar Y=-50-54-55-61+52+56+60+68=16.$

$\hat\tau_A=16/4=\boxed{4}.$

**Interpretation.** Because $A\equiv BCD$ in this fraction, strictly speaking $\hat\tau_A$ estimates $\tau_A+\tau_{BCD}$. Under the **sparsity-of-effects** assumption (3-way interactions are typically small), $\tau_{BCD}\approx 0$, so $\hat\tau_A\approx\tau_A$: on average, response rises by 4 units when $A$ flips from $-$ to $+$.

**中文。** 以 8 次运行当成 $2^3$ 来算主效应，除数 $=2^{3-1}=4$。$g_A^\top\bar Y=16\Rightarrow\hat\tau_A=16/4=4$。
含义：由于 $A\equiv BCD$，严格上估计的是 $\tau_A+\tau_{BCD}$。在"高阶效应稀疏"的常见假设下 $\tau_{BCD}\approx 0$，所以 $\hat\tau_A\approx\tau_A$：当 $A$ 从 − 切换到 + 时，响应平均升高 4 个单位。

### (d) Why $D=AB$ is worse

**English.** Defining relation $I=ABD$ has word length 3, giving **Resolution III**. Multiplying both sides by $A$: $A\cdot I=A\cdot ABD=BD$, so $A\equiv BD$ — a **main effect is aliased with a 2-factor interaction**. If the 2fi $BD$ is non-negligible (which cannot be ruled out a priori), the estimate of $A$'s main effect is contaminated. In Resolution IV (our original choice), main effects are clear of 2fi's, which is much safer. Rule of thumb: prefer higher resolution when possible (Res V > Res IV > Res III).

**中文。** $I=ABD$ 最短词长 $=3\Rightarrow$ **Resolution III**。两边乘 $A$：$A\equiv BD$，主效应与 2fi 混淆。若 $BD$ 不可忽略，$A$ 的主效应估计被污染。相比之下 Res IV（原方案）主效应与所有 2fi 不混淆，更安全。经验法则：优先选高分辨度（Res V > Res IV > Res III）。

---

# 🎯 答题要点回顾 / Post-Exam Debrief

1. **Always identify the design first** — everything else follows.
2. **Sharp null wording**: "for every unit $i$", not "on average".
3. **Variance type**: conservative (CRE, Multi-Trt, Block, Factorial) vs exact (Matched Pair, Crossover).
4. **Reference set**: design-specific, not always $\binom{N}{N_T}$ — Matched Pair $2^P$, Block $\prod\binom{N(k)}{N_t(k)}$, Crossover $2^N$, Factorial huge (MC).
5. **Weights in Block**: population $N(k)/N$, not $1/B$.
6. **Factorial**: lex order → $g_F$ → Hadamard product → estimator $\hat\tau_F = g_F^\top\bar Y / 2^{K-1}$.
7. **Fractional**: generator → defining relation → alias table → resolution → interpret $\hat\tau$ as $\tau + $ aliased effect.

祝考试顺利！ Good luck on the exam!
