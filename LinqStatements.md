# Linq Statements

## `FirstOrDefault` vs `Find`

This comes from a Pull Request comment I made.

There are two areas at play here, readability and efficiency. I'll start with efficiency.
Linq statements can at times be particularly inefficient, especially when chaining.
So I would expect that using `Where(...).ToList().Find(...)` would be slower than `FirstOrDefault(...)` (though at the scale we are working at there is likely no measurable difference).
`FirstOrDefault` can be used on IEnumerables as you have found, where as `Find` can only be used on Lists.
This likely doesn't matter in this case as I'm guessing `inspections` is a List.
Other than this, `Find` and `FirstOrDefault` behave the same, though reading online `Find` can be faster.

Readability wise, reducing the number of links in the Linq statement tends to improve readability,
so `Where(...).ToList().Find(...)` can be reduced to just a `FirstOrDefault(...)` or just a `Find(...)` as the `Where` and `Find` conditionals can be combined.
What I like (my subjective opinion) with `FirstOrDefault` is that its name is explicit with what it is doing, it only returns the First item found,
otherwise it returns the default, while `Find` does the same thing, that is less clear unless you recall how `Find` operates.

I'm less concerned about whether you choose `FirstOrDefault` vs `Find`, it is more that the `Where(...).ToList()` is unnecessary.

Efficiency comparison links:
- https://stackoverflow.com/questions/9335015/find-vs-where-firstordefault
- https://stackoverflow.com/questions/14032709/performance-of-find-vs-firstordefault
