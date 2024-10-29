## math

## isclose
```
math.isclose(a, b, rel_tol, abs_tol)
```

- rel_tol = value
  - Optional. The relative tolerance. It is the maximum allowed difference between value a and b. Default value is 1e-09

- abs_tol = value
  - Optional. The minimum absolute tolerance. It is used to compare values near 0. The value must be at least 0

Compare with 0:
```
math.isclose(a, 0, abs_tol=1e-9)
```
