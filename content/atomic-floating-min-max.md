---
execute: true
---

## What It Does

The `fetch_max`, `fetch_min`, `fetch_fmaximum`, `fetch_fminimum`, `fetch_fmaximum_num`, and `fetch_fminimum_num`
member functions for `std::atomic` and `std::atomic_ref` make it possible to atomically
take the minimum or maximum of two floating-point numbers.
`std::fmaximum`, `std::fmaximum_num`, `std::fminimum`, and `std::fminimum_num`
are also added to `<cmath>`, providing non-atomic counterparts.

## Why It Matters

By utilizing hardware support,
floating-point minimum and maximum can have much better performance than existing code
which uses `compare_exchange_weak` in a loop,
especially with high contention.
The new `<cmath>` functions imported from C23 are also generally useful,
and implement operations from the ISO/IEC 60559 standard.

## Example

```cpp
#include <atomic>
#include <print>

std::atomic<double> x = 0.5;

int main() {
    std::println("{}", x.fetch_max(0)); // prints 0.5, no update
    std::println("{}", x.fetch_max(1)); // prints 0.5, x = 1
    std::println("{}", x.load());       // prints 1
}
```
