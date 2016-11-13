---
layout: mathpost
title: Not A Calculus Problem
date: 2016-11-13
---

I was watching [a lecture on calculus by David Jerison][video] and this particular problem caught my attention (18:45): A wire of fixed length hangs between two fixed points. On the wire is a weight that can slide freely along it. _At what point will the weight settle due to gravity?_

![Figure 1]

Prof. Jerison solves it as a differential minimisation problem.  Conceptually it's a nice demonstration of calculus technique, but the equations are unwieldy, and in the end we aren't even given the coordinates of the point because it's _"just a mess."_

We can do better.  The problem doesn't require calculus.  The point can be easily constructed with Euclidean geometry, and the coordinates are  straightforward to calculate with basic algebra.  No mess.


Physical Intution
-----------------

Call the wire's two fixed points **O** and **P**, and call the length ___l___.  Presume that the weight is hanging at a point **R**, and the wire is taut.

Consider a horizontal line through  **R**.  The weight will settle at the lowest possible point. I say that this happens when the distance <len> ORP </len> = ___l___ and <len> ORP </len> is minimal w.r.t. any point on that line. For if there were a point **S** on the line such that <len> OSP </len> \< <len> ORP </len> then **R** could be shifted toward **S** creating slack in the line, which would allow it to fall down further.

![Figure 2]


Constructing the Solution
-------------------------

Draw a circle with center **O** and radius ___l___.  
Draw a vertical line through **P**.  It meets the circle in a point **Q**.  
Draw the perpendicular bisector to the segment **PQ**. It's horizontal since it's perpendicular to a vertical line.  
Draw the segment **OQ**. It meets the bisector in the point **R**.  
Draw the segment **RP**.

![Figure 3]

**R** is the point where the weight settles, for <len> ORP </len> = _l_ <sup>[1]</sup>, and <len> ORP </len> is minimal w.r.t. the horizontal line <sup> [2] </sup>.

The proof for <sup>[1]</sup> is similar triangles. The proof for <sup>[2]</sup> is the triangle inequality. The details are left as an exercise for the reader.


The Algebra
-----------

Finding the coordinates of **R** is an exercise in basic coordinate geometry. Since this is not a good format for presenting all the details it will suffice to give an account of the order of inference, and the result.

Without loss of generality, say that **O** = $$(0,0)$$ and **P** = $$(a,b)$$.  
The equation of the vertical line is  $$ x = a $$.  
The equation of the circle is  $$ x^2+y^2=l^2 $$.  
The point **Q** is  $$ (a,i) $$.  Find  $$ i $$   from the circle equation; _x_ is known.  
The horizontal line has equation  $$ y = h $$  for some number  $$ h $$.  
To find  $$ h $$,  note that the point  $$ {P+Q} \over 2 $$  lies on the line.  
The radial line **OQ** has equation  $$ y = \frac{i}{a} x $$.  
Find the _x_-coordinate of **R** from the radial line equation; _y_ is known.  
In conclusion,  

$$ 
\begin{align*} 
R &= \left( \frac{a h}{i},  h \right) \\
\text{where} & \\
i &= -\sqrt{l^2-a^2} & \\
h &= \frac{b+i}{2} & \\
\end{align*} 
$$




[video]: https://youtu.be/sRIDVAcoG5A?t=18m45s
[Figure 1]: {{ site.url }}/blog/figures/2016-11-13-fig1.jpg
[Figure 2]: {{ site.url }}/blog/figures/2016-11-13-fig2.png
[Figure 3]: {{ site.url }}/blog/figures/2016-11-13-fig3.png
