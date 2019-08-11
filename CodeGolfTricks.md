# Tricks for Code Golf

## Alternate between numbers

### 0 and 1

```
i = ~-i*-1;
i = Math.Abs(i-1);
```

### i and -i

```
i = -i;
```

### 0 and x

```
i = Math.Abs(i-x);
```

### i and i+2

```
i = x;
i ^= 2;
```
E.g.:
```
i = 1;
i ^= 2;
```
