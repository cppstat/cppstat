---
execute: false
---

## What It Does

`&&` and `const&` references can bind to temporary objects.
With this change, this is no longer permitted when binding the result of a function
to a temporary object.

## Why It Matters

Binding the result of a function to a temporary object creates a reference that immediately dangles.
Disallowing it prevents the user from running into undefined behavior by accident.

## Example

```cpp
int&& f1() {
    return 42;  // error
}
const double& f2() {
    static int x = 42;
    return x;   // error
}

auto&& id(auto&& r) {
    return static_cast<decltype(r)&&>(r);
}
int&& f3() {
    return id(42);  // OK, but probably a bug
}
```
