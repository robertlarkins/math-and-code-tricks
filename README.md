# math-and-code-tricks
Math and code tricks

## Math Tricks

### [Divisibility Rules](https://en.wikipedia.org/wiki/Divisibility_rule)

#### Three

An integer is divisible by 3 if its digits sum to 3, 6 or 9.
Continually sum the digits until there is only a single digit left, if this is a 3, 6, or 9, then the original number is divisible by 3.

E.g.:
```
16842
1 + 6 + 8 + 4 + 2 = 21
2 + 1 = 3
∴ 16842 is divisible by 3

9472
9 + 4 + 7 + 2 = 22
2 + 2 = 4
∴ 9472 is not divisible by 3
```

#### Nine

An integer is divisible by 9 if its digits sum to 9.
Continually sum the digits until there is only a single digit left, if this is 9, then the original number is divisible by 9.

E.g.:
```
93874
9 + 7 + 8 + 7 + 5 = 36
3 + 6 = 9
∴ 93874 is divisible by 9

1565
1 + 5 + 6 + 5 = 17
1 + 7 = 8
∴ 1565 is not divisible by 9
```

### Log Values

The following is how `log` values work.

 - *log*<sub>base</sub>(result) = power
 - base<sup>power</sup> = result

This can then be written as log<sub>*b*</sub>(*a*) = *c* and rearranged to look like *b*<sup>*c*</sup> = *a*, where
 - *a* = argument
 - *b* = base
 - *c* = exponent
 
E.g:

10<sup>2</sup>=100  
*log*<sub>10</sub>(100) = 2

