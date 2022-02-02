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
\begin{equation} \epsilon'(x_i) = \frac{f^{n+1}(\xi)}{(n+1)!}\prod_{j=0,j\neq i}^n (x_i-x_j). \end{equation}$$ (pprimeerr)

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
\end{align}$$ (ppperr)

This is a bit messier than the error for the first derivative {eq}`pprimeerr`.  In most cases one can argue that the second term is small compared to the first as it contains one additional factor of the difference of grid points and one expects $\xi'$ to be small.

**Example: Error in the two point formulas**  In the previous section, for two points $x_0$ and $x_1=x_0+h$ we derived the forward/backwards difference formulae and we would now like to know the error in using these formulae.  Using {eq}`pprimeerr` with $n=1$ we straightforwardly get

$$ \epsilon'(x_0) = \frac{f''(\xi)}{2}(-h),\quad \text{and}\quad  \epsilon'(x_1) = \frac{f''(\xi)}{2}(h),$$

noting that $\xi$ is not likely to be the same number in the two cases.  So, in summary, we now have

$$
\begin{equation}
\left[{\begin{array}{c}
f'(x_0) \\
f'(x_1) \\
\end{array}} \right] =
\left[{\begin{array}{c}
\frac{f(x_1)-f(x_0)}{h} \\
\frac{f(x_1)-f(x_0)}{h} \\
\end{array}} \right] +
\left[{\begin{array}{c}
  -\frac{h}{2}f''(\xi) \\
   \frac{h}{2}f''(\xi) \\
\end{array}} \right].
\end{equation}
$$ (twopoint)

**Example: Error in the three point formulae**  In the previous section, for three points $x_0,\,x_1=x_0+h,$ and $x_2=x_0+2h,$ $x_0$ we derived finite difference formulae for first and second derivatives and we would now like to know the error in using these formulae.  Using {eq}`pprimeerr` with $n=2$ we straightforwardly get the result for the first derivatives:

$$
\begin{equation}
\left[{\begin{array}{c}
f'(x_0) \\
f'(x_1) \\
f'(x_2)
\end{array}} \right] =
\left[{\begin{array}{c}
\frac{-3f(x_0)+4f(x_1)-f(x_2)}{2h} \\
\frac{f(x_2)-f(x_0)}{2h} \\
\frac{3f(x_2)-4f(x_1)+f(x_0)}{2h}
\end{array}} \right] +
\left[{\begin{array}{c}
  \frac{h^2}{3}f(3)(\xi) \\
  -\frac{h^2}{6}f(3)(\xi) \\\
  \frac{h^2}{3}f(3)(\xi) \\
\end{array}} \right].
\end{equation}
$$

First note that the errors here are $\mathcal{O}(h^2)$ compared to {eq}`twopoint` for the two-point formulae which are $\mathcal{O}(h)$.  So, assuming $h$ is small these do indeed have a higher order accuracy.  Further, the error in the *central* difference formula is *half* that of the backward/forward differences.  As a result, the central difference formula is the one most widely used.

We now turn to the second derivative errors.  At $x_0$ {eq}`ppperr` gives

$$
\begin{align}
\epsilon''(x_0) = 2\frac{f^{(3)}(\xi)}{3!}\left[(x_0-x_1)+(x_0-x_2)\right]+\frac{f^{(4)}(\xi)\xi'}{3!}(x_0-x_1)(x_0-x_2)\\
&= -f^{(3)}(\xi) h + \frac{f^{(4)}(\xi)\xi'}{3}h^2.
\end{align}
$$

At $x_1$ we get

$$
\begin{align}
\epsilon''(x_1) = 2\frac{f^{(3)}(\xi)}{3!}\left[(x_1-x_0)+(x_1-x_2)\right]+\frac{f^{(4)}(\xi)\xi'}{3!}(x_1-x_0)(x_1-x_2)\\
&= -\frac{f^{(4)}(\xi)\xi'}{6} h^2.
\end{align}
$$

These, together wit the result at $x_2$ gives

$$
\begin{equation}
\left[{\begin{array}{c}
f''(x_0) \\
f''(x_1) \\
f''(x_2)
\end{array}} \right] =
\left[{\begin{array}{c}
\frac{f(x_0)-2f(x_1)+f(x_2)}{h^2} \\
\frac{f(x_0)-2f(x_1)+f(x_2)}{h^2} \\
\frac{f(x_0)-2f(x_1)+f(x_2)}{h^2}
\end{array}} \right] +
\left[{\begin{array}{c}
  -h f(3)(\xi) \\
  -\frac{h^2}{6}f(4)(\xi)\xi' \\\
  h f(3)(\xi) \\
\end{array}} \right].
\end{equation}
$$

Note that even through the approximation formula is the same for all 3 points, the error in this formula is much worse at the endpoints ($\mathcal{O}(h)$ versus $\mathcal{O}(h^2)$).  In homework you will see that you can also derive the central difference formula using a Taylor series expansion (including errors) about $x_1$ and then using the mean value theorem to combine the errors at the points $x_1\pm h$ at a new, also unknown, point $\eta$ to get

$$ f''(x_1) = \frac{f(x_0)-2f(x_1)+f(x_2)}{h^2} -\frac{h^2}{12}f(4)(\eta). $$







