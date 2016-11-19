---
layout: mathpost
date: 2016-11-19
---


The __Euler Transform__ is a technique for accelerating the convergence of an alternating series. To motivate the value of the technique, I'll demonstrate it's power with an example. 

The natural logarithm of 2 is equal to the series 

$$
ln(2) = 1 - {1 \over 2} + {1 \over 3} - {1 \over 4} + \dots
$$

It should be clear that calculating the value of this series to 6 decimal places requires us to take *at least* 1,000,000 terms. With the Euler Transform we get 6 decimal places after only 18 terms, and 16 digits after about 50 terms.

Note however that it does not *always* converge quicker. Particularly if the series is already converging rather rapidly, such as for example the Taylor series of $$ sin(x) $$. 

The point of this post is to make this calculation even more efficient by reducing it's time complexity from $$ O(n^2) $$ to $$ O(n) $$.

## Euler Transform

Given an alternating series

$$
S \ =\  \sum_{k=0}^{\infty} (-1)^k a_k 
  \ =\  a_0 - a_1 + a_2 - a_3 + \dots  
$$

the Euler Transform of $$ S $$ is defined as

$$
S^T  \ =\  \sum_{n=0}^\infty {\nabla_n \over 2^{k+1}} 
$$

where

$$
\nabla_n  \ =\  \sum_{k=0}^n (-1)^k {n \choose k} a_k
$$

I don't know about you, but when I see a formula like that, my eyes glaze over. It's mostly just a jumble of symbols. So to get a better idea of what's going on, we'll expand the first few terms of the sums and see if we can't get some intuition about it. Starting by expanding the first few $$ \nabla_n $$.

$$
\begin{align}
    & \nabla_0 = 1 a_0  \\
    & \nabla_1 = 1 a_0 - 1 a_1  \\
    & \nabla_2 = 1 a_0 - 2 a_1 + 1 a_2  \\
    & \nabla_3 = 1 a_0 - 3 a_1 + 3 a_2 - 1 a_3  \\
\end{align}
$$

OK. Simple enough. Just Pascal's triangle with $$a_n$$ and alternating signs mixed in.

Now to get some sense for $$ S^T $$ let's calculate some partial sums. The $$ p^{\rm th} $$ partial sum is denoted $$ S^T_p $$

$$
\begin{alignat}{2}
    & S_0^T =         {1 \over 2} \nabla_0 \; &&= \; {1 \over 2}  a0             \\
    & S_1^T = S_0^T + {1 \over 4} \nabla_1 \; &&= \; {1 \over 4} (3a_0-a_1)      \\
    & S_2^T = S_1^T + {1 \over 8} \nabla_2 \; &&= \; {1 \over 8} (7a_0-4a_1+a_2) \\
\end{alignat}
$$

On paper I'd do more, but let's focus more on the ideas here.


## Linearizing

The idea is that although $$ S^T $$ is defined as a double sum, where calculating $$ S_p^T $$ requires on the order of $$ p^2 $$ operations, we can see that it reduces algebraically to a single sum with an order of $$ p $$ operations. If only we could calculate the constant factors of $$ a_n $$ directly.

Tentatively we can rewrite the transform as

$$
S_p^T  \ =\  {1 \over 2^{p+1}} \sum_{k=0}^p (-1)^k c_{pk} a_k
$$

Now the problem is to find an expression for the constants $$ c_{pk} $$. Here's a table with some more values calculated by hand. I'll also include $$ 2^{p+1} $$ in the first column, for reasons which will become clear.

$$
\begin{array}{r|r|lllll}
    p & 2^{p+1} & c_{p0} & c_{p1} & c_{p2}& c_{p3} & c_{p4} \\
    \hline
    0 &  2 & 1  &   &   &   &    \\
    1 &  4 & 3  & 1 &   &   &    \\
    2 &  8 & 7  & 4 & 1 &   &    \\
    3 & 16 & 15 &11 & 5 & 1 &    \\
    4 & 32 & 31 &26 &16 & 6 & 1  \\
\end{array}
$$

Do you see the pattern yet? I didn't. One go-to technique in these situations is to make a diffrence table, so let's do that for row 4

$$
\begin{array}{c|ccc}
    c_{4 k} & 32 && 31 && 26 && 16 && 6 && 1 && 0  \\
    \hline
    \Delta    && -1 && -5 &&-10 &&-10 &&-5 &&-1 &   \\
\end{array}
$$

Would you look at that. The 5th row of Pascal's triangle popped out! That's great. You can check that it holds for all the other rows as well. Now we can define $$ c_{pk} $$ as a cumulative difference of a Pascal's triangle row from $$ 2^{p+1} $$.

$$
c_{pk} = 
\begin{cases}
    2^{p+1}                      & \text{if } k < 0         \\
    c_{p k-1} - {p+1 \choose k}  & \text{if } 0 \leq k \leq n  \\
\end{cases}
$$

The formal proof is not too advanced, just induction. But it's induction in two variables with multiple edge cases to fix, and this post is already long enough, so I'll leave it out.

## Conclusion

That's it. Our refactored formula for $$ S^T_p $$ is now $$ O(n) $$.

To get some appreciation for this result, let's apply it to calculating digits of $$ \pi $$. We're using Python's `decimal` module, and Leibniz's famous series. 
[Source code here.](https://github.com/rmj86/algos/tree/master/eulertransform)

$$
\pi \ =\  1 - {1 \over 3} + {1 \over 5} - {1 \over 7} + {1 \over 9} - \dots
$$

Getting 100 digits of $$ \pi $$ out of this series would require us to add up somewhere on the order of $$ 10^{100} $$ terms. Good luck! With Euler Transform, it gives us a little more than 100 digits per 350 terms (empirical). On my machine, getting $$ n $$ digits with the original versus the refactored formula takes so much time:

 # digits  |  # terms  |  Original  |  Refactored
-----------|-----------|------------|--------------
  100      |  350      |  6.8 s     |  0.11 s 
  200      |  700      |  54 s      |  0.42 s
  500      |  1750     |  1600 s    |  5.1 s
  1000     |  3500     |  ????      |  40 s
  
The empirical performance is much worse than $$ O(n^2) $$ and $$ O(n) $$. That's accounted for by the fact that as we increase precision, the cost of each arithmetic operation increases, whereas the above analysis is only concerned with the number of operations.
