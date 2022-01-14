# Closed-form Equation Derivation

If a mathematical problem can be directly solved using an equation, then this equation is a closed-form solution of the mathematical problem.
That is, it can solve the given problem in terms of functions and mathematical operations.
Eg: sum of values from 1 to `n` has the closed-form solution `n(n+1)/2`.

**Terminology**
The term _general formula/form/solution_ might also be used for this.

See Also:
- https://brilliant.org/wiki/closed-form-expressions/
- https://math.stackexchange.com/questions/38155/what-is-the-difference-between-equation-and-formula

## Derivation

If we wanted to derive the closed-form equation for say a summation, such as `1+2+3+...+n`,
the first step would be to search on google and see if it has already been solved.

If a solution doesn't already exist online then the next step is to determine what type the equation might be
- linear - `y=mx+b` where `m` & `b` are some constant numbers and `y` is a variable which changes with the `x` variable. When graphed they will produce a single line
- quadratic - <code>ax<sup>2</sup>+bx+c=0</code>
- cubic - <code>ax<sup>3</sup>+bx<sup>2</sup>+cx+d=0</code>
- polynomial - this equation type covers any equation that has `x` to an exponent of some level. - <code>2x<sup>5</sup>+2x/3+7</code>
- rational
- radical

***How to determine which equation might fit your problem?***

### Example

Deriving the closed-form equation for `1+2+3+...+n` can be done by determining that this is a quadratic equation.
That is the closed-form equation will have a structure of <code>an<sup>2</sup>+bn+c</code>.
We can then supply some examples from what we are trying to solve:
<pre><code>3<sup>2</sup>+b3=6
a4<sup>2</sup>+b4=10
</code></pre>

Then we can solve these simultaneous equations, first by solving for `a`:

See Also:
- https://math.stackexchange.com/questions/183316/how-to-get-to-the-formula-for-the-sum-of-squares-of-first-n-numbers

## Induction

