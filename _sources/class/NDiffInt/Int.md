(page_topic1)=
Numerical Integration
=======================

Numerical integration has a long history.  Older literature refers to this as numerical "quadrature".  Numerical differentiation is primarily used for cases where we have discrete data corresponding to measurements of our underlying function. While numerical integration is used in similar circumstances, it is also often a necessity even when we have a relatively simple analytic form for the underlying function due to the lack of an analytic form for most anti-derivatives.  For example, $f(x) = e^{-x^2}$ can be easily differentiated ($f'(x)=2x e^{-x^2}$) but lacks an analytic antiderivative.  Statistics textbooks still typically contain tables related to the antiderivative of this function that are obtained by numerical integration.

We will start our analysis in a similar manner to our discussion of numerical differentiation and discuss integration of data at equally spaced points.  However, after deriving low order formulas in this manner we will move onto other methods of deriving higher order formulas.  As before,  we replace $f(x)$ with its interpolatory function (Lagrange or splines) and take the derivative of that.  i.e.

$$ f(x) = p(x) + \epsilon(x), $$

where $p(x)$ is our interpolation and $\epsilon(x)$ is the error in the interpolation.  Integrating this gives

$$ \int_a^b f(x) dx= \int_a^b p(x) dx + \int_a^b \epsilon(x) dx.$$

Applying this to Lagrange interpolation we get  

$$
\begin{align}
\int_a^b f(x) dx &\approx \int_a^b \sum_{k=0}^n f(x_k) L_k(x) dx,\\
& = \sum_{k=0}^n f(x_k)  \int_a^b L_k(x) dx,\\
& = \sum_{k=0}^n a_k f(x_k),
\end{align}
$$ (LagrangeInt)

with  

$$
a_k=\int_a^b L_k(x) dx,
$$

and error  

$$
E =  \int_a^b  \frac{f^{n+1}(\xi)}{(n+1)!}\prod_{j=0}^n (x-x_j) dx.
$$  

**Example: Trapezoidal Rule.**  Here we use a linear interpolation between the endpoints so $x_0=a$ and $x_1=b$.  Integration of the Lagrange polynomials for this case gives  
 
$$
\begin{align}
a_0 &= \int_a^b L_0(x) dx,\\
&= \int_a^b \frac{x-x_1}{x_0-x_1} dx,\\
&= \frac{b-a}{2}.
\end{align}
$$  

A very similar integration for $L_1(x)$ gives  

$$
a_1 =\int_a^b L_0(x) dx = \frac{b-a}{2}.
$$

Putting this into Eq. {eq}`LagrangeInt` yields  

$$
\int_a^b f(x) dx \approx \frac{b-a}{2}\left(f(a)+f(b)\right).
$$

This is called the *trapezoidal* rule as the shape under the line we are integration over is a trapezoid.  To find the error,

$$
E =  \int_{x_0}^{x_1}  \frac{f''(\xi)}{2}(x-x_0)(x-x_1) dx.
$$

The difficulty here is that $\xi$ depends on $x$ in an unknown manner.  Noting that $(x-x_0)(x-x_1) \leq 0$ on $[x_0,x_1]$ we can make use of

````{dropdown} **The Weighted Mean Value Theorem**  

If $f(x)$ is continuously and $g(x)$ is an integrable function that does not change sign on $[a,b]$, then there exists $\eta \in (a,b)$ such that  

$$
\int_a^b f(x)g(x) dx = f(\eta)\int_a^b g(x)dx.
$$

````

to conclude that

$$
\begin{align}
E &= \frac{f''(\eta)}{2} \int_{x_0}^{x_1} (x-x_0)(x-x_1) dx,\\
&= - \frac{(x_1-x_0)^3}{12} f''(\eta),\\
&= - \frac{(b-a)^3}{12} f''(\eta),
\end{align}
$$

for some (unknown) $\eta\in (a,b)$.

Often quadrature formulas are characterized by the *degree of accuracy*.  This is defined as the largest integer $m$ for which the formula is exact for all polynomials of degree $\leq m$.  We see here that the trapezoidal rule has degree of accuracy of 1 as it is exact for all polynomials of degree $\leq 1$ (as the error term involves the second derivative).

To improve upon the trapezoidal rule (i.e. reduce the error) we could use more points and a higher degree polynomial.  However, we actually have another option which we will consider first.  In particular, we can break up the interval and apply the trapezoidal rule on each interval.  We will illustrate this next.

### Composite Integration

Let's assume that we are given data at equally spaced points $x_j=a+jh$ on the interval $[a,b]$, for $j=0,\cdots,n$ so $h=(b-a)/n$.  We break up the integral into $n$ separate intevals over each segment $(x_j,x_{j+1})$ and apply the trapezoidal rule to each segment:  

$$
\begin{align}
\int_a^b f(x) dx &= \int_{x^0}^{x_1} f(x) dx + \int_{x^1}^{x_2} f(x) dx + \cdots +\int_{x^{n-1}}^{x_n} f(x) dx,\\
&= \sum_{j=0}^{n-1} \int_{x_j}^{x_{j+1} f(x) dx,\\
&= \sum_{j=0}^{n-1} \frac{h}{2}(f(x_j)+f(x_{j+1})) - \frac{h^3}{12} \sum_{j=0}^{n-1} f''(\xi_j).
\end{align}
$$  

Note that the unknown $\xi_j$'s are each in a differnt subinterval $(x_j,x_{j+1})$. In additon, any given $f(x_i)$ appears in two terms in the first sum, except for the endpoints.  We can the rearrange the terms to get

$$
\begin{align}
\int_a^b f(x) dx 
&= \sum_{j=0}^{n-1} \frac{h}{2}(f(a) + 2 \sum_{j=1}^{n-1} f(x_j)+f(b)) - \frac{(nh)h^2}{12} \frac{\sum_{j=0}^{n-1} f''(\xi_j)}{n}.
\end{align}
$$

The sum in the error term is the mean value of $n$ values of $f''(x)$ at different values in $(a,b)$.  It is not too hard to argue that, if $f''(x)$ is continuous on $[a,b]$, that this mean value is realized at some $x=\eta$.  As such, we can write this as

$$
\begin{align}
\int_a^b f(x) dx 
&= \frac{h}{2}(f(a) + 2 \sum_{j=1}^{n-1} f(x_j)+f(b)) - \frac{h^2(b-a)}{12} f''(\eta),
\end{align}
$$

where we also used the fact that $nh=(b-a)$.  



