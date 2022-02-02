(page_topic1)=
Errors in Numerical Differentiation
=======================

In order for the finite difference formulas derived in the previous section to be useful, we need to have some idea of the errors involved in using these formulas.  As mentioned, we are replacing $f(x)$ with its polynomial interpolant $p_n(x)$ and then taking the derivatives, so that 

$$ f'(x) = p_n'(x) + \epsilon'(x),\qquad f''(x) = p_n''(x) + \epsilon''(x).$$

We need to $\epsilon'(x)$ and \epsilon''(x) to determine the errors in the formulas we derived.

First recall [the polynomial interpolation error](../InterpFit/InterpErrors) is

$$\begin{equation} \epsilon(x)= f(x)-p_n(x) = \frac{f^{n+1}(\xi)}{(n+1)!}\prod_{j=0}^n (x-x_j). \end{equation}$$ (inter_err)

Taking the derivative of this using the product rule yields

$$
\begin{equation} \epsilon'(x) = \frac{f^{n+1}(\xi)}{(n+1)!}\left[\sum_{k=0}^n \prod_{j=0,j\neq k}^n (x-x_j)\right] + \frac{f^{n+2}(\xi)\xi'}{(n+1)!}\prod_{j=0}^n (x-x_j), \end{equation}$$ (errprime)

where the second term arises from the fact that $\xi$ may depend on $x$.  Evaluating this at one of the gridpoints, say $x_i$, simplifies the expression considerably as all but one of the terms contain the factor $(x_i-x_i)$,

$$
\begin{equation} \epsilon'(x_i) = \frac{f^{n+1}(\xi)}{(n+1)!}\prod_{j=0,j\neq i}^n (x_i-x_j). \end{equation}$$

We can now use this expression for the errors in our first derivative finite difference formulas.  Before we do, let's derive the error formula for the second derivatives.


Taking the derivative {eq}`errprime` yields

$$
\begin{align} \epsilon''(x) = & \frac{f^{n+1}(\xi)}{(n+1)!}\left[ \sum_{k=0}^n \sum_{m=0, m\neq k}^n \prod_{j=0,j\neq k,m}^n (x-x_j)\right]\\
& + \frac{f^{n+2}(\xi)\xi''+f^{n+3}(\xi)\xi'}{(n+1)!}\prod_{j=0}^n (x-x_j) \\
&+ \frac{f^{n+2}(\xi)\xi'}{(n+1)!}\left[\sum_{k=0}^n \prod_{j=0,j\neq k}^n (x-x_j)\right],\end{align}$$

Evaluating this at one of the gridpoints, say $x_i$, simplifies the expression to,

$$
\begin{align}
\epsilon''(x_i) &=\frac{f^{n+1}(\xi)}{(n+1)!}\left[\sum_{m=0,m\neq i}^n \prod_{j=0, j\neq i,m} (x_i-x_j) + \sum_{k=0,k\neq i}^n \prod_{j=0, j\neq i,k} (x_i-x_j) \right] +\frac{f^{n+2}(\xi)\xi'}{(n+1)!}\prod_{j=0,j\neq i}^n (x_i-x_j),\\
&=\frac{2f^{n+1}(\xi)}{(n+1)!}\left[\sum_{k=0,k\neq i}^n \prod_{j=0, j\neq i,k} (x_i-x_j) \right] +\frac{f^{n+2}(\xi)\xi'}{(n+1)!}\prod_{j=0,j\neq i}^n (x_i-x_j).
\end{align}$$






