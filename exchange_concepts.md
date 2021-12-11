- [Liquidity Pool](#liquidity-pool)
  - [Automated Market Maker (AMM)](#automated-market-maker-amm)
  - [Price](#price)
    - [Real-time Price](#real-time-price)
    - [Avg Price (No Transaction Fees)](#avg-price-no-transaction-fees)
    - [Avg Price (With Transaction Fees)](#avg-price-with-transaction-fees)
  - [Add and Remove Liquidity (Uniswap v1, v2 Protocol)](#add-and-remove-liquidity-uniswap-v1-v2-protocol)
    - [Add Liquidity](#add-liquidity)
    - [Remove Liquidity](#remove-liquidity)
  - [Impermanent Loss](#impermanent-loss)
    - [No Transaction Fees](#no-transaction-fees)
    - [With Transaction Fees](#with-transaction-fees)
# Liquidity Pool


## Automated Market Maker (AMM)
Uniswap uses the formula $x*y=k$. Where $x$ is the amount of one token in the liquidity pool and $y$ is the amount of the other. $k$ is a fixed constant.

## Price

### Real-time Price
How to define the price of asset X with in asset Y? In the Uniswap Math model. Intuitively, we define it as $\frac{y}{x}$ where $y$ is the amount of asset $Y$ and $x$ is the amount of asset $X$. 

More accurately, to define the real-time price of asset $X$ in asset $Y$. We can use the mechanism of Uniswap AMM Model.

$$
\begin{aligned}
\lim_{\Delta x \rightarrow 0} (x+\Delta x)(y-\Delta y) &= k\\
\end{aligned}
$$

And we are looking for $\lim_{\Delta x \rightarrow 0} \frac{\Delta y}{\Delta x}$. Isn't this just the negative value of derivative? (Notice $y-\Delta y$ instead of $y+\Delta y$ in the expression). By $x\cdot y=k$ it's easy to calculate $y = \frac{k}{x}$ and $\frac{dy}{dx} = -\frac{k}{x^2} = -\frac{y}{x}$. We can validate it by following.

$$
\begin{aligned}
y - \Delta y &= \lim_{\Delta x \rightarrow 0} \frac{k}{x + \Delta x} \\
\Delta y &= \lim_{\Delta x \rightarrow 0} y - \frac{k}{x + \Delta x} \\
\Delta y &= \lim_{\Delta x \rightarrow 0} \frac{k}{x} - \frac{k}{x + \Delta x} \\
\Delta y &= \lim_{\Delta x \rightarrow 0} k\frac{\Delta x}{x^2 + x\Delta x} \\
\end{aligned}
$$

Therefore, we would get
$$
\begin{aligned}
\lim_{\Delta x \rightarrow 0} \frac{\Delta y}{\Delta x} &= \lim_{\Delta x \rightarrow 0} \frac{k}{x^2 + x\Delta x} \\
&= \frac{k}{x^2} \\
&= \frac{y}{x} \\
&= -\frac{dy}{dx} \\
\end{aligned}
$$

Another direction where $x$ decreases and $y$ increases will be omitted, indeed, it's really just the negative + the derivative of $y$ if you think about it.

### Avg Price (No Transaction Fees)
Suppose we exchange $\Delta x$ token to $\Delta y$ tokens by using the liquidity pool or vice versa. Then the average price we define as $\frac{\Delta y}{\Delta x}$. And this actually corresponds to the following calculation. (Assuming $\Delta x$ is positive, and we only consider x increase case, the other way around is similar)

$$
\begin{aligned}
P_{avg} &= \frac{\int_{x}^{x+\Delta x} P_x}{\Delta x} \\
\int_{x}^{x+\Delta x} P_x &= \int_{x}^{x+\Delta x} \frac{k}{x^2} \\
&= -\frac{k}{x} |_{x}^{x+\Delta x} \\
&= \frac{k}{x} - \frac{k}{x + \Delta x}
&= \Delta y \\
P_{avg} = \frac{\Delta y}{\Delta x}
\end{aligned}
$$

Another view of $\Delta x$ and $\Delta y$. Consider the following. Define $\alpha = \frac{\Delta x}{x}$ and $\beta = \frac{\Delta y}{y}$. Then

$$
\begin{aligned}
x' = x + \Delta x = (1 + \alpha)x = \frac{k}{y - \Delta y} = \frac{\frac{k}{y}}{1 - \frac{\Delta y}{y}} &= \frac{1}{1-\beta} x \\
y' = y - \Delta y = (1 - \beta)y &= \frac{1}{1 + \alpha} y 
\end{aligned}
$$

Therefore,

$$
\Delta x = \frac{\beta}{1-\beta}x\\
\Delta y = \frac{\alpha}{1+\alpha}y
$$

### Avg Price (With Transaction Fees)
With transaction fee, many things should change. Basically, consider the transaction fee is $\rho$ where $0 \leq \rho < 1$. Also let's define $\gamma = 1 - \rho$. Then we Uniswap AMM requires the following equation:

$$
(x + (1 - \rho)\Delta x)(y - \Delta y) = xy = k
$$

We have 

$$
\begin{aligned}
y - \Delta y &= \frac{k}{x + (1- \rho) \Delta x} \\
&= \frac{\frac{k}{x}}{1 + (1-\rho) \frac{\Delta x}{x}} \\
&= \frac{1}{1 + (1-\rho)\alpha}y \\
&= \frac{1}{1 + \gamma\alpha}y \\
\end{aligned}
$$

By using $y - \Delta y = (1-\beta)y$ we could get $\frac{1}{1+\gamma\alpha} = 1-\beta$. And we can get $\alpha = \frac{\beta}{(1-\beta)\gamma}$

Therefore, for $x + \Delta x$, we could have

$$
\begin{aligned}
x + \Delta x &= (1 + \alpha)x \\
&= (1 + \frac{\beta}{(1-\beta)\gamma})x \\
&= (1 + \frac{\frac{\beta}{\gamma}}{(1-\beta)})x \\
&= \frac{1-\beta + \frac{\beta}{\gamma}}{1-\beta}x \\
&= \frac{1 + \beta(\frac{1}{\gamma}-1)}{1-\beta}x
\end{aligned}
$$

In conclusion, we get the following equations:

$$
x'_\rho = x + \Delta x = (1 + \alpha)x = \frac{1 + \beta(\frac{1}{\gamma}-1)}{1-\beta}x \\
y'_\rho = y - \Delta y = (1 - \beta)y = \frac{1}{1 + \gamma\alpha}y
$$

And

$$
\begin{aligned}
\Delta x &= \frac{\beta}{1-\beta}\frac{1}{\gamma}x \\
\Delta y &= \frac{\alpha\gamma}{\alpha\gamma + 1}y
\end{aligned}
$$

**Therefore, we can use the above formula to calculate the corresponding exchange amount with transaction fee. (i.e. Putting $\Delta x$ amount of asset $X$ should exchange how much asset $Y$ or to exchange $\Delta y$ amount of asset $Y$, how much asset $X$ we need)**

Notice the result consists with no tranaction fees if $\rho = 0 \Leftrightarrow \gamma = 1$

Also, please notice that $x_\rho y_\rho > xy$ if $\rho > 0$ according to $(x+(1-\rho)\Delta x)(y-\Delta y)=xy=k$

## Add and Remove Liquidity (Uniswap v1, v2 Protocol)
Assuming we have a pool of pair Eth with amount $e$ and Token with amount $t$. Then Adding and removing liquidity require not changing the ratio $e:t:l$ where $l$ is the liquidity such that $l^2 = k$.
### Add Liquidity
Suppose we want to provide liquidity of $\Delta e > 0$ amount of Eth. Then how much Token $\Delta t$ we also need to provide and how many liquidity token pairs $\Delta l$ we would get?

If we do the following such that, let's define $\alpha = \frac{\Delta e}{e}$. Intuitively, we want $P_t^{after} = P_t^{before}$. Therefore, $\frac{e+\Delta e}{t+\Delta t} = \frac{e}{t} \Rightarrow t + \Delta t = (1+\alpha)e\cdot \frac{t}{e}=(1+\alpha)t$. Then $l' = l + \Delta l$ can be calculated using $e'$ and $t'$ 

$$
\begin{aligned}
e' &= (1 + \alpha)e \\
t' &= (1 + \alpha)t \\
l' &= (1 + \alpha)l
\end{aligned}
$$

We can show that $e':t':l' = e:t:l$ and this should be straightforward. Therefore, $\Delta t = \alpha t, \Delta l = \alpha l$.

### Remove Liquidity
Similar methods as adding liquidity, removing liquidity usually means burning $\Delta l$ liquidity tokens. Denote $\alpha = \frac{\Delta l}{l}$. We can get

$$
\begin{aligned}
e' &= (1 - \alpha)e \\
t' &= (1 - \alpha)t \\
l' &= (1 - \alpha)l 
\end{aligned}
$$

Therefore we can see this is a very good practice.

## Impermanent Loss
What is impermanent loss? Basically, in theory, it is the loss that we provide the liquidity instead of holding the assets. Usually, when two assets relative price changes a lot, no matter increase or decrease, we will both face impermanent loss. 

The following calculations shows the theory of impermanent loss. **Notice: The calculation doesn't consider the increment of k and it is also a theory-based calculations.**

Define $L$ be the liquidity such that $L^2 = K$. Then $x\cdot y = L^2$. Let's define $P$ be the relative price of asset $X$ in terms of asset $Y$. Then we would have $P = \frac{y}{x}$. We would have $y = Px$. Therefore, we have $x = \frac{L}{\sqrt{P}}$ and $y = L\sqrt{P}$.

Let $V$ be the total value of asset $X$ and asset $Y$ in the relative price in terms of asset $Y$. Then $V = P\cdot x + Y$. Therefore, suppose $P' = cP$. Let $IL(c)$ denote the impermenant loss ratio depending on variable $c$. Then 

### No Transaction Fees

$$
\begin{aligned}
IL(c) = \frac{V-V_{held}}{V_{held}} &= \frac{P'x_2 + y_2 - (P'x_1 + y_1)}{P'x_1 + y_1} \\
&= \frac{cP\frac{L}{\sqrt{cP}}+L\sqrt{cP}-cP\frac{L}{\sqrt{P}}-L\sqrt{P}}{cP\frac{L}{\sqrt{P}}+L\sqrt{P}} \\
&= \frac{2L\sqrt{cP}}{(c+1)L\sqrt{P}} - 1\\
&= \frac{2\sqrt{c}}{c+1} - 1
\end{aligned}
$$

If you check the plot of this function, $IL(c)$ is always below $0$. Which implies if the transaction fee $\rho = 0$, the liquidity provider is always losing money.

### With Transaction Fees

Let $\rho$ be the transaction fees. In this case, we have $(x_1 + (1 - \rho)\Delta x)(y_1 - \Delta y) = k$. 

$$
\begin{aligned}
(x_1 + \Delta x - \rho \Delta x)y_2 &= k\\
(x_2 - \rho(x_2 - x_1))y_2 = k
\end{aligned}
$$