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

### Mean-variance analysis



### Limits of diversification

As we have seen, uncorrelated assets allows us to reduce the overall volatility in a portfolio. The ups and downs are less volatile. However, there is only so much benefit you can achieve by adding more assets to a portfolio. Think of it as the "Laws of Diminishing Returns." We can see, as the number of assets in the portfolio increase, the portfolio variance decreases.

![Portfolio Risk](/post/images/Diversification/figure1.jpg)

However, notice the benefits of diversification are constrained not only by the portfolio variance, but what we call "systematic risk". In essence, investors aren't compensated for taking what we call "unsystematic risk," that is, risk that can be diversified away. Recall that the Equity Risk Premium is denoted by $E[r]=r_\alpha-r_f=\beta_\alpha(r_m-r_f)+\epsilon$.

Looking at the derivation of the Capital Asset Pricing Model (CAPM), we should receive a premium for investing in equities over debt, particularly the risk-free rate ($r_\alpha-r_f$). This makes sense, as without this premium, there would be nothing to entice owners of capital to invest in equities over treasuries. Also, investors should expect a premium for the systemic risk of the overall market $\big(\beta*(r_m-r_f)\big)$. Since equities are significantly more risky compared to treasuries, and given the universe of stocks have $\beta>1$ (meaning, most stocks are positively correlated), it is nearly impossible to diversify away systemic risk.
