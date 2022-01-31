(page_topic1)=
Numerical Differentiation
=======================

Suppose we can either evaluate $f(x)$ for any $x$, or are provided with $f(x)$ as some set of points $\{x_i\}_{i=0}^n$, but do not know $f'(x)$.  How do we compute an estimate of $f'(x)$, or $f''(x)$(or higher derivatives, though we will restrict ourselves to the first two derivatives here)?  As always, it is useful to make use of something we already know how to do, namely interpolation.  We replace $f(x)$ with its interpolatory function (Lagrange or splines) and take the derivative of that.  i.e.

$$ f(x) = p(x) + \epsilon(x), $$

where $p(x)$ is our interpolation and $\epsilon(x)$ is the error in the interpolation.  Taking the derivative gives

$$ f'(x) = p'(x) + \epsilon'(x),\qquad f''(x) = p''(x) + \epsilon''(x)$$

Our estimate for the derivative $f'(x) \approx p'(x)$ and $\epsilon'(x)$ is the error in this estimate.  Similarly $f''(x) \approx p''(x)$ and $\epsilon''(x)$ is the error in that estimate. 

In the Lagrange form for the interpolating polynomial, writen in [barycentric form](../InterpFit/BarycentricInterp)  

$$ p_n(x) = \sum_{i=0}^n L_i(x)$ f(x_i) = \frac{\sum_{i=0}^n \frac{w_i}{x-x_i} f(x_i)}{\sum_{i=0}^n \frac{w_i}{x-x_i}}. $$ (pnformula)

To simplify the notation, we have dropped the $n$ from $L_i^n(x)$ as the appropriate $n$ should be clear from most contexts.  We can see that {eq}`pnformula that 

$$ p_n'(x) =  \sum_{i=0}^n L_i'(x) f(x_i), \qquad p_n''(x) =  \sum_{i=0}^n L_i''(x) f(x_i)$$

and

$$ L_i^n(x) =  \frac{\frac{w_i}{x-x_i}}{\sum_{i=0}^n \frac{w_i}{x-x_i}}.$$

So to work out the derivative estimates we need to compute $L_i'(x)$ and $L_i''(x)$ for all $i$ (note that these still depend on $n$ but $n$ is the same for all in a given application).  