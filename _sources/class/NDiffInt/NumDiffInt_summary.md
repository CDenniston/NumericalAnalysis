(page_topic1)=
Numerical Differentiation
=======================

Suppose we can either evaluate $f(x)$ for any $x$, or are provided with $f(x)$ as some set of points $\{x_i\}_{i=0}^n$, but do not know $f'(x)$.  How do we compute an estimate of $f'(x)$, or $f''(x)$(or higher derivatives, though we will restrict ourselves to the first two derivatives here)?  As always, it is useful to make use of something we already know how to do, namely interpolation.  We replace $f(x)$ with its interpolatory function (Lagrange or splines) and take the derivative of that.  i.e.

$$ f(x) = p(x) + \epsilon(x), $$

where $p(x)$ is our interpolation and $\epsilon(x)$ is the error in the interpolation.  Taking the derivative gives

$$ f'(x) = p'(x) + \epsilon'(x),\qquad f''(x) = p''(x) + \epsilon''(x)$$

Our estimate for the derivative $f'(x) \approx p'(x)$ and $\epsilon'(x)$ is the error in this estimate.  Similarly $f''(x) \approx p''(x)$ and $\epsilon''(x)$ is the error in that estimate. 

In the Lagrange form for the interpolating polynomial, writen in [barycentric form](../InterpFit/BarycentricInterp)  

$$ p_n(x) = \sum_{i=0}^n L_i(x) f(x_i) = \frac{\sum_{i=0}^n \frac{w_i}{x-x_i} f(x_i)}{\sum_{i=0}^n \frac{w_i}{x-x_i}}. $$ (pnformula)

To simplify the notation, we have dropped the $n$ from $L_i^n(x)$ as the appropriate $n$ should be clear from most contexts.  We can see from {eq}`pnformula` that 

$$ p_n'(x) =  \sum_{i=0}^n L_i'(x) f(x_i), \qquad p_n''(x) =  \sum_{i=0}^n L_i''(x) f(x_i)$$

and

$$ L_i(x) =  \frac{\frac{w_i}{x-x_i}}{\sum_{i=0}^n \frac{w_i}{x-x_i}}.$$ (Lieqn)

So to work out the derivative estimates we need to compute $L_i'(x)$ and $L_i''(x)$ for all $i$ (note that these still depend on $n$, but $n$ is the same for all $L_i$ in a given application).

In almost all cases, we are looking for the derivative at a node point, say $x_j$.  To ensure various terms remain differentiable, it is helpful to multiply both sides by $x-x_j$ and rearrange Eq. (Lieqn) to get

$$ L_i(x)\sum_{i=0}^n w_i \frac{x-x_j}{x-x_i} = w_i\frac{x-x_j}{x-x_i},  $$(Lisetup)

where both sides are now differentiable at $x_j$.  Defining

$$q_i(x)\equiv \sum_{i=0}^n w_i \frac{x-x_j}{x-x_i},$$

we can rewrite {eq}`Lisetup` as

$$ L_i(x)q_i(x) = w_i\frac{x-x_j}{x-x_i},  $$

then take the first and second derivative to get

$$
\begin{align}
L_i'(x)q_i(x)+L_i(x)q_i'(x)&=w_i \left[\frac{1}{x-x_i}-\frac{x-x_j}{(x-x_i)^2}\right],\\
L_i''(x)q_i(x)+2L_i'(x)q_i'(x)+L_i(x)q_i''(x) &= w_i \left[-2\frac{1}{(x-x_i)^2}+2\frac{x-x_j}{(x-x_i)^3}\right].
\end{align}
$$

From the definition of $q_i(x)$ one can also work out

$$
\begin{align}
q_i'(x) &=  \sum_{i\neq j}^n w_i \frac{1}{x-x_i}-\sum_{i\neq j}^n w_i \frac{x-x_j}{(x-x_i)^2},\\
q_i''(x) &= -2\sum_{i\neq j}^n w_i \frac{1}{(x-x_i)^2} + 2\sum_{i\neq j}^n w_i \frac{x-x_j}{(x-x_i)^3}.
\end{align}
$$

Evaluating these at $x=x_j$ we see that

$$
\begin{align}
q_i'(x_j) = \sum_{i\neq j}^n w_i \frac{1}{x_j-x_i},\\
q_i''(x_j) &= -2\sum_{i\neq j}^n w_i \frac{1}{(x_j-x_i)^2}.
\end{align}
$$

