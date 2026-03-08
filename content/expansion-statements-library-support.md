---
execute: true
---

## What It Does

`std::integer_sequence` can now be used in `template for`
to conveniently write a loop where the loop index is a `constexpr` variable,
as well as to create a `constexpr` pack using structured bindings.
This is made possible by specializing `std::tuple_size`, `std::tuple_element`, and `std::get`
for `std::integer_sequence`.

## Why It Matters

These changes remove boilerplate code such as creating an immediately invoked lambda expression
when creating a `constexpr` pack of indices or writing a loop with a `constexpr` index.
This is especially important because C++26 supports pack indexing,
where a `constexpr` index is needed.

## Example

```cpp
#include <utility>
#include <tuple>
#include <print>

int main() {
    auto tup = std::tuple{1, 3.14, "hello"};

    // Output:
    //   tup[0] = 1
    //   tup[1] = 3.14
    //   tup[2] = hello
    template for (constexpr std::size_t I : std::make_index_sequence<3>()) {
        std::println("tup[{}] = {}", I, std::get<I>(tup));
    }

    constexpr auto [...Is] = std::make_index_sequence<3>();
}
```
