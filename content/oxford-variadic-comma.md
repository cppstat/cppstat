---
execute: false
---

## What It Does

This proposal makes use of a variadic ellipsis (`...`) deprecated,
if it is not separated by a comma from previous parameters.

## Why It Matters

Without a separating comma,
users can confuse whether `...` refers to a parameter pack
or to "C-style" variadic arguments.
The lack of a comma also prevents using syntax such as `int...` for new features,
without changing the meaning of existing code.

## Example

```cpp
void f(int...);        // deprecated
void g(int, ...);      // OK, also valid in C
void h(...);           // OK, also valid in C

void i(auto...);       // OK, declares a function parameter pack
void j(auto......);    // deprecated
void k(auto..., ...);  // OK
```
