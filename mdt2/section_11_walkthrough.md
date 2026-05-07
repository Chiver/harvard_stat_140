# 📘 STAT 140 Section 11 · 六大设计 · 手把手走一遍
## 每一章的节奏 / Format per chapter:
1. **🧪 设计一次具体实验** — 编一组真数据
2. **🚶 从头走到尾** — 每一步都用那组数字亲手算一遍（包括 Fisherian 枚举 + Neymanian 估计+方差+CI），走完你就懂这个设计了
3. **📝 独立练习题** (英文) — 带多个小题覆盖不同子概念
4. **✅ 解答** — 先英文完整解，紧跟中文对照

---

# Chapter 1 · Completely Randomized Design (CRE)

## 🧪 实验设计

我手上有 **$N=6$ 块地**，编号 $i=1,\dots,6$。我要测试"新肥料 vs 标准肥料"，用 CRE：从 6 块中抽 $N_T=3$ 块给新肥料，其余 $N_C=3$ 块给标准。

## 🚶 手把手走一遍

### 步骤 1 · 写下假想的科学表 (Science Table)

**先假装上帝视角**，看看每块地的两个潜在结果（考试不会给你这个，但理解它至关重要）：

| 地块 $i$ | $Y_i(1)$ 新肥料 | $Y_i(0)$ 标准 | 真效应 $\tau_i$ |
|---|---|---|---|
| 1 | 30 | 25 | 5 |
| 2 | 32 | 28 | 4 |
| 3 | 28 | 22 | 6 |
| 4 | 34 | 30 | 4 |
| 5 | 26 | 21 | 5 |
| 6 | 30 | 24 | 6 |

**真的平均因果效应** $\tau=\frac1N\sum\tau_i=(5+4+6+4+5+6)/6=30/6=\boxed{5}$。

但是——**因果推断的根本问题**——我们**永远只能看到一列**（取决于谁被分到处理组）。

### 步骤 2 · 分配机制 / Assignment mechanism

$$|W^+|=\binom{6}{3}=20,\quad P(W)=\frac{1}{20},\quad P(W_i=1)=\frac{N_T}{N}=\frac12.$$

想像把 20 种分组全写出来。**每一种等概率抽到 1/20**。

### 步骤 3 · 自然抽到一组

假设随机抽到的 $W=(1,1,0,1,0,0)$，即地块 {1,2,4} 处理，{3,5,6} 对照。因此**观测到**的 6 个数字是：

| $i$ | $W_i$ | $Y_i^{obs}$ (从科学表读) |
|---|---|---|
| 1 | 1 | 30 |
| 2 | 1 | 32 |
| 3 | 0 | 22 |
| 4 | 1 | 34 |
| 5 | 0 | 21 |
| 6 | 0 | 24 |

### 步骤 4 · Neymanian 估计 + CI

$$\bar Y_T^{obs}=\frac{30+32+34}{3}=32,\qquad \bar Y_C^{obs}=\frac{22+21+24}{3}=\frac{67}{3}\approx 22.33.$$
$$\hat\tau=\bar Y_T^{obs}-\bar Y_C^{obs}=32-22.33=\boxed{9.67}.$$

（注意：估计值 $9.67$ 和真值 $\tau=5$ 有差距 — 这就是"无偏但不精确"的含义。）

**样本方差** (分母 $N_j-1=2$)：
- 处理组偏差 $\{-2,0,2\}$ → 平方和 $=8$ → $s_1^2=8/2=4$
- 对照组偏差 $\{-0.33, -1.33, 1.67\}$ → 平方和 $\approx 0.111+1.778+2.778=4.667$ → $s_0^2=4.667/2\approx 2.333$

**保守估计方差**：
$$\widehat{\mathrm{Var}}(\hat\tau)=\frac{s_1^2}{N_T}+\frac{s_0^2}{N_C}=\frac{4}{3}+\frac{2.333}{3}\approx 1.333+0.778=2.111.$$
$SE=\sqrt{2.111}\approx 1.453$。**95% CI**：$9.67\pm 2(1.453)\approx (6.76,\,12.58)$。

> 💡 **留意**：真值 $\tau=5$ **没有**落在这个 CI 内！这就是随机化的现实——在这一次抽到的 $W$ 下估计偏高，但如果跑 2000 次实验，约 95% 次 CI 能覆盖真值。

### 步骤 5 · Fisherian 推断

**锐零假设 / Sharp null**: $H_0: Y_i(1)=Y_i(0)$ 对每块地 $i$（不是"平均为 0"，是"每一块都 0"）。

锐零下，6 个观测数字 $\{30,32,22,34,21,24\}$ **固定不变**；只是它们被分成哪 3 个处理、哪 3 个对照是随机的。

**检验统计量**: $T_{dm}=\bar Y_T-\bar Y_C$，观测值 $T^{obs}=9.67$。

**参照集大小** $=\binom{6}{3}=20$。让我来**真的枚举几种**让你看看过程：

| 3 个"T"是哪几个 $i$ | $Y_T$ 值 | $\bar Y_T$ | $\bar Y_C$ | $T(W)$ |
|---|---|---|---|---|
| {1,2,4} (实际观测) | 30,32,34 | 32.00 | 22.33 | **+9.67** |
| {1,2,3} | 30,32,22 | 28.00 | 26.33 | +1.67 |
| {1,4,6} | 30,34,24 | 29.33 | 25.00 | +4.33 |
| {3,5,6} | 22,21,24 | 22.33 | 32.00 | **−9.67** |
| {2,3,5} | 32,22,21 | 25.00 | 29.33 | −4.33 |
| … (共 20 个) | | | | |

**$p$ 值** = (在 20 种分组里 $|T(W)|\ge 9.67$ 的比例)。
因为 6 个数字中没有其他组合能让 3 大 3 小更极端：只有 {1,2,4} 和它的"镜像" {3,5,6} 能给 $\pm 9.67$。所以
$$p=\frac{2}{20}=0.10.$$

**诠释 / Reading**：若锐零成立，看到 $|T|\ge 9.67$ 的概率是 10%。正好卡在显著性边缘，不够拒绝 $H_0$。

> 📌 **如果 $N=30$ 呢？** $\binom{30}{15}\approx 1.55\times 10^8$ 太多，无法枚举 → **Monte Carlo**：随机抽 $B=10{,}000$ 种 $W$ 近似 $p$。

### 一句话总结 CRE

"拿 6 块地掷骰子，分 3 个 T 和 3 个 C；估计 $\hat\tau=\bar Y_T-\bar Y_C$ 无偏但在单次实验可能偏差很大；Fisherian 把观测值固定、枚举 20 种分配算 $p$；Neymanian 保守方差丢掉看不到的 $S_\tau^2$。"

---

## 📝 Exam Problem 1 (English)

A researcher runs a CRE with $N=8$ fish, $N_T=4$ given a new feed, $N_C=4$ given standard feed. She measures growth (cm) after 30 days:

$$Y_T^{obs}=\{6, 8, 7, 9\},\quad Y_C^{obs}=\{5, 4, 6, 5\}.$$

**(a)** Write the size of the randomization set $|W^+|$ and $P(W_i=1)$.
**(b)** Compute $\hat\tau$ and its conservative 95% CI.
**(c)** State the sharp null in words. Using $T_{dm}$, compute $T^{obs}$.
**(d)** The research assistant says "we need Monte Carlo for the $p$‑value because enumeration is infeasible." Is she right? Justify with a number.

---

### ✅ Solution 1(a)

**English.** $|W^+|=\binom{8}{4}=70$. Marginally $P(W_i=1)=N_T/N=4/8=1/2$.

**中文对照。** $|W^+|=\binom{8}{4}=70$ 种分配方案，每种等概率。个体而言 $P(W_i=1)=4/8=1/2$。

---

### ✅ Solution 1(b)

**English.**
$\bar Y_T^{obs}=(6+8+7+9)/4=7.5$, $\bar Y_C^{obs}=(5+4+6+5)/4=5$. $\hat\tau=7.5-5=2.5$.

Sample variances (divide by $N_j-1=3$):
- Treatment deviations from 7.5: $-1.5, 0.5, -0.5, 1.5$; squared sum $=2.25+0.25+0.25+2.25=5$. $s_1^2=5/3\approx 1.667$.
- Control deviations from 5: $0,-1,1,0$; squared sum $=2$. $s_0^2=2/3\approx 0.667$.

$$\widehat{\mathrm{Var}}(\hat\tau)=\frac{s_1^2}{N_T}+\frac{s_0^2}{N_C}=\frac{1.667}{4}+\frac{0.667}{4}=\frac{2.333}{4}\approx 0.583.$$
$SE\approx 0.764$. 95% CI: $2.5\pm 2(0.764)=(0.97,\,4.03)$.

**中文对照。** $\bar Y_T^{obs}=7.5$，$\bar Y_C^{obs}=5$，$\hat\tau=2.5$。样本方差（分母 $N_j-1=3$）：$s_1^2=5/3\approx 1.667$，$s_0^2=2/3\approx 0.667$。保守方差 $=1.667/4+0.667/4\approx 0.583$，$SE\approx 0.764$。95% CI：$2.5\pm 2\times 0.764=(0.97,\,4.03)$。

---

### ✅ Solution 1(c)

**English.** The sharp null is $H_0: Y_i(1)=Y_i(0)$ **for every fish** $i$ — the new feed produces exactly the same growth as the standard feed for each individual fish (not merely on average). Under $H_0$, the 8 observed numbers are fixed; only the labels move. $T^{obs}=\bar Y_T^{obs}-\bar Y_C^{obs}=7.5-5=2.5$.

**中文对照。** 锐零 $H_0: Y_i(1)=Y_i(0)$ 对**每一条鱼**都成立——新饲料对**每条鱼个体**效果都恰好为 0（不只是"平均为 0"）。锐零下 8 个观测数字固定，只有标签随机。$T^{obs}=7.5-5=2.5$。

---

### ✅ Solution 1(d)

**English.** She is **wrong**. The reference set has only $\binom{8}{4}=70$ elements, which is small enough to enumerate by hand or in one line of code. Monte Carlo is only needed when $|W^+|$ is too large to enumerate (e.g. $\binom{30}{15}\approx 1.55\times 10^8$). 70 is trivial.

**中文对照。** 她错了。参照集只有 $\binom{8}{4}=70$ 种，完全可以用一行代码枚举。只有当 $|W^+|$ 大到无法枚举时（例如 $\binom{30}{15}\approx 1.55\times 10^8$）才需要 Monte Carlo。70 微不足道。

---

# Chapter 2 · Matched Pair Design

## 🧪 实验设计

同一位研究员把 **$N=8$ 条鱼配成 $P=4$ 对**（按初始体重配对：最大两条配一对，次大两条配一对，以此类推）。每对内**独立掷硬币**：其中一条新饲料，另一条标准饲料。

配对表：

| 对 $p$ | 初始体重 | 鱼 A (之一) | 鱼 B (之一) |
|---|---|---|---|
| 1 | 重 | 鱼 1 | 鱼 2 |
| 2 | 次重 | 鱼 3 | 鱼 4 |
| 3 | 次轻 | 鱼 5 | 鱼 6 |
| 4 | 轻 | 鱼 7 | 鱼 8 |

## 🚶 手把手走一遍

### 步骤 1 · 分配机制

$|W^+|=2^P=2^4=16$ 种符号模式（每对内两种可能，4 对独立）。$P(W)=1/16$。

### 步骤 2 · 硬币结果

假设掷出：

| 对 $p$ | 鱼 T (新) | 鱼 C (标) | $Y_T^{obs}$ | $Y_C^{obs}$ | $d_p=Y_T-Y_C$ |
|---|---|---|---|---|---|
| 1 | 鱼 1 | 鱼 2 | 11 | 8 | **+3** |
| 2 | 鱼 4 | 鱼 3 | 9 | 8 | **+1** |
| 3 | 鱼 5 | 鱼 6 | 8 | 3 | **+5** |
| 4 | 鱼 8 | 鱼 7 | 6 | 3 | **+3** |

### 步骤 3 · Neymanian

**配对估计量**：$\hat\tau_p=d_p$，直接无偏估计该对的 $\tau_p$。

**总体 ATE 估计量**：
$$\hat\tau=\bar d=\frac{3+1+5+3}{4}=\frac{12}{4}=\boxed{3}.$$

**$s_d^2$**：偏差 $\{0,-2,2,0\}$，平方和 $=0+4+4+0=8$。$s_d^2=8/(P-1)=8/3\approx 2.667$。

**方差估计**（**精确，非保守！**）：
$$\widehat{\mathrm{Var}}(\hat\tau)=\frac{s_d^2}{P}=\frac{8/3}{4}=\frac{2}{3}\approx 0.667.$$
$SE=\sqrt{2/3}\approx 0.816$。**95% CI**：$3\pm 2(0.816)=(1.37,\,4.63)$。

> 💡 **为什么精确？** 我们**直接观测到** $d_p$（不需要估算任何未观测的协方差项），所以 $s_d^2/P$ 本身就是 $\mathrm{Var}(\hat\tau)$ 的无偏估计，不是保守估计。

### 步骤 4 · Fisherian

**锐零**：$Y_{i,p}(1)=Y_{i,p}(0)$ 对每条鱼都成立 → $d_p$ 的**符号**随机（谁在对内被分处理是抛硬币）。

**参照集**：**16 种符号组合**，不是 $\binom{8}{4}=70$！（$\binom{8}{4}$ 是 CRE 的，会允许把第 1 对的鱼跨到第 3 对，这在 matched pair 是**禁止**的。）

**观测** $T^{obs}=\bar d=3$。

我来**真的枚举一部分** 16 种符号组合，看看 $\bar d^*$ 的分布：

| 符号模式 $(\epsilon_1,\epsilon_2,\epsilon_3,\epsilon_4)$ | $d^*=(\epsilon_1\cdot 3,\epsilon_2\cdot 1,\epsilon_3\cdot 5,\epsilon_4\cdot 3)$ | $\bar d^*$ |
|---|---|---|
| $(+,+,+,+)$ (观测) | $(3,1,5,3)$ | **+3.0** |
| $(+,+,+,-)$ | $(3,1,5,-3)$ | +1.5 |
| $(+,+,-,+)$ | $(3,1,-5,3)$ | +0.5 |
| $(+,-,+,+)$ | $(3,-1,5,3)$ | +2.5 |
| $(-,+,+,+)$ | $(-3,1,5,3)$ | +1.5 |
| $(+,+,-,-)$ | $(3,1,-5,-3)$ | −1.0 |
| $(+,-,+,-)$ | $(3,-1,5,-3)$ | +1.0 |
| $(-,+,+,-)$ | $(-3,1,5,-3)$ | 0.0 |
| $(+,-,-,+)$ | $(3,-1,-5,3)$ | 0.0 |
| $(-,+,-,+)$ | $(-3,1,-5,3)$ | −1.0 |
| $(-,-,+,+)$ | $(-3,-1,5,3)$ | +1.0 |
| $(+,-,-,-)$ | $(3,-1,-5,-3)$ | −1.5 |
| $(-,+,-,-)$ | $(-3,1,-5,-3)$ | −2.5 |
| $(-,-,+,-)$ | $(-3,-1,5,-3)$ | −0.5 |
| $(-,-,-,+)$ | $(-3,-1,-5,3)$ | −1.5 |
| $(-,-,-,-)$ | $(-3,-1,-5,-3)$ | **−3.0** |

**$|T^{rand}|\ge 3$ 的组合**：只有 $(+,+,+,+)$ 和 $(-,-,-,-)$，共 2 种。

$$p\text{-value}=\frac{2}{16}=0.125.$$

> 📌 **注意**：这个 $p$ 是双侧的（包括 $T\le-3$ 和 $T\ge+3$）。

### 一句话总结 Matched Pair

"把相似的两条鱼配成一对，对内抛硬币决定谁上新饲料。方差**精确** = $s_d^2/P$；参照集是 $2^P$（对内翻符号），不是 $\binom{N}{N/2}$；配得好 → $d_p$ 变异小 → $\hat\tau$ 更精确。"

---

## 📝 Exam Problem 2 (Matched Pair, English)

A cosmetics company runs a matched‑pair trial with $P=5$ pairs of identical twins. Each pair gets one twin on the new cream (T), one on placebo (C). After 6 weeks, the within‑pair differences in skin‑smoothness score (T − C) are:

$$d=\{2,\ -1,\ 4,\ 3,\ 2\}.$$

**(a)** Write $|W^+|$ and describe the correct re‑randomization procedure in one sentence.
**(b)** Compute $\hat\tau,\ s_d^2,\ \widehat{\mathrm{Var}}(\hat\tau)$, and a 95% CI.
**(c)** Is this variance estimator **exact** or **conservative**? Explain why in 1–2 sentences.
**(d)** Suppose you wanted to run a Fisherian test with $T_{dm}=\bar d$. How many sign patterns are in the reference set, and does sampling error allow exact enumeration?

---

### ✅ Solution 2(a)

**English.** $|W^+|=2^P=2^5=32$. Re‑randomization: within each of the 5 pairs independently, flip a coin to swap which twin is labeled T and which is C — the twins themselves cannot be swapped between pairs.

**中文对照。** $|W^+|=2^5=32$。重随机化：在 5 对的**每对内部**独立翻转 T/C 标签 —— 两个双胞胎不能被换到别对去。

---

### ✅ Solution 2(b)

**English.**
$\bar d=(2-1+4+3+2)/5=10/5=2.$
Deviations from 2: $0,-3,2,1,0$; squared sum $=0+9+4+1+0=14$.
$s_d^2=14/(P-1)=14/4=3.5.$
$\widehat{\mathrm{Var}}(\hat\tau)=s_d^2/P=3.5/5=0.7.$
$SE=\sqrt{0.7}\approx 0.837.$
95% CI: $2\pm 2(0.837)=(0.33,\,3.67)$.

**中文对照。** $\bar d=10/5=2$。偏差平方和 $=14$，$s_d^2=14/4=3.5$。方差 $=3.5/5=0.7$，$SE\approx 0.837$。95% CI：$2\pm 2\times 0.837=(0.33,\,3.67)$。

---

### ✅ Solution 2(c)

**English.** This estimator is **exact** (not conservative). The reason: $d_p$ is directly observable for every pair, so $s_d^2$ is a legitimate sample variance of the $d_p$'s with nothing unobservable dropped. Contrast with CRE, where the variance formula contains $S_\tau^2$ which we can never observe, so we drop it and get a conservative upper bound.

**中文对照。** 此估计是**精确的**（非保守）。原因：$d_p$ 每对都直接观测到，$s_d^2$ 就是这些 $d_p$ 的样本方差，没有丢掉任何看不到的量。对比 CRE 中方差公式含 $S_\tau^2$（永远看不到），只能丢掉 → 保守上界。

---

### ✅ Solution 2(d)

**English.** The reference set has $2^5=32$ sign patterns. Yes, 32 is tiny; exact enumeration is trivially feasible (a short loop over the 32 subsets of $\{1,2,3,4,5\}$, each determining which $d_p$'s get their sign flipped).

**中文对照。** 参照集大小 $2^5=32$。32 非常小，用一段短代码枚举 $\{1,2,3,4,5\}$ 的 32 个子集（每个子集决定翻哪几个 $d_p$ 的符号）即可得到精确 $p$ 值，不需要 Monte Carlo。

---

# Chapter 3 · Multi‑Treatment CRE

## 🧪 实验设计

同一位研究员这回测 **$J=3$ 种饲料**：标准 ($j=1$)、低剂量新 ($j=2$)、高剂量新 ($j=3$)。$N=9$ 条鱼，$N_1=N_2=N_3=3$。

## 🚶 手把手走一遍

### 步骤 1 · 分配机制

$$|W^+|=\frac{N!}{\prod_j N_j!}=\frac{9!}{3!\,3!\,3!}=\frac{362880}{6\cdot 6\cdot 6}=1680.$$
每种等概率。

### 步骤 2 · 观测数据

假设随机抽到一次分组后观测到：

| arm | 观测值 | $\bar Y^{obs}_j$ |
|---|---|---|
| 1 (std) | $\{5, 7, 6\}$ | 6 |
| 2 (low) | $\{8, 9, 7\}$ | 8 |
| 3 (high) | $\{12, 11, 10\}$ | 11 |

总均值 $\bar Y^{obs}=(6+8+11)/3=25/3\approx 8.333$（因为 $N_j$ 相等，总均值 = 组均值的均值）。

### 步骤 3 · Neymanian 两两比较

$$\hat\tau(3,1)=\bar Y_3-\bar Y_1=11-6=5,\quad \hat\tau(2,1)=8-6=2,\quad \hat\tau(3,2)=11-8=3.$$

组内样本方差（分母 $N_j-1=2$）：
- arm 1: 偏差 $\{-1,1,0\}$，平方和 $=2$，$s_1^2=1$。
- arm 2: 偏差 $\{0,1,-1\}$，平方和 $=2$，$s_2^2=1$。
- arm 3: 偏差 $\{1,0,-1\}$，平方和 $=2$，$s_3^2=1$。

**例 1**：$\hat\tau(3,1)$ 的保守方差 $=s_3^2/N_3+s_1^2/N_1=1/3+1/3=2/3\approx 0.667$，$SE\approx 0.816$。95% CI：$5\pm 2(0.816)=(3.37,\,6.63)$。

### 步骤 4 · Fisherian ANOVA $F$ 检验

**锐零** $H_0: Y_i(1)=Y_i(2)=Y_i(3)\ \forall i$。9 个观测数字固定。

**$SS_{\text{Treat}}$**：
$$\sum_j N_j(\bar Y_j-\bar Y)^2 = 3(6-25/3)^2 + 3(8-25/3)^2 + 3(11-25/3)^2.$$
逐项：$6-25/3=-7/3$，平方 $=49/9$。$8-25/3=-1/3$，平方 $=1/9$。$11-25/3=8/3$，平方 $=64/9$。
$$SS_{\text{Treat}}=3\cdot\frac{49+1+64}{9}=3\cdot\frac{114}{9}=\frac{342}{9}=38.$$

**$SS_{\text{Res}}$**：每组组内偏差平方和 $=2$，三组合计 $=6$。

**ANOVA 表**：
| Source | df | SS | MS |
|---|---|---|---|
| Treatments | $J-1=2$ | 38 | 19 |
| Residuals | $N-J=6$ | 6 | 1 |
| Total | 8 | 44 | |

$F=MS_{\text{Treat}}/MS_{\text{Res}}=19/1=\boxed{19}$。

> 💡 **直觉**：$F\approx 1$ 代表"组间变异 ≈ 组内噪音 → 锐零下正常的景象"。$F=19\gg 1$ → 强烈证据反对锐零。

**$p$ 值**：枚举所有 1680 种分组，重算 $F^{rand}$，统计 $F^{rand}\ge 19$ 的比例。实际实现：1680 可以枚举；更一般地 Monte Carlo。

### 步骤 5 · 多重检验警告

$J=3$ arms → $\binom{3}{2}=3$ 对成比较。若每个对比都用 $\alpha=0.05$，族错误率 (family‑wise error rate) 可能膨胀到 $1-(1-0.05)^3\approx 0.14$。**先用 $F$ 检验"打门槛"，拒绝后再做两两比较**，更稳妥。

### 一句话总结 Multi‑Treatment CRE

"$J\ge 2$ 种处理，$|W^+|=\tfrac{N!}{\prod N_j!}$。Fisherian 用 ANOVA $F$ 检验；锐零下 $F\approx 1$，大 $F$ 反对 $H_0$。两两 ATE 用 Neymanian 保守方差，但小心多重检验。"

---

## 📝 Exam Problem 3 (Multi‑Treatment CRE, English)

A clinic tests $J=3$ fitness programs on $N=6$ volunteers, $N_1=N_2=N_3=2$. Observed weight‑loss (lbs):

$$\text{A}:\{3,5\},\quad \text{B}:\{8,6\},\quad \text{C}:\{10,12\}.$$

**(a)** State $|W^+|$.
**(b)** Build the full ANOVA table and compute $F$.
**(c)** Compute $\hat\tau(C,A)$ and a conservative 95% CI.
**(d)** Why is the conservative variance formula, not the exact one, used in (c)?

---

### ✅ Solution 3(a)

**English.** $|W^+|=\dfrac{6!}{2!\,2!\,2!}=\dfrac{720}{8}=90$.

**中文对照。** $|W^+|=\dfrac{6!}{2!\,2!\,2!}=90$ 种分组。

---

### ✅ Solution 3(b)

**English.**
Arm means: $\bar Y_A=4,\ \bar Y_B=7,\ \bar Y_C=11$. Overall $\bar Y=(4+7+11)/3=22/3\approx 7.333$.

$SS_{\text{Treat}}=2(4-22/3)^2+2(7-22/3)^2+2(11-22/3)^2$.
- $4-22/3=-10/3 \Rightarrow (-10/3)^2=100/9$
- $7-22/3=-1/3\Rightarrow 1/9$
- $11-22/3=11/3\Rightarrow 121/9$
$SS_{\text{Treat}}=2\cdot(100+1+121)/9=2\cdot 222/9=444/9\approx 49.33.$

$SS_{\text{Res}}$: within‑arm sums:
- A: $(3-4)^2+(5-4)^2=1+1=2$
- B: $(8-7)^2+(6-7)^2=1+1=2$
- C: $(10-11)^2+(12-11)^2=1+1=2$
Total $=6$.

| Source | df | SS | MS |
|---|---|---|---|
| Treatments | 2 | $444/9\approx 49.33$ | $\approx 24.67$ |
| Residuals | 3 | 6 | 2 |
| Total | 5 | $\approx 55.33$ | |

$F=24.67/2\approx 12.33$.

**中文对照。** 组均值 $\bar Y_A=4,\bar Y_B=7,\bar Y_C=11$；总均值 $22/3$。
$SS_{\text{Treat}}=2\cdot(100+1+121)/9=444/9\approx 49.33$。组内平方和每组都是 2，合计 $SS_{\text{Res}}=6$。
$MS_{\text{Treat}}\approx 24.67$，$MS_{\text{Res}}=2$，$F\approx 12.33$。

---

### ✅ Solution 3(c)

**English.**
$\hat\tau(C,A)=\bar Y_C-\bar Y_A=11-4=7.$
$s_A^2=2/(N_A-1)=2/1=2,\ s_C^2=2/1=2.$
$\widehat{\mathrm{Var}}(\hat\tau(C,A))=s_C^2/N_C+s_A^2/N_A=2/2+2/2=2.$
$SE=\sqrt 2\approx 1.414$. 95% CI: $7\pm 2(1.414)=(4.17,\,9.83)$.

**中文对照。** $\hat\tau(C,A)=11-4=7$。$s_A^2=s_C^2=2$。保守方差 $=2/2+2/2=2$，$SE\approx 1.414$。95% CI：$7\pm 2\times 1.414=(4.17,\,9.83)$。

---

### ✅ Solution 3(d)

**English.** The exact variance contains the term $S_{jj'}^2/N$, the finite‑population variance of the unit‑level effects $\tau_i(j,j')=Y_i(j)-Y_i(j')$. This requires knowing **both** potential outcomes $Y_i(C)$ and $Y_i(A)$ for the same volunteer, which is impossible — each volunteer only received one program. So we drop this term (it's $\ge 0$, so dropping it inflates the estimated variance), yielding a **conservative** (upper‑bound) estimator.

**中文对照。** 精确方差含 $S_{jj'}^2/N$ 项，它是单元级效应 $\tau_i(j,j')=Y_i(j)-Y_i(j')$ 的有限总体方差，需要**同一个**志愿者在两个 program 下的潜在结果——但每人只接受了一个 program，这是**无法观测**的。因此丢掉这项（$\ge 0$，丢掉会抬高方差估计），得到**保守**上界估计。

---

# Chapter 4 · Crossover Design

## 🧪 实验设计

**$N=5$ 名偏头痛患者**，每人被观察**两个时期**。每人独立掷硬币决定第 1 期给新药还是安慰剂；第 2 期反过来。

**关键假设**：无延滞效应 + 无时段效应（第 1 期的药不影响第 2 期；两期除顺序外对称）。

## 🚶 手把手走一遍

### 步骤 1 · 抛硬币结果 + 观测

| 患者 $i$ | 第 1 期 | 第 2 期 | 观测值 P1 | 观测值 P2 | $d_i=Y_T-Y_C$ |
|---|---|---|---|---|---|
| 1 | T | C | 6 | 3 | 3 |
| 2 | C | T | 4 | 5 | 1 |
| 3 | T | C | 7 | 2 | 5 |
| 4 | C | T | 5 | 4 | −1 |
| 5 | T | C | 8 | 3 | 5 |

> 📌 **$d_i$ 的定义**：不管哪一期是 T，$d_i$ 永远 = (T 期观测值) − (C 期观测值)。

### 步骤 2 · Neymanian 估计

$$\bar d=(3+1+5-1+5)/5=13/5=2.6.$$
这就是 $\hat\tau$——**总体平均因果效应**的估计。

$s_D^2$：偏差 $\{0.4,-1.6,2.4,-3.6,2.4\}$，平方 $\{0.16,2.56,5.76,12.96,5.76\}$，和 $=27.2$。
$s_D^2=27.2/(N-1)=27.2/4=6.8$。

标准误 $SE(\bar d)=s_D/\sqrt N=\sqrt{6.8/5}=\sqrt{1.36}\approx 1.166$。95% CI：$2.6\pm 2(1.166)=(0.27,\,4.93)$。

### 步骤 3 · Fisherian

**锐零** $Y_{i,j}(1)=Y_{i,j}(0)\ \forall i,j$ → 每个 $d_i$ 的符号纯随机。

**检验统计量**：
$$T_{paired}=\frac{\bar d}{s_D/\sqrt N}=\frac{2.6}{1.166}\approx 2.23.$$

**参照集**：$2^N=2^5=32$ 个符号模式。

让我枚举几个：
- 原始 $(+,+,+,+,+)$ on $|d|=(3,1,5,1,5)$：$\bar d=2.6$。
- $(-,-,-,-,-)$：$\bar d=-2.6$。
- $(+,+,+,-,+)$：$d^*=(3,1,5,1,5)$，$\bar d^*=3.0$。
- $(-,+,+,+,+)$：$d^*=(-3,1,5,-1,5)$，$\bar d^*=1.4$。

实际要**对每一种 $\boldsymbol\epsilon\in\{-1,+1\}^5$** 重算 $T^*=\bar d^*/(s_D^*/\sqrt N)$。

**双侧 $p$ 值** $=\#\{|T^*|\ge 2.23\}/32$。32 小，**可完全枚举**。

### 步骤 4 · Crossover 的独特优势

- **单元级效应可估**：$\hat\tau_i=d_i$ 对单个患者都无偏。这在所有"人与人之间比较"的设计里都做不到（CRE/Matched Pair 只能估平均效应）。
- **消除了人与人之间的差异**：每个患者自己做自己的对照。

### 步骤 5 · 警惕前提

如果新药在第 1 期造成肠胃反应持续到第 2 期（延滞），或患者经过第 1 期心理上更放松（时段效应），那 $d_i$ 就**不再是**单纯的处理效应，估计被污染。

### 一句话总结 Crossover

"每人两期，独立抛硬币决定哪期处理；$d_i$ 直接估计个人效应；$|W^+|=2^N$ → 符号翻转枚举 $p$。**前提是无延滞+无时段效应**。"

---

## 📝 Exam Problem 4 (Crossover, English)

In a crossover trial of a new sleep aid with $N=6$ participants, within‑person differences (drug night − placebo night, in hours of sleep) are:

$$d=\{1.2,\ 0.8,\ -0.3,\ 1.5,\ 0.6,\ 1.0\}.$$

**(a)** Write the assignment space $|W^+|$ and the two assumptions required for this design to be valid.
**(b)** Compute $\hat\tau,\ s_D^2$, and $T_{paired}$.
**(c)** State the sharp null in this context. Explain why under the sharp null the sign of each $d_i$ is uniformly $\pm 1$.
**(d)** Explain in one sentence why the crossover design can estimate the unit‑level effect $\tau_i$ (something impossible in a CRE).

---

### ✅ Solution 4(a)

**English.** $|W^+|=2^N=2^6=64$. Required assumptions: (1) **no carryover effect** — the drug's effect does not persist into the placebo night; (2) **no time/period effect** — the two nights are otherwise exchangeable (e.g. no learning, no adaptation, no systematic weekday/weekend difference).

**中文对照。** $|W^+|=2^6=64$。两大假设：(1) **无延滞 (no carryover)**——药效不会持续到安慰剂夜；(2) **无时段 (no time/period)**——两晚除顺序外可互换（无习得、无适应、无工作日/周末差异）。

---

### ✅ Solution 4(b)

**English.**
$\bar d=(1.2+0.8-0.3+1.5+0.6+1.0)/6=4.8/6=0.8$.
Deviations from 0.8: $0.4, 0.0, -1.1, 0.7, -0.2, 0.2$.
Squared: $0.16, 0.0, 1.21, 0.49, 0.04, 0.04$; sum $=1.94$.
$s_D^2=1.94/5=0.388$. $s_D\approx 0.623$. $s_D/\sqrt 6\approx 0.254$.
$T_{paired}=0.8/0.254\approx 3.15$.

**中文对照。** $\bar d=4.8/6=0.8$。偏差平方和 $=1.94$，$s_D^2=0.388$，$s_D\approx 0.623$，$s_D/\sqrt 6\approx 0.254$。$T_{paired}\approx 0.8/0.254\approx 3.15$。

---

### ✅ Solution 4(c)

**English.** Sharp null: $Y_{i,j}(1)=Y_{i,j}(0)$ for every participant $i$ and every night $j\in\{1,2\}$ — the drug produces exactly the same hours of sleep as the placebo for every individual on every night. Under $H_0$, whichever night was labeled "drug" vs "placebo" is irrelevant: the observed pair of numbers is fixed, and $d_i=(\text{night labeled T})-(\text{night labeled C})$ is equally likely to be $+|d_i|$ or $-|d_i|$ depending on the coin flip for which order participant $i$ got — i.e. the sign of $d_i$ is a fair coin flip.

**中文对照。** 锐零 $Y_{i,j}(1)=Y_{i,j}(0)$ 对每人、每晚都成立——新药对每人每晚的睡眠时长效果都恰好为 0。锐零下，哪晚标 "T"、哪晚标 "C" 完全无关紧要：观测的两个数是固定的，$d_i$ 的符号取决于这个人被分到哪种顺序（抛硬币 50/50）→ $d_i$ 以等概率 $\pm|d_i|$。

---

### ✅ Solution 4(d)

**English.** Because under no‑carryover/no‑time, each participant is observed under **both** conditions (T in one night, C in the other), so $d_i$ directly gives the within‑person contrast $Y_{i,T}-Y_{i,C}=\tau_i$. In a CRE each unit is observed under only one condition, so $\tau_i$ is always unobservable.

**中文对照。** 因为无延滞+无时段假设下，每个参与者在**两种条件**都被观测了（一晚 T 一晚 C），所以 $d_i$ 就直接给出了个人内部对比 $Y_{i,T}-Y_{i,C}=\tau_i$。CRE 中每人只在一种条件下被观测，$\tau_i$ 永远无法估计。

---

# Chapter 5 · Block Design

## 🧪 实验设计

一家临床试验测一款血压药，**10 名受试者**。研究员按年龄分成两组：**年轻组 $k=y$** $N(y)=6$，**年长组 $k=o$** $N(o)=4$。每组内做独立 CRE：
- 年轻组：3 人 T，3 人 C
- 年长组：2 人 T，2 人 C

## 🚶 手把手走一遍

### 步骤 1 · 分配机制

$$|W^+|=\binom{6}{3}\binom{4}{2}=20\cdot 6=120.$$
设计分解为**两个独立的小 CRE**。

### 步骤 2 · 观测数据（假设）

| 区组 $k$ | arm | 值 | 样本均值 |
|---|---|---|---|
| $y$ (年轻) | T | $\{14, 12, 16\}$ | $\bar Y_T(y)=14$ |
| $y$ | C | $\{10, 8, 12\}$ | $\bar Y_C(y)=10$ |
| $o$ (年长) | T | $\{22, 18\}$ | $\bar Y_T(o)=20$ |
| $o$ | C | $\{18, 16\}$ | $\bar Y_C(o)=17$ |

### 步骤 3 · 块内效应

$\hat\tau(y)=14-10=4,\quad \hat\tau(o)=20-17=3.$

### 步骤 4 · 总 ATE (**人口加权**，非等权！)

$$\lambda_y=\frac{N(y)}{N}=\frac{6}{10}=0.6,\quad \lambda_o=\frac{4}{10}=0.4.$$
$$\hat\tau=\lambda_y\hat\tau(y)+\lambda_o\hat\tau(o)=0.6(4)+0.4(3)=2.4+1.2=\boxed{3.6}.$$

> 💡 **为什么 $N(k)/N$ 而非 $1/B$**：问题"平均每人的效应是多少？"权重就是人口份额。"每个块的平均效应"才用等权。

### 步骤 5 · Neymanian 方差

块内样本方差（分母 $N_w(k)-1$）：
- 年轻 T: 偏差 $\{0,-2,2\}$，平方和 $=8$，$s_T^2(y)=8/2=4$。
- 年轻 C: 偏差 $\{0,-2,2\}$，平方和 $=8$，$s_C^2(y)=4$。
- 年长 T: 偏差 $\{2,-2\}$，平方和 $=8$，$s_T^2(o)=8/1=8$。
- 年长 C: 偏差 $\{1,-1\}$，平方和 $=2$，$s_C^2(o)=2$。

保守方差：
$$\widehat{\mathrm{Var}}(\hat\tau)=\Big(\tfrac{6}{10}\Big)^2\Big(\tfrac{s_T^2(y)}{N_t(y)}+\tfrac{s_C^2(y)}{N_c(y)}\Big)+\Big(\tfrac{4}{10}\Big)^2\Big(\tfrac{s_T^2(o)}{N_t(o)}+\tfrac{s_C^2(o)}{N_c(o)}\Big).$$
- 年轻：$(0.6)^2(4/3+4/3)=0.36\cdot 8/3=2.88/3=0.96$
- 年长：$(0.4)^2(8/2+2/2)=0.16\cdot 5=0.80$

合计 $=0.96+0.80=1.76$。$SE=\sqrt{1.76}\approx 1.327$。**95% CI**：$3.6\pm 2(1.327)=(0.95,\,6.25)$。

### 步骤 6 · Fisherian

**检验统计量** $T_\lambda = \lambda_y\cdot 4 + \lambda_o\cdot 3 = 3.6$。

**参照集**：**只在块内重随机化**——120 种。决不能把年轻组的人换到年长组。

错误做法（不得分）：使用 $\binom{10}{5}=252$，这允许跨组互换，违反设计约束。

### 步骤 7 · 效应修饰 (Effect modification)

年轻组效应 $4$ vs 年长组 $3$，差 $1$。估计这个差值：$\hat\tau(y)-\hat\tau(o)=1$。

两块独立 → 方差直接相加：
$$\widehat{\mathrm{Var}}(\hat\tau(y)-\hat\tau(o))=\tfrac{s_T^2(y)}{N_t(y)}+\tfrac{s_C^2(y)}{N_c(y)}+\tfrac{s_T^2(o)}{N_t(o)}+\tfrac{s_C^2(o)}{N_c(o)}=4/3+4/3+8/2+2/2=8/3+5\approx 7.667.$$
$SE\approx 2.77$。差值 CI：$1\pm 5.54=(-4.54, 6.54)$——**含 0**，所以没有证据显示效应在年轻/年长间不同。

### 一句话总结 Block Design

"块内 CRE；$\hat\tau=\sum_k\frac{N(k)}{N}\hat\tau(k)$；**只在块内重随机**，参照集 $\prod_k\binom{N(k)}{N_t(k)}$。方差小了是因为块间变异被清除。"

---

## 📝 Exam Problem 5 (Block, English)

A gym tests a new supplement with $B=2$ blocks (gender): **female** $N(f)=8$, $N_t(f)=4$; **male** $N(m)=12$, $N_t(m)=6$. Observed:

- female: $\bar Y_T(f)=30,\ \bar Y_C(f)=25,\ s_T^2(f)=8,\ s_C^2(f)=8$
- male: $\bar Y_T(m)=40,\ \bar Y_C(m)=32,\ s_T^2(m)=12,\ s_C^2(m)=12$

**(a)** Compute $|W^+|$.
**(b)** Compute $\hat\tau$ (with population weights) and a conservative 95% CI.
**(c)** A student claims the reference set for a Fisherian test is $\binom{20}{10}=184756$. Is she right? Briefly explain and give the correct number.
**(d)** Test for effect modification: give $\hat\tau(m)-\hat\tau(f)$, its conservative variance, a 95% CI, and state whether the CI excludes 0.

---

### ✅ Solution 5(a)

**English.** $|W^+|=\binom{N(f)}{N_t(f)}\cdot\binom{N(m)}{N_t(m)}=\binom{8}{4}\binom{12}{6}=70\cdot 924=64{,}680$.

**中文对照。** $|W^+|=\binom{8}{4}\binom{12}{6}=70\times 924=64{,}680$。

---

### ✅ Solution 5(b)

**English.**
Block effects: $\hat\tau(f)=30-25=5,\ \hat\tau(m)=40-32=8$. Weights: $\lambda_f=8/20=0.4,\ \lambda_m=12/20=0.6$.
$\hat\tau=0.4(5)+0.6(8)=2+4.8=6.8$.

Variance contributions:
- female: $(0.4)^2(8/4+8/4)=0.16\cdot 4=0.64$.
- male: $(0.6)^2(12/6+12/6)=0.36\cdot 4=1.44$.
Total $\widehat{\mathrm{Var}}(\hat\tau)=0.64+1.44=2.08$. $SE\approx 1.442$. 95% CI: $6.8\pm 2(1.442)=(3.92,\,9.68)$.

**中文对照。** 块内效应 $\hat\tau(f)=5,\ \hat\tau(m)=8$；权重 $\lambda_f=0.4,\lambda_m=0.6$。
$\hat\tau=0.4\cdot 5+0.6\cdot 8=6.8$。方差贡献：女性 $(0.4)^2\cdot 4=0.64$；男性 $(0.6)^2\cdot 4=1.44$。合计 $2.08$，$SE\approx 1.442$。95% CI：$6.8\pm 2\times 1.442=(3.92,\,9.68)$。

---

### ✅ Solution 5(c)

**English.** She is **wrong**. $\binom{20}{10}$ would be the reference set for an unblocked CRE, which allows swapping a female into the male‑treatment group — violating the design. The correct reference set re‑randomizes **only within each block**: $\binom{8}{4}\binom{12}{6}=70\cdot 924=64{,}680$. This is much smaller than 184756 precisely because blocking constraints remove many label swaps.

**中文对照。** 她错了。$\binom{20}{10}$ 是未分块 CRE 的参照集，会把一名女性分到男性处理组——违反设计。正确参照集**只在块内重随机**：$\binom{8}{4}\binom{12}{6}=64{,}680$。比 184756 小得多，正是因为分块约束排除了很多不合法的标签互换。

---

### ✅ Solution 5(d)

**English.**
$\hat\tau(m)-\hat\tau(f)=8-5=3$.
Blocks independent → variances add:
$\widehat{\mathrm{Var}}=\frac{s_T^2(m)}{N_t(m)}+\frac{s_C^2(m)}{N(m)-N_t(m)}+\frac{s_T^2(f)}{N_t(f)}+\frac{s_C^2(f)}{N(f)-N_t(f)}=\frac{12}{6}+\frac{12}{6}+\frac{8}{4}+\frac{8}{4}=2+2+2+2=8.$
$SE=\sqrt 8\approx 2.828$. 95% CI: $3\pm 2(2.828)=(-2.66,\,8.66)$. The CI **includes 0**, so we cannot conclude effect modification is present at the 95% level.

**中文对照。** $\hat\tau(m)-\hat\tau(f)=3$。两块独立，方差相加：$12/6+12/6+8/4+8/4=8$。$SE\approx 2.828$。差值 95% CI：$3\pm 5.66=(-2.66,\,8.66)$，**包含 0**，95% 水平下不能断言效应修饰存在。

---

# Chapter 6 · Factorial Design ($2^K$)

## 🧪 实验设计

$K=3$ 因子: 烤箱温度 $F_1$ (低 $-1$ / 高 $+1$)、烘烤时间 $F_2$ (短/长)、面粉种类 $F_3$ (白/全麦)。共 $2^3=8$ 处理组合，每个组合做 1 次。测量"松软度评分"。

## 🚶 手把手走一遍

### 步骤 1 · 字典序表

$F_1$ 变最慢，$F_3$ 变最快：

| Run | $F_1$ | $F_2$ | $F_3$ | 观测 $\bar Y$ |
|---|---|---|---|---|
| 1 | − | − | − | 4 |
| 2 | − | − | + | 6 |
| 3 | − | + | − | 5 |
| 4 | − | + | + | 9 |
| 5 | + | − | − | 5 |
| 6 | + | − | + | 7 |
| 7 | + | + | − | 8 |
| 8 | + | + | + | 14 |

（这组数字故意取自 Section 10 Exercise 2，后面算出的效应可与那题对照）

### 步骤 2 · 对比向量

从表直接抄列：
$$g_{F_1}=(-,-,-,-,+,+,+,+),\ g_{F_2}=(-,-,+,+,-,-,+,+),\ g_{F_3}=(-,+,-,+,-,+,-,+).$$

**交互项**用 Hadamard 乘积（**同号得 +，异号得 −**）：
$$g_{F_1F_2}=(+,+,-,-,-,-,+,+)$$
$$g_{F_1F_3}=(+,-,+,-,-,+,-,+)$$
$$g_{F_2F_3}=(+,-,-,+,+,-,-,+)$$
$$g_{F_1F_2F_3}=(-,+,+,-,+,-,-,+)$$

**检查**：每个非恒一向量都是 4 个 + 4 个 −，且两两正交（内积 0）✓。

### 步骤 3 · 因子效应

公式：$\hat\tau_F=\dfrac{1}{2^{K-1}}\bar Y^{obs\top}g_F=\dfrac{1}{4}\bar Y^{obs\top}g_F$。

**主效应 $\hat\tau_{F_1}$**：
$$\bar Y^{obs\top}g_{F_1}=-4-6-5-9+5+7+8+14=10.$$
$$\hat\tau_{F_1}=10/4=\boxed{2.5}.$$
**意思**：温度从低切到高，松软度平均升 2.5。

类似：
- $\hat\tau_{F_2}=(-4-6+5+9-5-7+8+14)/4=14/4=\boxed{3.5}$
- $\hat\tau_{F_3}=(-4+6-5+9-5+7-8+14)/4=14/4=\boxed{3.5}$
- $\hat\tau_{F_1F_2}=(+4+6-5-9-5-7+8+14)/4=6/4=1.5$
- $\hat\tau_{F_1F_3}=(+4-6+5-9-5+7-8+14)/4=2/4=0.5$
- $\hat\tau_{F_2F_3}=(+4-6-5+9+5-7-8+14)/4=6/4=1.5$
- $\hat\tau_{F_1F_2F_3}=(-4+6+5-9+5-7-8+14)/4=2/4=0.5$

### 步骤 4 · Neymanian 方差

Section 10 Exercise 2 给 $s_w^2=2$（每个 cell 内部样本方差），$\hat S^2=\frac{1}{2^K}\sum s_w^2=2$。

每个 $\hat\tau_F$ 的 Neyman 保守方差：
$$\widehat{\mathrm{Var}}(\hat\tau_F)=\frac{2^K}{N}\hat S^2=\frac{8}{16}\cdot 2=1.$$
$SE=1$，因此 **95% CI** 为 $\hat\tau_F\pm 2$。例如 $\tau_{F_3}$ 的 95% CI：$3.5\pm 2=(1.5,\,5.5)$。

### 步骤 5 · Fisherian

**锐零**：所有因子效应为 0 → 每单元 8 个潜在结果相等。

**检验统计量**：直接用 $\hat\tau_F$ 本身作 $T$。

**零分布**：保持 16 个观测数字不变，反复把单元重随机分到 8 个 cell（每 cell $n=2$），每次重算 $T^{rand}$。空间 = $16!/(2!)^8 = 2{,}5\times 10^9$，太大 → **Monte Carlo** $B=10{,}000$。

**双侧 $p$ 值** $=\#\{|T^{rand}|\ge |T^{obs}|\}/B$。

### 一句话总结 Factorial

"字典序表 + Hadamard 乘积得 $g_F$；$\hat\tau_F=\frac{1}{2^{K-1}}\bar Y^\top g_F$；Neyman 方差 $\widehat{\mathrm{Var}}=\frac{2^K}{N}\hat S^2$；Fisherian 用 $\hat\tau_F$ 作 $T$，Monte Carlo 重分。"

---

## 📝 Exam Problem 6 (Factorial, English)

A coffee roaster studies two binary factors: **roast level** $F_1$ (light $-1$ / dark $+1$), **grind size** $F_2$ (fine $-1$ / coarse $+1$), across 4 cell‑mean tasting scores (lex order):

$$\bar Y^{obs}=(7,\ 12,\ 6,\ 13).$$

With $N=16$ total cups, $n=4$ per cell, the pooled within‑cell variance is $\hat S^2=4$.

**(a)** Write the lex‑order design matrix and list $g_{F_1},\ g_{F_2},\ g_{F_1F_2}$.
**(b)** Compute $\hat\tau_{F_1},\ \hat\tau_{F_2},\ \hat\tau_{F_1F_2}$.
**(c)** Compute the 95% Neyman CI for $\tau_{F_1}$ (use $\pm 2\,SE$).
**(d)** Describe in 2–3 sentences how you would obtain a Fisherian $p$‑value for the null $H_0:\tau_{F_1}=0$.

---

### ✅ Solution 6(a)

**English.**
| Run | $F_1$ | $F_2$ |
|---|---|---|
| 1 | − | − |
| 2 | − | + |
| 3 | + | − |
| 4 | + | + |

$g_{F_1}=(-,-,+,+),\ g_{F_2}=(-,+,-,+),\ g_{F_1F_2}=g_{F_1}\odot g_{F_2}=(+,-,-,+)$.

**中文对照。** 字典序 4 行如上。$g_{F_1}=(-,-,+,+)$，$g_{F_2}=(-,+,-,+)$，$g_{F_1F_2}=(+,-,-,+)$。

---

### ✅ Solution 6(b)

**English.** Divisor $=2^{K-1}=2$.
- $\bar Y^\top g_{F_1}=-7-12+6+13=0$. $\hat\tau_{F_1}=0/2=0$.
- $\bar Y^\top g_{F_2}=-7+12-6+13=12$. $\hat\tau_{F_2}=12/2=6$.
- $\bar Y^\top g_{F_1F_2}=+7-12-6+13=2$. $\hat\tau_{F_1F_2}=2/2=1$.

**中文对照。** 除数 $=2$。$\hat\tau_{F_1}=0$（烘焙深浅对口感平均无影响！），$\hat\tau_{F_2}=6$（粗细粉影响大），$\hat\tau_{F_1F_2}=1$（小交互）。

---

### ✅ Solution 6(c)

**English.**
$$\widehat{\mathrm{Var}}(\hat\tau_F)=\frac{2^K}{N}\hat S^2=\frac{4}{16}\cdot 4=1.$$
$SE=\sqrt 1=1$. 95% CI for $\tau_{F_1}$: $0\pm 2(1)=(-2,\,2)$.

**中文对照。** $\widehat{\mathrm{Var}}(\hat\tau_F)=\frac{2^K}{N}\hat S^2=\frac{4}{16}\cdot 4=1$，$SE=1$。$\tau_{F_1}$ 的 95% CI：$0\pm 2=(-2,2)$——**包含 0**，与 $\hat\tau_{F_1}=0$ 的观测一致。

---

### ✅ Solution 6(d)

**English.** Under the sharp null every unit's 4 potential outcomes are identical, so the 16 observed numbers are fixed; only cell labels are random. Using $T=\hat\tau_{F_1}$ as the test statistic, repeatedly re‑randomize the 16 units into 4 cells (4 each), recompute $\bar Y^{rand}$ and hence $T^{rand}=\frac{1}{2}\bar Y^{rand\top}g_{F_1}$; the two‑sided $p$‑value is $\#\{|T^{rand}|\ge |T^{obs}|=0\}/B$ over $B$ Monte Carlo draws (the full space $16!/(4!)^4$ is too large to enumerate).

**中文对照。** 锐零下每单元 4 个潜在结果相等，16 个观测数字固定，只有 cell 标签随机。以 $T=\hat\tau_{F_1}$ 为检验统计量，反复把 16 单元重分到 4 个 cell（每 cell 4 个），每次重算 $T^{rand}=\frac{1}{2}\bar Y^{rand\top}g_{F_1}$；双侧 $p$ 值 $=\#\{|T^{rand}|\ge|T^{obs}|=0\}/B$。完整空间 $16!/(4!)^4$ 太大，用 Monte Carlo $B=10{,}000$ 次近似。

---

# 一张总结卡 / Cheat Card

| 设计 Design | $|W^+|$ | 方差性质 | 独特之处 |
|---|---|---|---|
| CRE | $\binom{N}{N_T}$ | 保守 (丢 $S_\tau^2$) | 随机化，但可能不平衡 |
| Matched Pair | $2^P$ | **精确** $s_d^2/P$ | 对内翻符号；消除对间差异 |
| Multi‑Treat | $N!/\prod N_j!$ | 保守 | ANOVA $F$；小心多重检验 |
| Crossover | $2^N$ | 基于 $s_D^2$ | **单元级效应可估**；无延滞+无时段 |
| Block | $\prod_k\binom{N(k)}{N_t(k)}$ | 保守 | 只块内重随；人口加权 |
| Factorial $2^K$ | 看单元数 | $\frac{2^K}{N}\hat S^2$ | 对比向量、乘积法则 |

**考前默念**：
- Fisherian = 锐零 + 观测固定 + 参照集 + $p$ 值
- Neymanian = 估计 + CI（保守 or 精确看设计）
- 参照集是**设计决定的**（CRE 的大组合数 / 对内翻符号 / 块内重随）
- 先找设计，再选统计量，再算数字 🎯

祝考试顺利！
