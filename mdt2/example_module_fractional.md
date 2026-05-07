# 🐢 超慢速 Fractional Factorial 专题：$2^{K-p}$ 部分析因设计

> 📖 这一讲对应老师课上说的 "some fractional factorial exercise we did in class" 考点。它**不在** Stat_140 正式模块里（Module 13 结尾提了一句 "超出 Midterm 2"），但老师已经把它纳入 Midterm 2，所以我们按 Box-Hunter-Hunter / Montgomery DOE 标准讲法走一遍。符号与 Module 13 的 $2^K$ 完全一致。
>
> This topic isn't in an official module (Module 13 just says "beyond MDT2"), but your instructor added it. We'll follow the standard DOE treatment, keeping notation consistent with Module 13's $2^K$ material.

---

# 第一部分：为什么要"部分"析因？

## 全析因的代价

**回忆 Module 13：** $K$ 个两水平因子 → $2^K$ 种处理组合 → 每组 $n$ 个单位 → 总样本 $N = n \cdot 2^K$。

| $K$ | $2^K$ 处理组合 | 若 $n=4$ 则 $N=$ |
|---|---|---|
| 3 | 8 | 32 |
| 4 | 16 | 64 |
| 5 | 32 | 128 |
| 6 | 64 | 256 |
| 7 | 128 | 512 |

**问题：** $K$ 一大，$2^K$ 爆炸。实际场景里做 128 或 256 次独立试验（每次要烧一炉、刻一片、跑一次反应、给一批病人用药）**代价极高**。

## 关键观察（效应稀疏性原理 Effect Sparsity Principle）

**经验事实：**
- **主效应**通常**大**（占总变异的大头）
- **二阶交互 (2fi)** 通常**中等**
- **三阶以上交互**通常**可忽略** —— 实验科学里极少见显著的 3fi / 4fi

**那我们为什么还要花 $2^K$ 次运行去估计 $2^K - 1$ 个效应？** 大多数高阶交互 ≈ 0，估计它们是浪费。

## 部分析因的思路

**只跑 $2^{K-p}$ 次运行**（$K$ 个因子里的 $p$ 个由其它因子组合"派生"），付出"别名 (aliasing)"的代价——两个效应共享同一个估计；我们依赖效应稀疏性原理来**假设**其中一个（通常是高阶的那个）可以忽略。

**标准术语：**

| 名词 | 英文 | 含义 |
|---|---|---|
| 半分式 | half-fraction | $2^{K-1}$（一半运行） |
| 四分之一分式 | quarter-fraction | $2^{K-2}$ |
| $1/2^p$ 分式 | $1/2^p$ fraction | $2^{K-p}$ |
| 生成元 | generator | 如 $D = ABC$，定义 "派生因子"列 |
| 定义关系 | defining relation | $I = ABCD$ (由生成元推出) |
| 别名 / 混淆 | alias / confound | 两个效应用同一个对比估计 |
| 分辨度 | resolution | 定义关系里最短词的长度 |

---

# 第二部分：最简单的案例先热身 —— $2^{3-1}$

**在跳到 $2^{4-1}$ 前，先看最小的半分式。**

## 场景

**问题：** 你想优化一个烘焙配方的 3 个因素 (温度 $A$、时间 $B$、糖量 $C$)。全析因要 $2^3=8$ 次烘焙，你的烤箱每天只能跑 **4 炉**。

**方案：** 用 $2^{3-1} = 4$ 次运行的半分式。

## 构造设计矩阵

**第一步：** 写出 $K-p = 2$ 个"基础因子"的 $2^2$ 字典序（这里选 $A, B$）：

| Run | $A$ | $B$ |
|---|---|---|
| 1 | $-$ | $-$ |
| 2 | $-$ | $+$ |
| 3 | $+$ | $-$ |
| 4 | $+$ | $+$ |

**第二步：** 派生第三列 $C$，用生成元 $C = AB$（逐元素乘）：

| Run | $A$ | $B$ | $C = AB$ |
|---|---|---|---|
| 1 | $-$ | $-$ | $(-)(-)=+$ |
| 2 | $-$ | $+$ | $(-)(+)=-$ |
| 3 | $+$ | $-$ | $(+)(-)=-$ |
| 4 | $+$ | $+$ | $(+)(+)=+$ |

**这就是 Principal Fraction**（主要分式）。另一半是 $C = -AB$，对应的 4 行：

| Run | $A$ | $B$ | $C = -AB$ |
|---|---|---|---|
| 5 | $-$ | $-$ | $-$ |
| 6 | $-$ | $+$ | $+$ |
| 7 | $+$ | $-$ | $+$ |
| 8 | $+$ | $+$ | $-$ |

合起来就是完整的 $2^3 = 8$ 运行。所以"半分式"真的是 8 的一半。

## 定义关系

**生成元** $C = AB$ $\Rightarrow$ 两边乘 $C$：$C^2 = ABC \Rightarrow I = ABC$（因为 $C^2 = I$，正负号平方都 $=+$）。

$$\boxed{\text{Defining relation: } I = ABC}$$

## 别名结构

用"两边乘一个字母"的代数（规则：$A^2 = B^2 = C^2 = I$，$I \cdot X = X$）：

| 效应 | 两边乘 | 得到 | 别名对 |
|---|---|---|---|
| $A$ | $A$ | $A = A \cdot ABC = A^2BC = BC$ | $A \equiv BC$ |
| $B$ | $B$ | $B = B \cdot ABC = AC$ | $B \equiv AC$ |
| $C$ | $C$ | $C = C \cdot ABC = AB$ | $C \equiv AB$ |
| $I$ | — | $I = ABC$ | $I \equiv ABC$ |

**每个主效应都和一个 2fi 混淆！** 定义关系最短词长 $=3$ $\Rightarrow$ **分辨度 III**。

**代价：** 如果 $BC$ 有真实交互，$\hat\tau_A$ 其实在估计 $\tau_A + \tau_{BC}$，两者无法分离。Res III 设计**很冒险**，只有在强烈相信 "2fi 都可忽略" 时才用（经验上很少成立）。

---

# 第三部分：主课案例 —— $2^{4-1}$ 蛋白质产量实验

## 场景设定

**问题：** 你是一家 biotech 的研究员，在优化一个重组蛋白表达实验。候选的四个关键工艺因子：

| 因子 | 记号 | $-$ 水平 | $+$ 水平 |
|---|---|---|---|
| 诱导温度 (induction temp) | $A$ | 25°C | 37°C |
| 诱导时间 (induction time) | $B$ | 4 h | 16 h |
| IPTG 浓度 | $C$ | 0.1 mM | 1.0 mM |
| 摇床转速 (shaking rpm) | $D$ | 150 rpm | 250 rpm |

**输出 $Y$：** 每升菌液蛋白产量 (mg/L)。

**挑战：** 一次完整发酵 + 纯化需要 72 小时 + 一个熟练技术员 + 约 $800 耗材。**全析因 $2^4 = 16$ 次要两个月**。预算只允许 $N = 8$ 次独立运行，每组 $n = 1$ 瓶（不重复）。

## 选择生成元

**可能的生成元（4 个因子选 $p=1$ 派生）：**

| 生成元 | 定义关系 | 最短词长 | 分辨度 |
|---|---|---|---|
| $D = AB$ | $I = ABD$ | 3 | **III**（差） |
| $D = AC$ | $I = ACD$ | 3 | III |
| $D = BC$ | $I = BCD$ | 3 | III |
| $D = ABC$ | $I = ABCD$ | 4 | **IV**（最佳） |

**$D = ABC$ 是唯一的 Res IV 选项**，也就是 $2^{4-1}$ 的标准选法。

> 💡 口诀：半分式选**最长的词**当定义关系，得到最高分辨度。对 $2^{4-1}$ 就是 $I = ABCD$。

## 设计矩阵（$D = ABC$）

**第一步：** $A, B, C$ 按 $2^3$ 字典序列表（8 行，主列慢到快即 $A,B,C$）：

| Run | $A$ | $B$ | $C$ |
|---|---|---|---|
| 1 | $-$ | $-$ | $-$ |
| 2 | $-$ | $-$ | $+$ |
| 3 | $-$ | $+$ | $-$ |
| 4 | $-$ | $+$ | $+$ |
| 5 | $+$ | $-$ | $-$ |
| 6 | $+$ | $-$ | $+$ |
| 7 | $+$ | $+$ | $-$ |
| 8 | $+$ | $+$ | $+$ |

**第二步：** 每行 $D = A \cdot B \cdot C$（三个符号相乘）：

| Run | $A$ | $B$ | $C$ | $D = ABC$ |
|---|---|---|---|---|
| 1 | $-$ | $-$ | $-$ | $(-)(-)(-) = -$ |
| 2 | $-$ | $-$ | $+$ | $(-)(-)(+) = +$ |
| 3 | $-$ | $+$ | $-$ | $(-)(+)(-) = +$ |
| 4 | $-$ | $+$ | $+$ | $(-)(+)(+) = -$ |
| 5 | $+$ | $-$ | $-$ | $(+)(-)(-) = +$ |
| 6 | $+$ | $-$ | $+$ | $(+)(-)(+) = -$ |
| 7 | $+$ | $+$ | $-$ | $(+)(+)(-) = -$ |
| 8 | $+$ | $+$ | $+$ | $(+)(+)(+) = +$ |

**快速检验定义关系 $I = ABCD$：** 每行四列相乘都应 $=+1$。
- Run 1: $(-)(-)(-)(-) = +$ ✓
- Run 2: $(-)(-)(+)(+) = +$ ✓
- Run 7: $(+)(+)(-)(-) = +$ ✓
- 都通过 → 确实是 Principal Fraction。

## 别名表（完整）

**$2^4$ 有 $2^4 - 1 = 15$ 个效应 + 截距 = 16 项，配 8 行只能给出 8 个独立列。** 所以 16 项分成 **8 对别名**。

**规则：** 两边乘一个字母/字母组合（$X^2 = I$）：

| 起点 | $\times (I=ABCD)$ | 别名 |
|---|---|---|
| $I$ | — | $I \equiv ABCD$ |
| $A$ | $A \cdot ABCD = BCD$ | $A \equiv BCD$ |
| $B$ | $B \cdot ABCD = ACD$ | $B \equiv ACD$ |
| $C$ | $C \cdot ABCD = ABD$ | $C \equiv ABD$ |
| $D$ | $D \cdot ABCD = ABC$ | $D \equiv ABC$ |
| $AB$ | $AB \cdot ABCD = CD$ | $AB \equiv CD$ |
| $AC$ | $AC \cdot ABCD = BD$ | $AC \equiv BD$ |
| $AD$ | $AD \cdot ABCD = BC$ | $AD \equiv BC$ |

**关键结构：**
- **主效应 ≡ 3 阶交互**：通常 3fi 可忽略 $\Rightarrow$ 主效应"干净"估出
- **2fi ≡ 2fi**：两两混淆，无法分离 $\Rightarrow$ 这是代价

这就是 **Resolution IV 的定义特征**。

## 分辨度含义（权威版本）

**定义：** Resolution $R$ = 定义关系中**最短词**的长度。

**通用法则**（记口诀）：

| Res | 含义 |
|---|---|
| **III** | 主效应 $\equiv$ 2fi（危险） |
| **IV** | 主效应 $\equiv$ 3fi（一般可接受），2fi $\equiv$ 2fi |
| **V** | 主效应 & 2fi 都**彼此干净**，但 2fi $\equiv$ 3fi |
| **VI+** | 几乎等价于全析因 |

**经验法则：** Res IV 是"screening" 筛选实验的**甜点**——用少量实验筛出活跃主效应，再做更大的 follow-up 来分离 2fi。

---

# 第四部分：观测数据与主效应计算

## 实验结果

假设 8 次运行的蛋白产量 (mg/L) 是：

| Run | $A$ | $B$ | $C$ | $D$ | $Y^{obs}$ |
|---|---|---|---|---|---|
| 1 | $-$ | $-$ | $-$ | $-$ | 38 |
| 2 | $-$ | $-$ | $+$ | $+$ | 45 |
| 3 | $-$ | $+$ | $-$ | $+$ | 42 |
| 4 | $-$ | $+$ | $+$ | $-$ | 48 |
| 5 | $+$ | $-$ | $-$ | $+$ | 56 |
| 6 | $+$ | $-$ | $+$ | $-$ | 55 |
| 7 | $+$ | $+$ | $-$ | $-$ | 58 |
| 8 | $+$ | $+$ | $+$ | $+$ | 70 |

> 每组 $n = 1$ 瓶 $\Rightarrow \bar Y^{obs}_{\mathbf{w}} = Y^{obs}_{\mathbf{w}}$。

## 核心公式（与 Module 13 完全一致）

$$\hat\tau_F = \frac{1}{2^{K'-1}} \mathbf{\bar Y}^{obs\,\top} \mathbf{g}_F, \quad K' = K - p = 3$$

**为什么 $K' = 3$？** 因为 8 = $2^3$，半分式 $2^{4-1}$ **从计算角度看就是一个 $2^3$ 全析因**（只不过每一列有不同的"身份"：$A, B, C, D=ABC, AB=CD, AC=BD, AD=BC$）。所以除数 $2^{K'-1} = 2^{3-1} = 4$。

## 主效应 $\hat\tau_A$

**对比向量** $\mathbf{g}_A$ = $A$ 列符号（$\pm1$ 向量）：

$$\mathbf{g}_A = (-1, -1, -1, -1, +1, +1, +1, +1)^\top$$

**内积计算：**

$$\mathbf{\bar Y}^{obs\,\top} \mathbf{g}_A = -38 - 45 - 42 - 48 + 56 + 55 + 58 + 70$$

**逐步：**
- "$-$"组和：$38 + 45 + 42 + 48 = 173$
- "$+$"组和：$56 + 55 + 58 + 70 = 239$
- 内积 $= 239 - 173 = 66$

$$\boxed{\hat\tau_A = \frac{66}{4} = 16.5 \text{ mg/L}}$$

**但严格说，$A \equiv BCD$**：

$$\hat\tau_A \text{ 实际估计的是 } \tau_A + \tau_{BCD}$$

**解读：** 若假设 3fi $\tau_{BCD} \approx 0$（效应稀疏性），则 $\hat\tau_A \approx \tau_A$：**从 25°C 升到 37°C 平均增加 16.5 mg/L 产量**。

## 主效应 $\hat\tau_B$

$\mathbf{g}_B = (-1, -1, +1, +1, -1, -1, +1, +1)^\top$

$$\mathbf{\bar Y}^\top \mathbf{g}_B = -38 - 45 + 42 + 48 - 56 - 55 + 58 + 70$$

**逐步：**
- "$-$"组和：$38 + 45 + 56 + 55 = 194$
- "$+$"组和：$42 + 48 + 58 + 70 = 218$
- 内积 $= 218 - 194 = 24$

$$\hat\tau_B = \frac{24}{4} = 6.0 \text{ mg/L} \quad (B \equiv ACD)$$

## 主效应 $\hat\tau_C$

$\mathbf{g}_C = (-1, +1, -1, +1, -1, +1, -1, +1)^\top$

$$\mathbf{\bar Y}^\top \mathbf{g}_C = -38 + 45 - 42 + 48 - 56 + 55 - 58 + 70$$

**逐步：**
- "$-$"组和：$38 + 42 + 56 + 58 = 194$
- "$+$"组和：$45 + 48 + 55 + 70 = 218$
- 内积 $= 218 - 194 = 24$

$$\hat\tau_C = \frac{24}{4} = 6.0 \text{ mg/L} \quad (C \equiv ABD)$$

## 主效应 $\hat\tau_D$

**$D$ 列符号**（从设计矩阵直接读）：$\mathbf{g}_D = (-1, +1, +1, -1, +1, -1, -1, +1)^\top$

**验证：** $\mathbf{g}_D = \mathbf{g}_A \odot \mathbf{g}_B \odot \mathbf{g}_C$（因为 $D = ABC$）：

| $i$ | $g_A$ | $g_B$ | $g_C$ | 乘积 | $g_D$ |
|---|---|---|---|---|---|
| 1 | $-$ | $-$ | $-$ | $-$ | $-$ ✓ |
| 2 | $-$ | $-$ | $+$ | $+$ | $+$ ✓ |
| 3 | $-$ | $+$ | $-$ | $+$ | $+$ ✓ |
| 4 | $-$ | $+$ | $+$ | $-$ | $-$ ✓ |
| 5 | $+$ | $-$ | $-$ | $+$ | $+$ ✓ |
| 6 | $+$ | $-$ | $+$ | $-$ | $-$ ✓ |
| 7 | $+$ | $+$ | $-$ | $-$ | $-$ ✓ |
| 8 | $+$ | $+$ | $+$ | $+$ | $+$ ✓ |

$$\mathbf{\bar Y}^\top \mathbf{g}_D = -38 + 45 + 42 - 48 + 56 - 55 - 58 + 70$$

**逐步：**
- "$-$"组和：$38 + 48 + 55 + 58 = 199$
- "$+$"组和：$45 + 42 + 56 + 70 = 213$
- 内积 $= 213 - 199 = 14$

$$\hat\tau_D = \frac{14}{4} = 3.5 \text{ mg/L} \quad (D \equiv ABC)$$

---

# 第五部分：2 因子交互的计算（别名成对）

**半分式里 2fi 都是成对的**：$AB \equiv CD$, $AC \equiv BD$, $AD \equiv BC$。我们只能估计**对**，无法分离个体。

## $\hat\tau_{AB}$（$\equiv \hat\tau_{CD}$）

$\mathbf{g}_{AB} = \mathbf{g}_A \odot \mathbf{g}_B$：

| $i$ | $g_A$ | $g_B$ | $g_{AB}$ |
|---|---|---|---|
| 1 | $-$ | $-$ | $+$ |
| 2 | $-$ | $-$ | $+$ |
| 3 | $-$ | $+$ | $-$ |
| 4 | $-$ | $+$ | $-$ |
| 5 | $+$ | $-$ | $-$ |
| 6 | $+$ | $-$ | $-$ |
| 7 | $+$ | $+$ | $+$ |
| 8 | $+$ | $+$ | $+$ |

$$\mathbf{\bar Y}^\top \mathbf{g}_{AB} = +38 + 45 - 42 - 48 - 56 - 55 + 58 + 70$$

**逐步：**
- "$+$"组和：$38 + 45 + 58 + 70 = 211$
- "$-$"组和：$42 + 48 + 56 + 55 = 201$
- 内积 $= 211 - 201 = 10$

$$\hat\tau_{AB} = \frac{10}{4} = 2.5 \text{ mg/L} \quad \text{(估计的是 } \tau_{AB} + \tau_{CD})$$

## $\hat\tau_{AC}$（$\equiv \hat\tau_{BD}$）

$\mathbf{g}_{AC} = \mathbf{g}_A \odot \mathbf{g}_C = (+,-,+,-,-,+,-,+)$

$$\mathbf{\bar Y}^\top \mathbf{g}_{AC} = +38 - 45 + 42 - 48 - 56 + 55 - 58 + 70 = -2$$

$$\hat\tau_{AC} = \frac{-2}{4} = -0.5 \text{ mg/L}$$

**几乎为零** → $\tau_{AC} + \tau_{BD}$ 合计约 0；一般简化解读为两者都小。

## $\hat\tau_{AD}$（$\equiv \hat\tau_{BC}$）

$\mathbf{g}_{AD} = \mathbf{g}_A \odot \mathbf{g}_D$

**用 $D = ABC$ 代入**：$\mathbf{g}_{AD} = \mathbf{g}_A \odot (\mathbf{g}_A \odot \mathbf{g}_B \odot \mathbf{g}_C) = \mathbf{g}_B \odot \mathbf{g}_C$（因 $\mathbf{g}_A^{\odot 2} = \mathbf{1}$）。**这就是 $\mathbf{g}_{BC}$！** 这机械地验证了 $AD \equiv BC$。

$\mathbf{g}_{BC} = \mathbf{g}_B \odot \mathbf{g}_C = (+,-,-,+,+,-,-,+)$

$$\mathbf{\bar Y}^\top \mathbf{g}_{BC} = +38 - 45 - 42 + 48 + 56 - 55 - 58 + 70 = 12$$

$$\hat\tau_{AD} = \hat\tau_{BC} = \frac{12}{4} = 3.0 \text{ mg/L}$$

## 效应汇总表

| 效应（含别名） | $\hat\tau$ | 大小排名 |
|---|---|---|
| $A \equiv BCD$ | **16.5** | 🥇 最大 |
| $B \equiv ACD$ | **6.0** | 🥈 |
| $C \equiv ABD$ | **6.0** | 🥈 |
| $D \equiv ABC$ | 3.5 | |
| $AB \equiv CD$ | 2.5 | |
| $AD \equiv BC$ | 3.0 | |
| $AC \equiv BD$ | $-0.5$ | 最小 |

---

# 第六部分：解读与决策

## 在效应稀疏假设下

**假设** 3fi 都 $\approx 0$，则：
- $\hat\tau_A \approx \tau_A = 16.5$（温度影响巨大）
- $\hat\tau_B \approx \tau_B = 6.0$（时间有中等正效应）
- $\hat\tau_C \approx \tau_C = 6.0$（IPTG 有中等正效应）
- $\hat\tau_D \approx \tau_D = 3.5$（转速影响较小）

## 2fi 的歧义

**$\hat\tau_{AB} = 2.5$ 实际是 $\tau_{AB} + \tau_{CD}$。** 我们不能从这 8 次数据区分：
- 情景 a：$\tau_{AB} = 2.5, \tau_{CD} = 0$
- 情景 b：$\tau_{AB} = 0, \tau_{CD} = 2.5$
- 情景 c：$\tau_{AB} = 5, \tau_{CD} = -2.5$ 之类

**解决方案：**
1. **领域先验**：如果生物学上 "温度 × 时间 ($AB$)" 合理但 "IPTG × 转速 ($CD$)" 不合理，就把 $\hat\tau_{AB} = 2.5$ 全算给 $AB$
2. **Fold-over / Follow-up 实验**：再跑另一半 8 次运行，合起来就是 $2^4 = 16$ 次全析因，可完全分离
3. **追加补充设计**：只加 4-8 次关键点的运行，专门分离关键的 2fi

## 最优组合

**如果效应基本加性（交互小）**，最优组合就是把所有正效应因子全拉高：
$A = +$（37°C）,  $B = +$（16h）, $C = +$（1 mM）, $D = +$（250 rpm）

**这对应哪个 Run？** $(+,+,+,+)$ 即 Run 8, $Y^{obs} = 70$ mg/L —— **实际观测最高**！ 😊

---

# 第七部分：Fisher 尖锐零与 Neyman 推断

## Fisher 推断（尖锐零）

**$H_0$：** 所有 $2^{K-p} - 1 = 7$ 个（成对的）效应同时 $=0$。

**有效分配数：** 这里 $n = 1$（每种组合只有 1 个样品），所以 Fisher 参考集的结构取决于**怎么随机化**到 8 个处理。如果**先**固定 8 个"瓶子"然后随机分配 8 种处理，参考集就是 $8! = 40320$ 种排列。每次排列下，$\hat\tau_F$ 取不同值，得到零分布。

**$p$ 值：** $p_F = \#\{|\hat\tau_F^{rand}| \ge |\hat\tau_F^{obs}|\} / 40320$

**实际做法：** 蒙特卡洛抽样 $M = 10000$ 次，而不是枚举 $8!$。

## Neyman 推断

**每组 $n = 1$ 时无法算 $s_w^2$。** 两种救急：

### 救急 1：用"可忽略高阶效应"估方差

**做法：** 认为 $\hat\tau_{AC} = -0.5$ 和 $\hat\tau_{AD} = 3.0$ 等 2fi（如果先验上不太可能存在）其实是**噪声**，把它们的方差加起来估计 $\sigma^2$。

**具体：** 假设 $p$ 个非活跃效应的 $\hat\tau_F$ 服从 $N(0, \sigma^2 / 2^{K'-2})$，用它们的样本方差反推 $\sigma^2$。

### 救急 2：做重复实验

如果能再跑 **1-2 个中心点** (center points, 所有因子 $= 0$)，可以独立地估 $\sigma^2$。

### 假设 $\hat S^2 = 4.0$（给定）

$$\widehat{\text{Var}}(\hat\tau_F) = \frac{2^{K'}}{N} \hat S^2 = \frac{8}{8} \cdot 4.0 = 4.0 \Rightarrow \text{SE}(\hat\tau_F) = 2.0$$

**$\hat\tau_A$ 的 95% CI**（按 STAT 140 惯例 $\pm 2\cdot SE$）：

$$16.5 \pm 2 \cdot 2.0 = 16.5 \pm 4.0 = (12.5,\ 20.5)$$

**远离 0** → $A$ 的主效应（在 "3fi 忽略" 假设下）非常显著。

**$\hat\tau_{AB}$ 的 95% CI：**

$$2.5 \pm 4.0 = (-1.5,\ 6.5)$$

**包含 0** → 数据不足以说 $\tau_{AB} + \tau_{CD}$ 非零。

---

# 第八部分：为什么 $I = ABCD$ 比 $I = ABD$ 好？

**对比两个候选生成元（$2^{4-1}$）：**

## 方案 A：$D = ABC \Rightarrow I = ABCD$（Res IV）

| 主效应 | 别名 |
|---|---|
| $A$ | $\equiv BCD$（3fi，可忽略）|
| $B$ | $\equiv ACD$（3fi）|
| $C$ | $\equiv ABD$（3fi）|
| $D$ | $\equiv ABC$（3fi）|

**主效应都干净。**

## 方案 B：$D = AB \Rightarrow I = ABD$（Res III）

**推导别名：**
- $A \cdot I = A \cdot ABD = BD \Rightarrow A \equiv BD$
- $B \equiv AD$
- $D \equiv AB$
- $C \cdot ABD = ABCD \Rightarrow C \equiv ABCD$

| 主效应 | 别名 |
|---|---|
| $A$ | $\equiv BD$（**2fi**！）|
| $B$ | $\equiv AD$（2fi）|
| $C$ | $\equiv ABCD$（4fi，没问题）|
| $D$ | $\equiv AB$（2fi）|

**$A, B, D$ 的主效应都和 2fi 混淆。** 如果 $BD$ 真的有效应，$\hat\tau_A$ 就被"污染"了。

**一句话规则：选最长的词当定义关系。**

---

# 第九部分：更进一步 —— $2^{5-2}$ 四分之一分式（简介）

**情景：** 5 个因子 $A, B, C, D, E$，预算只能 $N = 8 = 2^3$ 次运行。

**需要 2 个生成元。** 标准选法：

$$D = AB, \quad E = AC$$

**定义关系：** $I = ABD = ACE$（两个 "**独立**" 的关系）。

**交叉积 (generalized interaction)：** 两个定义关系**逐元素乘**得到第三个：

$$ABD \cdot ACE = A^2 BCDE = BCDE \Rightarrow I = BCDE$$

**完整定义关系 (full defining relation)：**

$$I = ABD = ACE = BCDE$$

**最短词长：** $ABD$ 和 $ACE$ 都是 3 $\Rightarrow$ **Res III**（无法避免，因为 $K - p = 3$ 太紧）。

**所有主效应都和某个 2fi 别名**，解释要非常小心。实际操作：只作"筛选 (screening)"用，筛出几个可能活跃的主效应，再加更多 runs 分离 2fi。

**一般原则：** 分辨度 $R \le K - p + 1$，而 $R$ 最大化与运行数最小化**相互权衡**。

---

# 第十部分：常见陷阱与考场速查

## Trap 1：把 $2^{K-p}$ 当成 $2^{K-p}$ 全析因时，系数是 $1/2^{K'-1}$ 还是 $1/2^{K-1}$？

**答案：用 $K' = K - p$**，即分数运行数对应的 "effective $K$"。除数是 $2^{K'-1}$。

**例：** $2^{4-1}$ 时 $K' = 3$，除数 $= 4$，**不是 $2^{4-1} = 8$**。

## Trap 2：别名代数用错

**规则：** $X^2 = I$（任何因子字母的平方都是 "恒等项" $I$）。**只对字母本身**成立，**不是**对数值 $X$ 的平方。

**例：** $A \cdot AB = A^2 B = I \cdot B = B$（正确）。
**错：** $A \cdot AB = A \cdot A \cdot B = AB$（把 $A^2$ 当成 $A$，错）。

## Trap 3：混淆"generator"和"defining relation"

| 术语 | 形式 | 例子（$2^{4-1}$）|
|---|---|---|
| Generator 生成元 | 等式赋值派生因子 | $D = ABC$ |
| Defining relation | 带 $I$ 的等式 | $I = ABCD$ |

**两者等价**，通过两边乘 $D$ 互相转换；考场上两种都可能被问。

## Trap 4：别名符号 $\equiv$ vs 等号

**$A \equiv BCD$ 意思是："$A$ 和 $BCD$ 的对比向量完全相等"** → 我们只能估计 $\tau_A + \tau_{BCD}$。

**$A = BCD$ 是**对 contrast vector 的等式（$g_A = g_{BCD}$），**不**意味着真实效应 $\tau_A = \tau_{BCD}$。

## 考场步骤速查

> 看到 **"fractional factorial / half-fraction / $2^{K-p}$"**, 按以下顺序：

1. **写生成元** → **推出定义关系** $I = ...$
2. **画设计矩阵**：前 $K - p$ 列按字典序，最后 $p$ 列用生成元逐行计算
3. **快检**：每行乘出 $I$ 等式右侧（如 $ABCD$）应该 $=+1$
4. **算分辨度** = 定义关系最短词长
5. **写别名表**：两边乘任何项，用 $X^2 = I$ 化简
6. **计算 $\hat\tau_F$**：$\frac{1}{2^{K'-1}} \mathbf{\bar Y}^\top \mathbf{g}_F$（当成 $2^{K'}$ 全析因）
7. **解读**："$\hat\tau_A$ 估的是 $\tau_A + \tau_{\text{alias}}$；假设高阶 $\approx 0$，则……"
8. **方差**（若要）：$\frac{2^{K'}}{N} \hat S^2$，CI $= \hat\tau \pm 2\cdot SE$

---

# 📋 核心公式速查表

| 公式 | 含义 |
|---|---|
| $2^{K-p}$ runs | 运行数（$p$ = 派生因子数）|
| $N_w = N / 2^{K-p}$ | 每组样品数 |
| Generator $D = ABC$ | 派生 $D$ 的规则 |
| Defining relation $I = ABCD$ | 乘 $D$ 两边得 |
| Resolution $R$ = min word length | 定义关系最短词长 |
| $X \cdot I_{\text{relation}}$ | 求 $X$ 的别名 |
| $K' = K - p$ | 计算时的 "effective $K$" |
| $\hat\tau_F = \frac{1}{2^{K'-1}} \mathbf{\bar Y}^{\top} \mathbf{g}_F$ | 效应估计（当 $2^{K'}$ 全析因算）|
| $\widehat{\text{Var}}(\hat\tau_F) = \frac{2^{K'}}{N} \hat S^2$ | 方差 |
| $\#\text{ runs in half-fraction of } 2^4$ | $= 2^{4-1} = 8$ |

---

# 🎯 关键概念检查清单

1. **为什么需要部分析因？** → 运行代价高；效应稀疏原理说高阶交互通常 ≈ 0，估计它们浪费
2. **什么是 generator？** → 一个把派生因子赋为现有因子乘积的等式，如 $D = ABC$
3. **什么是 defining relation？** → 由 generator 两边乘派生字母得到的 $I = \cdots$ 形式
4. **Resolution 的定义？** → 定义关系里最短词的长度
5. **Res III/IV/V 实际意义？** → III: 主 vs 2fi 混淆（差）；IV: 主干净，2fi 混；V: 主 & 2fi 都干净
6. **$2^{4-1}$ 用哪个生成元最好？为什么？** → $D = ABC \Rightarrow I = ABCD$（Res IV）；其它三种都是 Res III
7. **$A \equiv BCD$ 什么意思？** → 两者对比向量相等；$\hat\tau_A$ 实际估计 $\tau_A + \tau_{BCD}$
8. **为什么要假设 3fi 可忽略？** → 效应稀疏原理 + 无法分离 → 不假设就无法解读
9. **$\hat\tau_F$ 公式里的 $K$ 是原始 $K$ 还是 $K-p$？** → 用 $K' = K - p$（有效 $K$）
10. **什么是 fold-over？** → 加跑另一半（$D = -ABC$），合并成全析因，完全分离别名
11. **如何在 $n = 1$ 时估方差？** → 用"假设可忽略"的高阶效应估 $\sigma^2$，或加中心点重复
12. **$I = ABD$ 和 $I = ABCD$ 哪个更好，为什么？** → $ABCD$ (Res IV) > $ABD$ (Res III)：选最长的词

---

# 🧪 附录：回答 mock_exam_v2 Problem 7 的模板化步骤

**如果考场遇到 "$2^{4-1}$ with generator $D=ABC$"：**

1. **第 1 分钟**：写下 $I = ABCD$
2. **第 3 分钟**：画设计矩阵（$A,B,C$ 字典序 + $D$ 由乘积得）
3. **第 5 分钟**：写别名对 8 组（$I,A,B,C,D,AB,AC,AD$ 各配一个 3fi/4fi）
4. **第 6 分钟**：宣布 Resolution IV
5. **第 10 分钟**：若题目给 $Y^{obs}$，计算 $\hat\tau_A = \frac{1}{4} \mathbf{g}_A^\top \mathbf{\bar Y}$
6. **第 12 分钟**：解读 "严格估计 $\tau_A + \tau_{BCD}$；假设 3fi 可忽略，$\hat\tau_A \approx \tau_A$"
7. **第 14 分钟**：如果要 CI，$\hat\tau \pm 2\cdot\sqrt{(2^{K'}/N)\hat S^2}$

**完美开挂 🚀**

---

# 📚 相关阅读

- Box, Hunter & Hunter (2005). *Statistics for Experimenters* (Ch 6-7) — 半分式的经典来源
- Montgomery (2019). *Design and Analysis of Experiments* (Ch 8) — 别名矩阵、resolution 的权威讲法
- Wu & Hamada (2009). *Experiments: Planning, Analysis, and Optimization* — 更严谨的 $2^{K-p}$ 系列
- Dasgupta, Pillai & Rubin (2015). "Causal inference from $2^K$ factorial designs." *JRSSB* — 全析因潜在结果框架（你的 Module 13 基础）
