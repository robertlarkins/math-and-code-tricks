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
the first step would be to search on google or [oeis.org](https://oeis.org) and see if it has already been solved.

If a solution doesn't already exist online then the next step is to determine what type the equation might be
- linear - `y=mx+b` where `m` & `b` are some constant numbers and `y` is a variable which changes with the `x` variable. When graphed they will produce a single line
- quadratic - <code>ax<sup>2</sup>+bx+c=0</code>
- cubic - <code>ax<sup>3</sup>+bx<sup>2</sup>+cx+d=0</code>
- polynomial - this equation type covers any equation that has `x` to an exponent of some level. eg: <code>2x<sup>5</sup>+2x/3+7</code>
- rational
- radical
- exponential

***How to determine which equation might fit your problem?***
- https://math.stackexchange.com/questions/183316/how-to-get-to-the-formula-for-the-sum-of-squares-of-first-n-numbers

Then we would want to find the constants of the equation that give us the closed-form equation. This is best demonstrated using worked examples.

### Worked Example: <code>1+2+3+&#x2026;+n</code>

Deriving the closed-form equation for `1+2+3+...+n` (also known as triangular numbers) can be done a couple of ways.

#### Approach 1

The most common approach is seeing that adding the first and last number together is the same as adding the second and second to last number, etc.
eg: `n=6` then `1+2+3+4+5+6` gives `(1+6) + (2+5) + (3+4)`
With each of these groups being equal with the value `n+1`. Then we just need to figure out the number of groups. When `n` is even there are `n/2` groups.
So this gives `(n+1)(n/2)`, which can be rearranged into the form `n(n+1)/2`.  
For the odd case we essentially still have `n/2` groups, as the middle value is half of `n+1`.  
eg: if `n=5` then we have `1+2+3+4+5`, giving `(1+5) + (2+4) + 3`.
This is then `(n+1) + (n+1) + (n+1)/2`
If this is rearranged we get `(n+1)(1+1+1/2) = (n+1)(2.5)` which given `n=5` can be formed as `(n+1)(n/2)`.
This equations is the same as the even case, and it works because the number of groups is `n/2` for when `n` is both even and odd. 

#### Approach 2

A general way of deriving the closed-form equation is first determining that it is a quadratic equation.
As far as I'm aware there isn't a specific way of determining the equation type of the closed-form solution.
In this particular case one way to determine it is quadratic is to think of how summation might be laid-out.
If `n=5` we would have this sort of layout

```
    1 2 3 4 5
1 | x
2 | x x
3 | x x x
4 | x x x x
5 | x x x x x
```
where we are summing each `x`. This shows that the summation has some form of <code>n<sup>2</sup></code>.

That is the closed-form equation will have a structure of <code>an<sup>2</sup>+bn+c</code>.
We can then supply some examples for given cases of `n` and then work through solving these simultanious equations:
<pre><code>a3<sup>2</sup> + b3 = 6
a4<sup>2</sup> + b4 = 10
</code></pre>

Then we can solve these simultaneous equations, first by solving for `a`:  
Using the _elimination method_ we do the following:
```
 9a + 3b =  6
16a + 4b = 10
```

then get the `b` components equal:
```
4*( 9a + 3b =  6) = 36a + 12b = 24
3*(16a + 4b = 10) = 48a + 12b = 30
```

subtract one equation from the other:
```
  48a + 12b = 30
-(36a + 12b = 24)
-----------------
  12a       =  6
```

Then rearrange for `a`, giving
```
12a = 6
  a = 6/12
  a = 1/2
```

Using the found value for `a`, we can substitute it back into one of the original equations and solve for `b`
```
16a     + 4b = 10
16(1/2) + 4b = 10
      8 + 4b = 10
          4b = 10 - 8
          4b = 2
           b = 2/4
           b = 1/2
```

Using the found values for `a` and `b`, we can then give the closed-form equation as
<code>(1/2)n<sup>2</sup> + (1/2)n</code>

Which can be rearranged into the more well known form:
<pre><code>(1/2)n<sup>2</sup> + (1/2)n
(1/2)(n<sup>2</sup> + n)
(n<sup>2</sup> + n)/2
n(n + 1)/2
</code></pre>


### Worked Example: `0,1,1,3,5,11,21,...`

This series comes from [this math.stackexchange question](https://math.stackexchange.com/questions/1068367/finding-a-closed-form-formula-for-a-sequence-that-is-defined-recursively), which provides the following definition for the series:

<pre><code>a<sub>0</sub> = 0, a<sub>1</sub> = 1
a<sub>n</sub> = a<sub>n-1</sub> + 2a<sub>n-2</sub>
for n &#x2265; 2
</code></pre>

The following description is based on [this answer](https://math.stackexchange.com/a/3525522).

Equations with a linear recurrence relation (an equation that relates a term in a sequence to previous terms using recursion) can be deemed exponential if they have constant coefficients that produce exponentially growing terms (the `a` values) in the sequence.

This recursive series has the following set of values up to `n = 6`:

| `n` | <code>a<sub>n</sub></code> |
| --- | ---                        |
| 0   | 0                          |
| 1   | 1                          |
| 2   | 1                          |
| 3   | 3                          |
| 4   | 5                          |
| 5   | 11                         |
| 6   | 21                         |

Because `n` gets used on each recursion and the original coefficients are constant, the closed form equation will be some type of exponential function <code>r<sup>n</sup></code> that relates to `n`. So to begin, let

<code>a<sub>n</sub> = r<sup>n</sup></code>

for some constant `r`, which is to be determined. As we have the recurrence relation, we can substitute it in

<code>r<sup>n</sup> = r<sup>n-1</sup> + 2r<sup>n-2</sup></code>

We then solve for `r`, which as <code>r<sup>n-1</sup></code> is not zero for `n = 2`, we can divide both sides of the equation by it.

<pre><code>r<sup>n</sup>/r<sup>n-2</sup> = (r<sup>n-1</sup> + 2r<sup>n-2</sup>)/r<sup>n-2</sup>
r<sup>n-(n-2)</sup> = r<sup>n-1-(n-2)</sup> + 2r<sup>n-2-(n-2)</sup>
r<sup>2</sup> = r + 2
r<sup>2</sup> - r - 2 = 0
</code></pre>

This is a quadratic equation that can be rewritten in the form

`(r - 2)(r + 1) = 0`

which shows that `r` has two solutions, `-1` and `2`. Because of this, <code>a<sub>n</sub> = r<sup>n</sup></code> is some linear combination of the powers of each root (`-1` and `2`), meaning

<code>a<sub>n</sub> = c<sub>0</sub>(-1)<sup>n</sup> + c<sub>1</sub>(2)<sup>n</sup></code>

where <code>c<sub>0</sub></code> and <code>c<sub>1</sub></code> are the linear coefficients, and can be found using simultanious equations

<pre><code>a<sub>0</sub> = c<sub>0</sub>(-1)<sup>0</sup> + c<sub>1</sub>(2)<sup>0</sup>
0 = c<sub>0</sub> + c<sub>1</sub>
c<sub>0</sub> = -c<sub>1</sub>

a<sub>1</sub> = c<sub>0</sub>(-1)<sup>1</sup> + c<sub>1</sub>(2)<sup>1</sup>
1 = -c<sub>0</sub> + 2c<sub>1</sub>
c<sub>0</sub> = 2c<sub>1</sub> - 1

-c<sub>1</sub> = 2c<sub>1</sub> - 1
-2c<sub>1</sub> - c<sub>1</sub> = -1
-3c<sub>1</sub> = -1
c<sub>1</sub> = 1/3
c<sub>0</sub> = -1/3
</code></pre>

The coefficients <code>c<sub>0</sub> = -1/3</code> and <code>c<sub>1</sub> = 1/3</code> can now be substituted into the original equation:

<pre><code>a<sub>n</sub> = c<sub>0</sub>(-1)<sup>n</sup> + c<sub>1</sub>(2)<sup>n</sup>
a<sub>n</sub> = (-1/3)(-1)<sup>n</sup> + (1/3)(2)<sup>n</sup>
a<sub>n</sub> = (2<sup>n</sup> - (-1)<sup>n</sup>)/3
</code></pre>


## Mathematical Induction

Mathematical induction is a way of proving that an equation holds true for every natural number.  
There are examples online of mathematical induction being applied, so an example wont be demonstrated here.
