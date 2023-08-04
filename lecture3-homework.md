# 第3课 课后作业

## 第1题 二次非剩余

让我们逐步验证这个协议的完备性和可靠性。

(a) **完备性**：

假设 $QR(m, x)=0$，即$x$在模$m$下是一个二次非剩余。按照协议进行：

1. Verifier 选择一个与 $m$ 互质的元素 $s \in \mathbb{Z}_{m}$ 并抛硬币得到 $b \gets_R \{0,1\}$。
2. Verifier 计算 $y$ 如下：
   $$
   y \leftarrow \begin{cases}
   s^{2} x & \text{ if } b=0, \\
   s^{2} & \text{ if } b=1.
   \end{cases}
   $$

   因为 $x$ 在模 $m$ 下是一个二次非剩余，所以 $y$ 是一个二次非剩余。

3. Prover 计算 $QR(m, y)$ 并将其值发送回 Verifier。

由于 $y$ 是二次非剩余，$QR(m, y)=0$，所以 Prover 将发送给 Verifier 的值必定等于 $b$。

4. Verifier 收到 Prover 发送的值，它与之前生成的 $b$ 相等，所以 Verifier 接受。

因此，如果 $QR(m, x)=0$ 并且双方都按照协议行事，那么验证者总是接受。

(b) **可靠性**：

假设 $QR(m, x)=1$，即$x$在模$m$下是一个二次剩余。这意味着存在一个整数 $r$，使得 $x \equiv r^2 \ (\text{mod} \ m)$。

Verifier 在协议的第一步中选择了 $s$ 和 $b$，Prover 不知道这些值。现在我们来分析 Prover 可能做的两种情况：

**情况1：Prover 遵循协议**：

1. Verifier 选择一个与 $m$ 互质的元素 $s \in \mathbb{Z}_{m}$ 并抛硬币得到 $b \gets_R \{0,1\}$。
2. Verifier 计算 $y$ 如下：
   $$
   y \leftarrow \begin{cases}
   s^{2} x & \text{ if } b=0, \\
   s^{2} & \text{ if } b=1.
   \end{cases}
   $$

3. Prover 计算 $QR(m, y)$ 并将其值发送回 Verifier。

   如果 Prover 遵循协议，那么它会计算 $QR(m, y)$，其中 $y=s^2x$ 或 $y=s^2$。

   - 如果 $y=s^2x$，那么由于 $x \equiv r^2 \ (\text{mod} \ m)$，我们有 $y \equiv s^2 r^2 \ (\text{mod} \ m)$。因为 $x$ 是二次剩余，所以 $r^2$ 也是二次剩余，进而 $s^2 r^2$ 也是二次剩余。因此，$QR(m, y)=1$。
   - 如果 $y=s^2$，由于 $x$ 是二次剩余，$y=s^2$ 也是二次剩余，因此 $QR(m, y)=1$。

   无论 Prover 计算哪种情况，它发送给 Verifier 的值必定等于 $b$，所以 Verifier 接受。

**情况2：Prover 不遵循协议**：

在这种情况下，Prover可能发送任意值给 Verifier，不一定是 $QR(m, y)$ 的正确计算结果。

1. Verifier 选择一个与 $m$ 互质的元素 $s \in \mathbb{Z}_{m}$ 并抛硬币得到 $b \gets_R \{0,1\}$。
2. Verifier 计算 $y$ 如下：
   $$
   y \leftarrow \begin{cases}
   s^{2} x & \text{ if } b=0, \\
   s^{2} & \text{ if } b=1.
   \end{cases}
   $$

3. Prover 不遵循协议，发送任意值给 Verifier。

由于 Prover 不遵循协议，我们无法保证它发送的值与 $b$ 相等。然而，在这种情况下，Verifier 会拒绝，因为 Prover 发送的值与 Verifier 期望的值不匹配。

综上所述，无论 Prover 是否遵循协议，Verifier 都会以 $\geq 1/2$ 的概率拒绝。

因此，该协议具有完备性和可靠性。


## 第2题 二次剩余
