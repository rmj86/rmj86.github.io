---
layout: mathpost
title: Rows of Binomial Coefficients in Linear Time
date: 2016-11-15
---

So you want to calculate a binomial coefficient. What do you do? If you're in a hurry you might blindly use the standard factorial formula,

$$
{n \choose r} = \frac {n!} {r!(n-r)!}
$$

It's quick and dirty, but not very efficient, requiring roughly $$2n$$ multiplications. If you have som experience calculating these things, you'll know to cancel out all the terms of $$(n-r)!$$ from the $$n!$$ and then multiply what's left over. I.e.

$$
{n \choose r} = \frac {n(n-1)(n-2) \dots (n-r+1)}
                      {1 \times 2 \times 3 \times \dots \times r}
              = \prod_{i=1}^r \frac {n-r+1} {i}
$$

This product formula requires about $$2r$$ multiplications. Good savings, particularly if $$r$$ is small. But it's still $$O(n)$$ in the general case; The average size of $$r$$ is $$n \over 4$$ (if you're clever) or $$n \over 2$$ (if you're not).

Now suppose you want to calculate _all_ the coefficients for $$n$$ in one go. I.e. you want to know the value of

$$
{n \choose 0},
{n \choose 1},
{n \choose 2},
\dots , 
{n \choose n-1},
{n \choose n}
$$

How much work is it? Calculating a single term is $$O(n)$$ and there are $$n$$ terms, so the total time complexity is $$O(n^2)$$.

Another common method for this case is to calculate rows of Pascal's triangle. It's usually my go-to method. Or rather it used to be. Pascals triangle also has time complexity $$O(n^2)$$ for calculating the $$n$$th row. Can we do better?

Yes! The following identity may seem obvious, in particular if you have used the product formula before. So obvious no one ever feels the ned to mention it, perhaps. And indeed I never do see it mentioned anywhere. Yet this simple recurrence relation allows us to calculate the sequence in $$O(n)$$ time.

$$
{n \choose r+1} = 
{n \choose r} \times \frac{n-r}{r+1 }
                                                              \tag 1 \label 1
$$

To get som intuition for why it might be true, let's take a numeric example of the product formula:

$$
\begin{align}
{5 \choose 2} = \prod_{i=1}^2 \frac{6-i}{i}
    = {\frac 5 1} \times {\frac 4 2} &   \\
{5 \choose 3} = \prod_{i=1}^3 \frac{6-i}{i}
    = {\frac 5 1} \times {\frac 4 2} & \times {\frac 3 3}   \\
    = {5 \choose 2} & \times {\frac 3 3}

\end{align}
$$

Now let's prove it, using only the standard factorial formula. $$\eqref{1}$$ is equivalent to

$$
\frac {n \choose r+1} {n \choose r} = \frac{n-r}{r+1}
$$

We evaluate the left-hand side.

$$
\frac {n \choose r+1} {n \choose r}
= {n \choose r+1}  {n \choose r}^{-1}  \\
= \frac {n!} {(r+1)!(n-(r+1))!}  \frac {r!(n-r)!} {n!}
$$

When we refactor the denominator of the first fraction, almost everything cancels out

$$
= \frac {n!} {(r+1)r!  \frac{(n-r)!}{n-r}  }  \frac {r!(n-r)!} {n!}  \\
= \frac 1 {\frac {r+1} {n-r}} \\
= \frac {n-r} {r+1}
$$

Q.E.D.

Now let's have a go at an example to illustrate, calculating the $$4 \choose r$$ sequence:

$$
\begin{align*}
&\binom 4 0                                & = 1  \\
&\binom 4 1 = \binom 4 0 \times \frac 4 1  & = 4  \\
&\binom 4 2 = \binom 4 1 \times \frac 3 2  & = 6  \\
&\binom 4 3 = \binom 4 2 \times \frac 2 3  & = 4  \\
&\binom 4 4 = \binom 4 3 \times \frac 1 4  & = 1  \\
\end{align*}
$$

We're really just doing the exact same thing as if we were calculating a single coefficient, but we get the whole row of them _for free_ by _saving the intermediate products._ 

Not sure if it's an important theorem on it's own, but it will come up in a future post where I show that the Euler Transform of an alternating series can be done in linear time (whereas the standard formulation is quadratic time).
