---
title: "Does Diversification Always Matter?"
date: "2024-04-29T00:00:00Z"
authors:
  - admin

categories:
  - Finance
  - Programming

tags:
  - Quant Finance

summary: They say "never put your eggs in one basket" when it comes to investing. This intutition is grounded in the mathematics of modern portfolio theory.

featured: true
reading_time: true
draft: false

markup: mmark

image:
  placement: 2
  preview_only: false

math: true
---

I've previously talked about Modern Portfolio Theory (MPT) and diversification in the past, the assumptions underlying MPT, and the construction of an Portfolio Frontier (PF). When most people think of diversifying their portfolio, they think of limiting their downside risk. However, it is possible construct a portfolio that still maximizes return with the least amount of volatility (choosing a portfolio that lies somewhere along the PF).

Now I'm going to further expand upon why diversification helps to maximize risk-adjusted returns, and express the limits of diversification.

### What Makes a Portfolio?

We can define a _portfolio_ as a combination of $N$ assets with $N$ portfolio weights that sum to unity:

$$\textbf{w}=[w_1,\ldots,w_N],\quad\sum^{N}_{n=1}w_n=1.$$

The weight, $w_N$, represents the proportion of the $n$th asset in the portfolio. If $M_n$ and $P_n$ are the number and price of the $n$th asset, then $w_n$ is simply the total value of the $n$th asset normalized by the value of the portfolio:

$$w_n=\frac{M_nP_n}{M_1P_1+\ldots+M_nP_N}$$

Weights can be negative, such we could *short sell* an asset (profiting from asset price going down). Also, weights could be greater than unity, meaning that we are using leverage. We won't get into these complicated scenarios and use only the basic assumptions in the summary of our portfolio. 

Imagine, for example, that we had an investment account of $\mathrm{\$}10,000$ with $40$ shares of stock $A$ at $\mathrm{\$}150$ per share, $50$ shares of stock $B$ at $\mathrm{\$}20$ per share, and $25$ shares of stock $C$ at $\mathrm{\$}120$ per share. Then our portfolio with weights would be

|  Asset | Shares  | Price per share  | Investments | Weight |
| :----: | :-----: | :---------------:| :----------:|:------:|
| $A$    | 40      | 150              | 6000        | 0.6    |
| $B$    | 50      | 20               | 1000        | 0.1    |
| $C$    | 25      | 120              | 3000        | 0.3    |

However, the weights need not be just the proportion of a given stock or asset. For example, suppose we were trading on margin, with $\mathrm{\$}8,000$ in our account to support our $\mathrm{\$}10,000$ investment. If we withdrew $\mathrm{\$}2,000$ from our investment account, then our portfolio in dollars would be unchanged, but our portfolio weights would have changed:


|  Asset | Shares  | Price per share  | Investments | Weight |
| :----: | :-----: | :---------------:| :----------:|:------:|
| $A$    | 40      | 150              | 6000        | 0.6    |
| $B$    | 50      | 20               | 1000        | 0.1    |
| $C$    | 25      | 120              | 3000        | 0.3    |
| Margin |         |                  | -2000       |        |

The weights change because the normalizer changes from $\mathrm{\$}10,000$ to $\mathrm{\$}8,0000$.

### Defining risk and reward

Now that we have defined what a portfolio is, we can outline what we want to do. We can construct a portfolio that maximizes reward and minimizes risk, where "reward" is defined as overal portfolio return and "risk" is defined as the volatility ($\sigma$ or $\sigma^2$) of that return.

### Diversification with uncorrelated assets

We can derive the mean-variance analysis by calculating the mean (expected return) and the variance on that portfolio. Let $R_n$ denote the return on the $n$th asset in a portfolio. By definition, its mean and variance are

{{< math >}}
$$
\begin{aligned}
\mathbb{E}[\mathbb{R_n}]\overset{\Delta}{=}&\mu_n,\\
\mathbb{V}[R_n]=&\mathbb{E}[(R_n-\mu_n)^2]\overset{\Delta}{=}\sigma^2_{n}.\\
\end{aligned}
$$
{{< /math >}}

Now let $R_p$ denote the return on the entire portfolio, this is the quantity we're interested in. By the linearity of expectation, we have

{{< math >}}
$$
\begin{aligned}
R_p\overset{\Delta}{=}&w_1R_1+\dots+w_NR_N,\\
\Downarrow & \\
\mathbb{E}[R_p]=&\mathbb{E}[w_1R_1+\dots+w_NR_N]\\
=&w_1\mathbb{E}[R_1]+\dots+w_N\mathbb{E}[R_N]\\
=&w_1\mu_1+\dots+w_N\mu_N\\
\overset{\Delta}{=}&\mu_p
\end{aligned}
$$
{{</ math >}}

The first line of this equation is just an accounting identity. It's how we would calculate the return on our portfolio given weights $\textbf{w}$ and returns $R_1,\ldots,R_N$. The variance of our portfolio's return is

{{< math >}}
$$
\begin{aligned}
\mathbb{V}[R_p]&=\mathbb{E}[(R_p-\mu_p)^2]\\
&=\mathbb{E}\Big[\big((w_1R_1+\dots+w_NR_N)-(w_1\mu_1+\dots+w_N\mu_N)\big)^2\Big]\\
&=\mathbb{E}\Big[\big((w_1(R_1-\mu_1)+\dots+w_n(R_N-\mu_N)\big)^2\Big]\\
&\overset{\Delta}{=}\sigma^2_p.
\end{aligned}
$$
{{</ math >}}

If we have $N$ assets in our portfolio, and we square the term in the last line in this equation, we get $N^2$ terms inside this expectation. We can write the variance for a single combination $R_n$ and $R_m$ as:

{{< math >}}
$$
\begin{aligned}
\mathbb{E}[w_nw_m(R_n-\mu_n)(R_m-\mu_m)]&=w_nw_m\mathbb{E}[(R_n-\mu)(R_m-\mu)]\\
&=w_nw_m\text{Cov}[R_n,R_m]\\
&=w_nw_m\sigma_{nm}\\
&=w_nw_m\sigma_n\sigma_m\rho_{nm},
\end{aligned}
$$
{{</ math >}}

where $\sigma_{nm}$ and $\rho_{nm}$ are the covariance and correlation between the $n$th and $m$th assets respectively. This provides some basic definitions from probability. Recard that

$$
\rho_{mn}=\frac{\text{Cov}[R_n, R_m]}{\sigma_n\sigma_m}.
$$

Now here's the main point: The variance of our portfolio is a function of the covariances between the assets in the portfolio. We can represent this compactly using a covariance matrix: 

{{< math >}}
$$
\begin{bmatrix}
w_1^2\sigma_1^2 & \dots &w_1w_N\sigma_{1N}\\
\vdots & \ddots & \vdots \\
w_Nw_1\sigma_{N1} & \dots & w^2_N\sigma^2_N\\
\end{bmatrix}=\textbf{w}^T
\begin{bmatrix}
\sigma^2_1 & \dots & \sigma_{1N} \\
\vdots & \ddots & \vdots \\
\sigma_{N1} & \dots & \sigma^2_{N} \\
\end{bmatrix}\textbf{w}
$$
{{</ math >}}

Notice that there are $N$ variance terms (the diagonal of the covariance matrix), while there are $N^2-N$ covariance terms (everything else in the matrix). What this means is that the correlations between assets controls our portfolio's volatility. Positive or negative correlation between asset controls our portfolio's volatility. Positive or negative correlation between assets can increase portfolio volatility, while uncorrelated assets decrease volatility.

So what exactly is "Diversification?" By the Modern Portfolio Theory, diversification is the process of selecting assets that are uncorrelated, and thereby reducing the variance of our portfolio's returns. Failing to be diversified doesn't necessarily mean owning just a small number of assets. We would, in theory, own a large number of assets that are all highly correlated, which would increase the variance in our expected returns (which is not what we want).

### Mean-variance analysis

Now that we have a better understanding of diversification, we can start talking about a mean-variance framework. Let's assume that, all things equal, investors prefer higher expected returns and lower volatility. We assume investors only care about return on their entire portfolio, not on a single asset. In other words, we care about $R_p$, not any individual $R_n$. Given the observed or assumed expected returns and covariance between assets, what portfolios should we prefer?

![risk reward](/post/images/Diversification/figure1.jpg)

Considering the following image. On the $x$-axis is the standard deviation of a portfolio's return $\sigma_p$, and the $y$-axis is the expected return $\mu_p$. This is call the risk-return spectrum.

Considering our assumptions, an investor should prefer portfolio $B$ over $D$, since both have the same volatility but $B$ has higher expected returns. Broadly speaking, investors want to be in the top-left corner: a portfolio with the maximum return possible with the smallest amount of risk.

How do we find the weights that will allow this to happen? Imagine we have a fixed set of assets (considering there are only so many assets we can own). We can estimate returns, variances, and covariances however we'd like, for example, by looking at historical data. Now let $\Sigma$ denote the covariance matrix, and let $\textbf{r}$ be an $N-vector$ of expected returns, i.e. $\textbf{r}\overset{\Delta}{=}[\mu_1,\ldots,\mu_N]$. Then the mean-variance portfolio optimization problem is:

{{< math >}}
$$
\begin{aligned}
\underset{\textbf{w}}{\min}&\quad\textbf{w}^T\Sigma\textbf{w},\\
\text{subject to}&\quad \textbf{w}^T\textbf{r}=K,\\
\text{and}&\quad\sum_nw_n=1,
\end{aligned}
$$
{{</ math >}}

where $K$ is a user specified hyperparameter that controls the desired expected return. In other words, w want to minimize the variance/covariance terms while ensuring our weights normalize to unity and gives us our expected portfolio return $K$ given our estimated expected asset returns $\textbf{r}$.

### The two asset example

Let's consider the special case of two assets, stock $A$ with weight $w_1$ and stock $B$ with weight $w_2$. This will allow us to carefully reason about what is happening. Since $w_1+w_2=1$, we can easily visualize all possible portfolio by sweeping $w_1\in[0,1]$, calculating $w_2\overset{\Delta}{=}1-w_1$, and then computing the $(x,y)$-coordinates in the risk-reward spectrum:

{{< math >}}
$$
\begin{aligned}
&\mathbb{E}[R_p]=w_1\mu_2+w_1\mu_2, \\
&\mathbb{V}[R_p]=w^2_1\sigma^2_1+w^2_2\sigma^2_2+2w_1w_2\sigma_1\sigma_2\rho_{12}
\end{aligned}
$$
{{</ math >}}

Suppose that stock $A$ had an average monthly return of 2 and a standard deviation of 10, while stock $B$ had an average return of $1$ and a standard deviation of $6$. Suppose their correlation if $0.35$. How would a portfolio of two stocks perform? We can construct a table comparing expected portfolio return and volatility for a variety of different weights $\textbf{w}$.

|  $w_1$    | $w_2$    | $\mu_p$   | $\sigma$   | 
| :------:  | :-------:| :-------: | :-------:  |
| $0$       | $1$      | $1.00$    | $6.00$     |
| $0.25$    | $0.75$   | $1.25$    | $5.86$     |
| $0.5$     | $0.5$    | $1.50$    | $6.67$     |
| $0.75$    | $0.25$   | $1.75$    | $8.15$     |
| $1$       | $0$      | $2.00$    | $10.00$    |
| $1.25$    | $-0.25$  | $2.25$    | $12.06$    |

This table shows the potential combination of weights we could have for our two assets. Instead of taking $100\%$ of stock $A$ and $100\%$ of stock $B$, we would take a combination of the two. Portfolio Theory doesn't tell us which combination we should take; that would depend on where you want to be on the risk-reward spectrum. 

We could adopt a portfolio $100\%$ of stock A for the maximum return, which is very risky. We could adopt a portfolio $25\%$ of stock $A$ and $75\%$ of stock $B$ for the least amount of volatility. We could also short stock $B$ to go beyond the maximum return, which will greater increase or risk.

Plotting all the possible portfolios for these two stocks gives us the following image. Obviously, the relationship between risk and reward is nonlinear, as the portfolio is a functional relationship betweeen $\mu_p$ $\sigma_p$. As mentioned before, this is known as an *efficient frontier* or *portfolio frontier*. 

![portfolio frontier](/post/images/Diversification/figure2.jpg)

Now we can see the risk-returns of holding just stock $A$ or just stock $B$. Clearly just holding stock $A$ is less risky than holding just stock $B$. However, notice that if we draw a vertical line straight up from stock $A$, we intersect the curve. This tells us that with a creative selecttion of portfolio weights, we can get the same risk but with a higher expected return.

So it's rational to prefer this point over just stock $A$.

### Limits of diversification

As we have seen, uncorrelated assets allows us to reduce the overall volatility in a portfolio. The ups and downs are less volatile. However, there is only so much benefit you can achieve by adding more assets to a portfolio. Think of it as the "Laws of Diminishing Returns." We can see, as the number of assets in the portfolio increase, the portfolio variance decreases.

![Portfolio Risk](/post/images/Diversification/figure4.jpg)

However, notice the benefits of diversification are constrained not only by the portfolio variance, but what we call "systematic risk". In essence, investors aren't compensated for taking what we call "unsystematic risk," that is, risk that can be diversified away. Recall that the Equity Risk Premium is denoted by $E[r]=r_\alpha-r_f=\beta_\alpha(r_m-r_f)+\epsilon$.

Looking at the derivation of the Capital Asset Pricing Model (CAPM), we should receive a premium for investing in equities over debt, particularly the risk-free rate ($r_\alpha-r_f$). This makes sense, as without this premium, there would be nothing to entice owners of capital to invest in equities over treasuries. Also, investors should expect a premium for the systemic risk of the overall market $\big(\beta*(r_m-r_f)\big)$. Since equities are significantly more risky compared to treasuries, and given the universe of stocks have $\beta>1$ (meaning, most stocks are positively correlated), it is nearly impossible to diversify away systemic risk.
