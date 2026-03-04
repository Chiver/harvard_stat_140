# STAT 140 公式大全与详解

> **Coverage**: Module 1-6 | Potential Outcomes, Assignment Mechanisms, Fisher, Neyman, Bayesian
> **Purpose**: 所有公式的系统梳理，含直觉解释与适用条件

---

## 1. 潜在结果框架 (Potential Outcomes Framework)

### 1.1 观察结果切换方程 (Switching Equation)

$$Y_i^{\text{obs}} = W_i \cdot Y_i(1) + (1 - W_i) \cdot Y_i(0)$$

| 符号 | 含义 |
|------|------|
| $Y_i^{\text{obs}}$ | 单位 $i$ 实际观察到的结果 |
| $W_i$ | 处理指示器：$W_i = 1$ 表示接受处理，$W_i = 0$ 表示对照 |
| $Y_i(1)$ | 单位 $i$ 在处理下的潜在结果（固定常数） |
| $Y_i(0)$ | 单位 $i$ 在对照下的潜在结果（固定常数） |

**直觉**：$W_i$ 像一个开关。$W_i=1$ 时观察到 $Y_i(1)$；$W_i=0$ 时观察到 $Y_i(0)$。等价写法：$Y_i^{\text{obs}} = Y_i(W_i)$。

**前提**：需要 SUTVA 成立。若 SUTVA 被违反，需写成 $Y_i(\mathbf{W})$，即结果依赖于整个分配向量。

---

### 1.2 个体处理效应 (Individual Treatment Effect)

$$\tau_i = Y_i(1) - Y_i(0)$$

**含义**：单位 $i$ 因接受处理而产生的因果效应。

**根本问题**：由于因果推断的根本问题（Fundamental Problem of Causal Inference），我们永远无法直接观察 $\tau_i$——对每个单位，$Y_i(1)$ 和 $Y_i(0)$ 中只有一个可观测。

---

### 1.3 平均处理效应 (Average Treatment Effect, ATE)

$$\text{ATE} = \tau = \frac{1}{N}\sum_{i=1}^{N} \tau_i = \frac{1}{N}\sum_{i=1}^{N}[Y_i(1) - Y_i(0)] = \bar{Y}(1) - \bar{Y}(0)$$

| 符号 | 含义 |
|------|------|
| $N$ | 总单位数（有限总体） |
| $\bar{Y}(1) = \frac{1}{N}\sum_{i=1}^N Y_i(1)$ | 所有单位在处理下的总体均值 |
| $\bar{Y}(0) = \frac{1}{N}\sum_{i=1}^N Y_i(0)$ | 所有单位在对照下的总体均值 |

**直觉**：如果所有人都接受处理，平均结果减去如果所有人都不接受处理的平均结果。

---

### 1.4 处理组平均处理效应 (ATT) 与对照组平均处理效应 (ATC)

$$\text{ATT} = \frac{1}{N_t}\sum_{i: W_i=1}[Y_i(1) - Y_i(0)]$$

$$\text{ATC} = \frac{1}{N_c}\sum_{i: W_i=0}[Y_i(1) - Y_i(0)]$$

| 符号 | 含义 |
|------|------|
| $N_t$ | 处理组单位数 |
| $N_c$ | 对照组单位数，$N_c = N - N_t$ |

**关系**：$\text{ATE} = \frac{N_t}{N} \cdot \text{ATT} + \frac{N_c}{N} \cdot \text{ATC}$

**注意**：在 CRD 下，$E[\text{ATT}] = \text{ATE}$（期望相等），但对于任意具体分配，ATT 和 ATE 可能不同（尤其当处理效应异质时）。

---

## 2. 分配机制 (Assignment Mechanisms)

### 2.1 伯努利试验 (Bernoulli Trial)

$$W_i \overset{\text{iid}}{\sim} \text{Bernoulli}(p), \quad i = 1, 2, \ldots, N$$

**分配向量概率**：

$$\Pr(\mathbf{W} = \mathbf{w}) = p^{\sum_i w_i} \cdot (1-p)^{N - \sum_i w_i}$$

**边际概率**：$\Pr(W_i = 1) = p$

**关键特征**：
- 各单位分配**独立**
- 处理组大小**随机**，期望为 $Np$
- 分配空间 $|\mathbb{W}^+| = 2^N$（所有二元向量）

---

### 2.2 完全随机化设计 (Completely Randomized Design, CRD)

**可能分配数**：

$$|\mathbb{W}^+| = \binom{N}{N_t} = \frac{N!}{N_t! \cdot N_c!}$$

**每个分配的概率**：

$$\Pr(\mathbf{W} = \mathbf{w}) = \frac{1}{\binom{N}{N_t}}, \quad \mathbf{w} \in \mathbb{W}^+$$

**边际概率**：

$$\Pr(W_i = 1) = \frac{N_t}{N}$$

**条件概率（非独立性）**：

$$\Pr(W_j = 1 \mid W_i = 1) = \frac{N_t - 1}{N - 1} \neq \frac{N_t}{N}$$

**关键特征**：
- 处理组大小**固定**为 $N_t$
- 各单位分配**不独立**（知道一个单位的分配会改变其他单位的概率）
- $\mathbb{W}^+$ 仅包含恰好有 $N_t$ 个 1 的向量

---

### 2.3 分层随机化设计 (Stratified / Block Randomized Design)

将单位分为 $L$ 个层 $S_1, S_2, \ldots, S_L$，在每层内独立执行 CRD。

**层内边际概率**：

$$\Pr(W_i = 1 \mid i \in S_l) = \frac{N_{t,l}}{N_l}$$

| 符号 | 含义 |
|------|------|
| $N_l$ | 第 $l$ 层的单位总数 |
| $N_{t,l}$ | 第 $l$ 层的处理组大小 |

**可能分配总数**：

$$|\mathbb{W}^+| = \prod_{l=1}^{L} \binom{N_l}{N_{t,l}}$$

---

### 2.4 配对实验 (Paired Randomized Experiment)

分层的极端情况：每层大小为 2，$K = N/2$ 对。

**可能分配数**：

$$|\mathbb{W}^+| = 2^K$$

**边际概率**：$\Pr(W_i = 1) = 0.5$

---

### 2.5 三种分配机制对比

| 特征 | 伯努利 | CRD | 配对 |
|------|--------|-----|------|
| $|\mathbb{W}^+|$ | $2^N$ | $\binom{N}{N_t}$ | $2^{N/2}$ |
| 处理组大小 | 随机 | 固定 | 固定 |
| 单位间独立性 | 独立 | 不独立 | 对内不独立，对间独立 |
| 协变量平衡 | 期望平衡 | 期望平衡 | 分层变量精确平衡 |

---

## 3. Fisher 推断 (Fisherian Inference)

### 3.1 精确零假设 (Sharp Null Hypothesis)

$$H_0: Y_i(1) = Y_i(0) \quad \text{for all } i = 1, 2, \ldots, N$$

**含义**：处理对**每一个**单位都没有任何效应（比"平均效应为零"更强）。

**关键性质**：在 sharp null 下，所有缺失的潜在结果都被确定——$Y_i(1) = Y_i(0) = Y_i^{\text{obs}}$，科学表完全填满。

---

### 3.2 常数处理效应假设 (Constant Treatment Effect, CTE)

$$H_0(\tau_0): Y_i(1) - Y_i(0) = \tau_0 \quad \text{for all } i$$

**在此假设下补全科学表**：
- 若 $W_i = 1$（处理单位）：$Y_i(0) = Y_i^{\text{obs}} - \tau_0$
- 若 $W_i = 0$（对照单位）：$Y_i(1) = Y_i^{\text{obs}} + \tau_0$

**注意**：Sharp null 是 CTE 在 $\tau_0 = 0$ 时的特例。

---

### 3.3 检验统计量 (Test Statistic)

最常用的是**差值均值**：

$$T = \bar{Y}_t^{\text{obs}} - \bar{Y}_c^{\text{obs}} = \frac{1}{N_t}\sum_{i: W_i=1} Y_i^{\text{obs}} - \frac{1}{N_c}\sum_{i: W_i=0} Y_i^{\text{obs}}$$

**注意**：Fisher 框架对检验统计量没有限制，可以使用差值均值、Welch t 统计量、Wilcoxon 秩和、KS 统计量等任意函数。选择影响**功效**但不影响**有效性**。

---

### 3.4 Fisher p 值 (Fisher p-value)

**双侧 p 值**：

$$p = \frac{\#\{\mathbf{w} \in \mathbb{W}^+ : |T(\mathbf{w})| \geq |T^{\text{obs}}|\}}{|\mathbb{W}^+|}$$

**单侧 p 值**（检验 $T \geq T^{\text{obs}}$）：

$$p = \frac{\#\{\mathbf{w} \in \mathbb{W}^+ : T(\mathbf{w}) \geq T^{\text{obs}}\}}{|\mathbb{W}^+|}$$

**在 CRD 下**：$|\mathbb{W}^+| = \binom{N}{N_t}$，每种分配等概率。

**正确解读**：在零假设为真的条件下，所有等可能分配中，产生至少与观察值一样极端的检验统计量的比例。

**常见误解**：
- $\times$ p 值不是"零假设为真的概率"
- $\times$ p > 0.05 不意味着"接受零假设"
- $\checkmark$ p 值是数据与零假设的兼容性度量

---

### 3.5 最小可能 p 值

在 CRD 下，p 值的分辨率受限于分配总数：

$$p_{\min} = \frac{1}{\binom{N}{N_t}}$$

**例**：$N=4, N_t=2 \Rightarrow \binom{4}{2}=6 \Rightarrow p_{\min} = 1/6 \approx 0.167$（无法达到 0.05 显著性）

**例**：$N=6, N_t=3 \Rightarrow \binom{6}{3}=20 \Rightarrow p_{\min} = 1/20 = 0.05$（刚好在边界）

---

### 3.6 Fisher 置信区间 (Fisher Confidence Interval via Test Inversion)

$$\text{CI}_{1-\alpha} = \{\tau_0 : p(\tau_0) > \alpha\}$$

**构造步骤**：
1. 对每个候选值 $\tau_0$，假设常数处理效应 $\tau_i = \tau_0$
2. 在此假设下补全科学表
3. 进行 Fisher 随机化检验，得到 $p(\tau_0)$
4. 收集所有不被拒绝的 $\tau_0$ 值

**性质**：精确（任意样本量）；假设常数处理效应；计算成本高。

---

### 3.7 Hodges-Lehmann 估计量

$$\hat{\tau}_{\text{HL}} = \text{median}\{Y_i^{\text{obs}} - Y_j^{\text{obs}} : i \in \text{Treatment}, j \in \text{Control}\}$$

| 项目 | 说明 |
|------|------|
| 成对差异总数 | $N_t \times N_c$ 个 |
| 估计含义 | 使 Fisher p 值最大的 $\tau_0$ 值（与数据最兼容的常数效应） |
| 稳健性 | 中位数不受异常值影响，比均值更稳健 |

**两种等价计算方式**：
1. 计算所有 $N_t \times N_c$ 个成对差异的中位数
2. 遍历所有 $\tau_0$，找到最大化 $p(\tau_0)$ 的值

---

## 4. Neyman 推断 (Neymanian Inference)

### 4.1 差值均值估计量 (Difference-in-Means Estimator)

$$\hat{\tau} = \bar{Y}_t^{\text{obs}} - \bar{Y}_c^{\text{obs}}$$

$$\bar{Y}_t^{\text{obs}} = \frac{1}{N_t}\sum_{i: W_i=1} Y_i^{\text{obs}}, \quad \bar{Y}_c^{\text{obs}} = \frac{1}{N_c}\sum_{i: W_i=0} Y_i^{\text{obs}}$$

---

### 4.2 无偏性 (Unbiasedness)

$$\mathbb{E}[\hat{\tau}] = \tau$$

**证明思路**（基于 CRD 的对称性）：

$$\mathbb{E}[\bar{Y}_t^{\text{obs}}] = \bar{Y}(1) = \frac{1}{N}\sum_{i=1}^N Y_i(1)$$

$$\mathbb{E}[\bar{Y}_c^{\text{obs}}] = \bar{Y}(0) = \frac{1}{N}\sum_{i=1}^N Y_i(0)$$

因为每个单位被分配到处理组的概率都是 $N_t/N$，所以处理组样本均值是总体处理均值的无偏估计。

$$\mathbb{E}[\hat{\tau}] = \bar{Y}(1) - \bar{Y}(0) = \tau \quad \checkmark$$

**重要**：无偏性**不要求**常数处理效应。异质效应影响方差，不影响无偏性。

---

### 4.3 估计量的真实方差 (True Variance)

$$\text{Var}(\hat{\tau}) = \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} - \frac{S^2(\tau)}{N}$$

| 符号 | 定义 | 含义 |
|------|------|------|
| $S^2(1)$ | $\frac{1}{N-1}\sum_{i=1}^N [Y_i(1) - \bar{Y}(1)]^2$ | 处理下潜在结果的总体方差 |
| $S^2(0)$ | $\frac{1}{N-1}\sum_{i=1}^N [Y_i(0) - \bar{Y}(0)]^2$ | 对照下潜在结果的总体方差 |
| $S^2(\tau)$ | $\frac{1}{N-1}\sum_{i=1}^N (\tau_i - \tau)^2$ | 个体处理效应的方差 |

**关键性质**：$S^2(\tau) \geq 0$，因此：

$$\text{Var}(\hat{\tau}) \leq \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c}$$

**直觉**：处理效应的异质性（$S^2(\tau) > 0$）实际上**降低**了估计量的方差。当效应齐次（$S^2(\tau) = 0$）时，上式取等号。

---

### 4.4 保守方差估计量 (Conservative Variance Estimator)

$$\hat{V}_{\text{Neyman}} = \frac{s_t^2}{N_t} + \frac{s_c^2}{N_c}$$

| 符号 | 定义 |
|------|------|
| $s_t^2$ | $\frac{1}{N_t-1}\sum_{i: W_i=1}[Y_i^{\text{obs}} - \bar{Y}_t^{\text{obs}}]^2$（处理组样本方差） |
| $s_c^2$ | $\frac{1}{N_c-1}\sum_{i: W_i=0}[Y_i^{\text{obs}} - \bar{Y}_c^{\text{obs}}]^2$（对照组样本方差） |

**保守性**：

$$\mathbb{E}[\hat{V}_{\text{Neyman}}] = \frac{S^2(1)}{N_t} + \frac{S^2(0)}{N_c} \geq \text{Var}(\hat{\tau})$$

- 保守 = 期望上过度估计方差 $\Rightarrow$ 置信区间偏宽 $\Rightarrow$ 覆盖率 $\geq 95\%$
- 当 $\tau_i = \tau$（齐次效应）时，$S^2(\tau) = 0$，保守性消失，估计精确

**注意**："保守"是期望意义上的性质。对于任何具体数据集，实现值可能低于真实方差。

---

### 4.5 标准误 (Standard Error)

$$SE = \sqrt{\hat{V}_{\text{Neyman}}} = \sqrt{\frac{s_t^2}{N_t} + \frac{s_c^2}{N_c}}$$

---

### 4.6 Neyman 置信区间 (Neyman Confidence Interval)

$$95\% \text{ CI}: \hat{\tau} \pm 1.96 \times SE$$

$$\text{一般形式}: \hat{\tau} \pm z_{\alpha/2} \times SE$$

| 置信水平 | $z_{\alpha/2}$ |
|---------|---------------|
| 90% | 1.645 |
| 95% | 1.96 |
| 99% | 2.576 |

**依赖假设**：
1. **中心极限定理 (CLT)**：$N$ 足够大时，$\hat{\tau}$ 近似正态分布
2. **保守方差估计**：使用 $\hat{V}_{\text{Neyman}}$ 近似真实方差

**正确解读**：如果重复实验很多次，每次都构造 95% CI，约 95% 的区间会包含真实的 $\tau$。描述的是**方法的可靠性**，不是某个特定区间包含 $\tau$ 的概率。

**错误解读**："真实 ATE 落在此区间内的概率是 95%"——$\tau$ 是固定常数，不存在"概率"。

---

## 5. 贝叶斯因果推断 (Bayesian Causal Inference)

### 5.1 贝叶斯定理 (Bayes' Theorem)

$$p(\text{缺失} \mid \text{观测}) = \frac{p(\text{观测} \mid \text{缺失}) \times p(\text{缺失})}{p(\text{观测})}$$

$$\text{后验} \propto \text{似然} \times \text{先验}$$

在因果推断语境中：
- **缺失** = 未观测到的潜在结果
- **观测** = 观测到的结果 + 处理分配
- **先验** $p(\text{缺失})$：对缺失值的先验信念
- **似然** $p(\text{观测} \mid \text{缺失})$：给定缺失值后数据的似然度
- **后验** $p(\text{缺失} \mid \text{观测})$：综合数据后对缺失值的更新信念

---

### 5.2 参数化模型 (Parametric Model)

假设潜在结果服从正态分布：

$$Y_i(1) \sim N(\mu_1, \sigma_1^2), \quad Y_i(0) \sim N(\mu_0, \sigma_0^2)$$

**多次填补 (Multiple Imputation) 算法**：

```
for m = 1 to M:
  (1) 从参数后验中抽样 μ₁*, σ₁*², μ₀*, σ₀*²
  (2) 对每个对照单位 i: 从 N(μ₁*, σ₁*²) 抽样得 Ŷᵢ(1)
      对每个处理单位 i: 从 N(μ₀*, σ₀*²) 抽样得 Ŷᵢ(0)
  (3) 计算 τ̂(m) = (1/N) Σᵢ [Ŷᵢ(1) - Ŷᵢ(0)]
end

后验分布 ≈ {τ̂(1), τ̂(2), ..., τ̂(M)}
```

---

### 5.3 贝叶斯自助法 (Bayesian Bootstrap)

非参数方法，不假设任何分布形式：

```
for m = 1 to M:
  (1) 对每个对照单位 i: 从处理组观测值 {Yⱼ(1): Wⱼ=1} 中有放回抽样
  (2) 对每个处理单位 i: 从对照组观测值 {Yⱼ(0): Wⱼ=0} 中有放回抽样
  (3) 计算 τ̂(m)
end
```

**优点**：无分布假设；**缺点**：采样池太小时分布粗糙。

---

### 5.4 后验总结

- **后验均值**：$\hat{\tau}_{\text{Bayes}} = \frac{1}{M}\sum_{m=1}^M \hat{\tau}^{(m)}$
- **后验标准差**：$\text{SD} = \sqrt{\frac{1}{M-1}\sum_{m=1}^M (\hat{\tau}^{(m)} - \hat{\tau}_{\text{Bayes}})^2}$
- **95% 可信区间 (Credible Interval)**：取 $\hat{\tau}^{(m)}$ 的 2.5% 和 97.5% 分位数

**可信区间 vs 置信区间**：

| | 置信区间 (Neyman) | 可信区间 (Bayesian) |
|---|---|---|
| 解读 | 重复实验中 95% 的区间包含 $\tau$ | 真实 $\tau$ 有 95% 的后验概率在此区间内 |
| 哲学 | 频率论 | 贝叶斯 |

---

## 6. 关键假设汇总

### 6.1 SUTVA (Stable Unit Treatment Value Assumption)

**假设 1：无干扰 (No Interference)**

$$Y_i(\mathbf{W}) = Y_i(W_i)$$

单位 $i$ 的潜在结果仅取决于自身的分配，不受其他单位影响。

**违反例**：疫苗试验中的群体免疫；社交网络实验中的同伴效应。

**假设 2：无隐性处理变异 (No Hidden Versions of Treatment)**

对所有接受同一处理的单位，处理的定义相同。

**违反例**：不同外科医生执行同一手术但技术水平不同。

---

### 6.2 分配机制的三大性质

**个体主义 (Individualistic)**：

$$\Pr(W_i \mid \mathbf{X}, \mathbf{Y}(0), \mathbf{Y}(1)) = \Pr(W_i \mid X_i, Y_i(0), Y_i(1))$$

**概率性 (Probabilistic)**：

$$0 < \Pr(W_i = 1) < 1$$

**无混淆 (Unconfoundedness)**：

$$\Pr(\mathbf{W} \mid \mathbf{X}, \mathbf{Y}(0), \mathbf{Y}(1)) = \Pr(\mathbf{W} \mid \mathbf{X})$$

分配不依赖于潜在结果，只依赖于可观测协变量。

---

## 7. 三大推断方法完整对比

| 维度 | Fisher | Neyman | Bayesian |
|------|--------|--------|----------|
| **核心问题** | 有没有效果？ | 效果多大？ | 效果的完整分布？ |
| **输出** | p 值 | $\hat{\tau}$ + SE + CI | 后验分布 |
| **零假设** | Sharp null: $\tau_i = 0, \forall i$ | Weak null: $\tau = 0$ | 无需零假设 |
| **处理效应** | 需假设齐次（CI 时） | 可异质 | 可异质 |
| **样本量** | 任意（精确） | 需较大（CLT） | 灵活（先验补偿） |
| **关键假设** | Sharp null + CRD | CRD + CLT | 模型 + 先验 |
| **方差** | 从随机化分布精确推导 | 保守估计 | 后验标准差 |
| **计算** | 枚举所有分配 | 公式计算 | MCMC / Bootstrap |
| **适用场景** | 小样本检验 | 大样本估计 | 有先验信息时 |

---

## 8. 快速计算清单

### 做题步骤速查

**Fisher 随机化检验**：
1. 写出 $H_0: Y_i(1) = Y_i(0), \forall i$
2. 补全科学表（所有 $Y_i(1) = Y_i(0) = Y_i^{\text{obs}}$）
3. 计算 $T^{\text{obs}} = \bar{Y}_t - \bar{Y}_c$
4. 枚举所有 $\binom{N}{N_t}$ 种分配，计算每种的 $T$
5. $p = \frac{\#\{|T| \geq |T^{\text{obs}}|\}}{\binom{N}{N_t}}$（双侧）

**Hodges-Lehmann 估计**：
1. 计算所有 $N_t \times N_c$ 个成对差异 $Y_i^{\text{obs}} - Y_j^{\text{obs}}$
2. 排序
3. 取中位数

**Neyman 推断**：
1. $\hat{\tau} = \bar{Y}_t - \bar{Y}_c$
2. $s_t^2 = \frac{1}{N_t-1}\sum_{i \in T}(Y_i - \bar{Y}_t)^2$，$s_c^2$ 类似
3. $\hat{V} = s_t^2/N_t + s_c^2/N_c$
4. $SE = \sqrt{\hat{V}}$
5. $95\% \text{ CI} = \hat{\tau} \pm 1.96 \times SE$

**贝叶斯 Bootstrap**：
1. 确定两个采样池：处理组观测值 $\{Y_j: W_j=1\}$，对照组观测值 $\{Y_j: W_j=0\}$
2. 对每个缺失值从对应池中有放回抽样
3. 补全科学表，计算 ATE
4. 重复 $M$ 次，得到后验分布
