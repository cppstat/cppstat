---
execute: false
---

## What It Does

Most functions in the `<cmath>` and `<complex>` headers are made `constexpr`.

## Why It Matters

Historically, while arithmetic expressions such as addition and multiplication
between floating-point operands could be used in constant expressions,
common mathematical functions such as `std::sqrt` or `std::pow` couldn't be.
This meant that some computations had to be done unnecessarily at runtime,
or the user had to re-implement a `constexpr` version
of the `<cmath>`functions themselves.
The same applies to the corresponding functions in `<complex>`.

## Example

```cpp
#include <cmath>
#include <complex>

constexpr double sqrt_10 = std::sqrt(10);
constexpr std::complex<double> i = std::sqrt(std::complex<double>(-1));
```
